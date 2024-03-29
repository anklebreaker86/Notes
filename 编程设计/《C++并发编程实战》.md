





# 第三章   在线程间共享数据

## 3.1 线程间共享数据的问题

多线程共享数据的问题多由数据改动引发，如果所有共享数据都是只读数据，就不会有问题

“不变量”（invariant）被广泛使用，它是一个针对某一特定数据的断言，该断言总是成立的

并发代码中一种最常见的错误成因：条件竞争（race condition）



1. **条件竞争**

在并发编程中，操作由两个或多个线程负责，它们争先让线程执行各自的操作，而结果取决于它们执行的相对次序，所有这种情况都是条件竞争



C++标准还定义了术语“数据竞争”（data race）：并发改动单个对象而形成的特定的条件竞争



诱发恶性条件竞争的典型场景是，要完成一项操作，却需改动两份或多份不同的数据

恶性条件竞争当应用程序在调试环境下运行时，常常会完全消失，因为调试工具影响了程序的内部执行时序



2. **防止恶性条件竞争**

有几种方法防止恶性条件竞争：

* 采取保护措施包装数据结构，确保不变量被破坏时，中间状态只对执行改动的线程可见
* 修改数据结构的设计及其不变量，由一连串不可拆分的改动完成数据变更，每个改动都维持不变量不被破坏，称为无锁编程
* 将修改数据结构当作事务（transaction）来处理，类似于数据库在一个事务内完成更新：把需要执行的数据读写操作视为一个完整序列，先用事务日志存储记录，再把序列当成单一步骤提交运行



# 3.2 用互斥保护共享数据

运用名为互斥（mutual exclusion，略作mutex）的同步原语（synchronization primitive）就能达到只要有线程正在运行标记的代码，任何别的线程意图访问同一份数据，则必须等待，直到该线程完事的效果



访问一个数据结构前，先锁住与数据相关的互斥；访问结束后，再解锁互斥

互斥本身也有问题，表现形式是死锁



1. **在C++中使用互斥**

在C++中，我们通过构造std::mutex的实例来创建互斥，调用成员函数lock()对其加锁，调用unlock()解锁



不推荐直接调用成员函数的做法

原因是，若按此处理，那我们就必须记住，在函数以外的每条代码路径上都要调用unlock()，包括由于异常导致退出的路径



C++标准库提供了类模板std::lock_guard<>

在构造时给互斥加锁，在析构时解锁，从而保证互斥总被正确解锁

```c++
#include <list>
#include <mutex>
#include <algorithm>

std::list<int> some_list;
std::mutex some_mutex;

void add_to_list(int new_value)
{
    std::lockguard<std::mutex> guard(some_mutex);
    some_list.push_back(new value);
}

bool list_contains(int value_to_find)
{
    std::lock_guard<std::mutex> guard(some_mutex);
    return std::find(some_list.begin(),some_list.end(),
                    value_to_find) != some_list.end();
}


```



代码中的全局变量some_list由对应mutex实例保护



大多数场景下的普遍做法是，将互斥与受保护的数据组成一个类

本例中的add_list()和list_contains()将被改写为类的成员函数，互斥和受保护的共享数据会变成类的私有成员

类所有成员函数在访问数据成员之前，都先对互斥加锁，并在完成访问后解锁，共享数据就可以很好地受到全方位保护



如果成员函数返回指针或引用，指向受保护的共享数据，那么即便成员函数都按良好、有序方式锁定互斥，仍然没有效果

因此若利用互斥保护共享数据，则需谨慎设计程序接口，从而确保互斥已先行锁定，再对受保护共享数据进行访问，保证不留后门



2. **组织和编排代码以保护共享数据**

* 使用互斥保护共享数据并不太简单，一旦出现游离的指针或引用，这种保护就形同虚设

* 还有另外一种情况，若成员函数在自身内部调用了别的函数，而这些函数不受我们掌控，那么也不得像他们传递这些指针或引用



不得向锁所在的作用域之外传递指针和引用，指向受保护的共享数据，无论是通过函数返回值将它们保存到对外可见的内存，还是将它们作为参数传递给使用者提供的函数



3. **发现接口固有的条件竞争**

双向链表例子中，要使线程安全删除节点，必须禁止三个节点的并发访问：正要删除的节点和它两侧的邻居。若仅保护各节点的指针，则与没有使用互斥差不多，条件竞争仍可能发生

最简单的办法是，单独使用一个互斥保护整个链表



虽然整个链表上的单一操作是安全行为，还是有可能遇到条件竞争



例如在栈中，empty()和size()的结果不可信。尽管，在某线程调用empty()或size()时，返回值可能是正确的

然而，一旦函数返回，其他线程就不再受限，从而能自由地访问栈容器，可能马上有新元素入栈，或者，现有的元素会立刻出栈，令前面的线程得到的结果失效而无法使用

如果没有共享栈容器，那么这是安全行为

```c++
stack<int> s;
if(!s.empty())
{
    int const value = s.top();
    s.pop();
}
```

根本原因在于函数接口，即使在内部用互斥保护栈容器中的元素，也无法防范



若两个线程都运行上述代码，还有可能两个线程都读取了同一个栈顶元素，却弹出了两个元素，导致第二个元素未被读取就弹出

这要求从根本上更改接口设计，一种改法是将top和pop组成一个成员函数，再采取互斥保护



C++设计者将stack的top和pop分开原因：

只有在栈被改动之后，弹出的元素才返回给调用者，然而在向调用者复制数据的过程中，有可能抛出异常

万一弹出的元素已从栈上移除，但复制却不成功，就会令数据丢失



消除条件竞争方法：

1. 传入引用

借一个外部变量接收栈容器弹出的元素

```c++
std::vector<int> result;
some_stack.pop(result);
```



缺点：

* 调用pop前，须先依据栈中元素型别构造一个实例
* 对于某些型别，构建实例的时间代价高昂或耗费资源过多，所以不太实用
* 在另一些型别的栈元素的内部代码中，其构造函数不一定带有参数，故此法也并不总是可行
* 最后，这种方法还要求栈容器存储的型别是可赋值的（assignable），许多用户定义的型别不支持赋值



2. 提供不抛出异常的拷贝构造函数，或不抛出异常的移动构造函数

假设pop()按值返回，若它抛出异常，则牵涉异常安全的问题只会在这里出现

对于线程安全的栈容器，一种可行的做法是限制其用途，只允许存储上述型别的元素按值返回，且不抛出异常



就用户定义的型别而言，其中一些含有会抛出异常的拷贝构造函数，却不含移动构造函数，另一些则含有拷贝构造函数和/或移动构造函数，而且不抛出异常。相比之下，前者数量更多



3. 返回指针，指向弹出的元素

优点是指针可以自由地复制，不会抛出异常



缺点：

在返回的指针所指向的内存中，分配的目标千差万别，既可能是复杂的对象，也可能是简单的型别，如int，这就要求我们采取措施管理内存，但这便构成了额外的负担，也许会产生超过按值返回相应型别的开销



若函数接口采取上述方法，则指针型别std::shared_ptr是不错的选择

避免了内存泄漏、内存分配策略由C++标准库管控



4. 结合方法1和方法2，或结合方法1和方法3



5. 类定义示例：线程安全的栈容器类

```c++
#include <exception>
#include <memory>

struct empty_stack: std::exception
{
    const chat* what() const noexcept;
};

template<typename T>

class threadsafe_stack
{
    public:
    threadsafe_stack();
    threadsafe_stack(const threadsafe_stack&);
    threadsafe_stack& operator = (const threadsafe_stack&) = delete;
    void push(T new_value);
    std::shared_ptr<T> pop();
    void pop(T& value);
    bool empty() const;
}
```

消除了接口的条件竞争

其成员函数pop()具有两份重载，分别实现了方法1和方法3



假如我们终须针对某项操作锁住多个互斥，那就会让一个问题藏匿起来，伺机进行扰乱：死锁

条件竞争是两个线程同时抢先运行，死锁则差不多是其反面：两个线程同时互相等待，停滞不前



4. **死锁**

有两个线程，都需要同时锁住两个互斥，才可以进行某项操作，但它们分别都只锁住了一个互斥，都等着再给另一个互斥加锁

于是，双方毫无进展，因为它们同在苦苦等待对方解锁互斥。上述情形称为死锁（deadlock）



防范死锁的建议通常是，始终按相同顺序对两个互斥加锁

若我们总是先锁互斥A再锁互斥B，则永远不会发生死锁



有时会出现棘手情况，例如，运用多个互斥分别保护多个独立的实例，这些实例属于同一个类

考虑一个函数，操作同一个类的两个实例，互相交换它们的内部数据，两个实例上的互斥都必须加锁

若选用固定次序（先给第一个实例的互斥加锁，再轮到第二个实例的互斥），前面的建议就适得其反

针对两个相同的实例，若两个线程都通过该函数在它们之间互换数据，只是两次调用的参数顺序相反，会导致它们陷入死锁



C++提供了std::lock函数，可以同时锁住多个互斥，而没有发生死锁的风险

```c++
class some_big_object;

void swap(some_big_object& lhs, some_big_object& rhs);

class X
{
    private:
    some_big_object some_detail;
    std::mutex m;
    
    public:
    X(some_big_object const& sd):some_detail(sd){}
    
    friend void swap(X& lhs, X& rhs)
    {
        if(&lhs == &rhs)
            return;
        std::lock(lhs.m, rhs.m);
        std::lock_guard<std::mutex> lock_a(lhs.m, std::adopt_lock);
        std::lock_guard<std::mutex> lock_b(rhs.m, std::adopt_lock);
        swap(lhs.some_detail, rhs.some_detail);
    }
}
```



* 一开始就对比两个参数，以确定它们指向不同实例
* 代码调用std::lock()锁定两个互斥，并依据它们分别构造std::lock_guard实例
* 提供了std::adopt_lock对象，以指明互斥已被锁住，即互斥上有锁存在



无论函数是正常返回，还是因受保护的操作抛出异常而导致退出，std::lock_guard都保证了互斥全都正确解锁

std::lock()在其内部对lhs.m或rhs.m加锁，这一函数调用可能导致抛出异常，这样，异常便会从std::lock()向外传播



假如std::lock()函数在其中一个互斥上成功获取了锁，但它试图在另一个互斥上获取锁时却有异常抛出，那么第一个锁就会自动释放：若加锁操作涉及多个互斥，则std::lock()函数的语义是“全员共同成败”



std:: scoped_lock<>和std::lock_guard<>完全等价，只不过前者是可变参数模板

```c++
void swap(X& lhs, X& rhs)
{
    if(&lhs == &rhs)
        return;
    std::scoped_lock guard(lhs.m, rhs.m);
    swap(lsh.some_detail, rhs.some_detail);
}
```











6. **运用std::unique_lock<>灵活加锁**









