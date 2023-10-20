# 第一章   开始

## 1.1 编写一个简单的C++程序

每个C++都包含一个或多个函数，其中一个必须命名为main，操作系统调用main运行C++程序

```c++
int main()
{ return 0; }
```



函数定义包括：

* 返回类型
* 函数名
* 形参列表
* 函数体



main返回类型指示状态，0表明成功，非零值由系统定义，指出错误类型。

程序文件称为源文件。



## 1.2 初识输入输出

iostream库包含输入流istream和输出流ostream。

标准输入cin，标准输出cout，标准错误cerr，一般性信息clog。



输入两个数，然后输出他们的和：

```c++
#include<iostream>

int main()
{
    std::cout << "Enter two numbers:" << std::endl;
    int v1 = 0, v2  = 0;
    std::cin >> v1 >> v2;
    std :: cout << "The sum of " << v1 << " and " << v2
        		<< " is " << v1 + v2 << std::endl;
    return 0;
}
```



尖括号中的名字指出了一个头文件。

使用输出运算符<<打印信息。

双引号包围的字符为==字符串字面值常量==。

endl为操纵符，效果是结束当前行，并将缓冲区中内容刷到设备中。

::为作用域运算符，指出定义在某命名空间中的名字。

​	>>为输入运算符。



## 1.3 注释简介

* 单行注释，使用==//==，换行符结束
* 界定符注释，使用==/*==和==*/==，不能嵌套。



## 1.4 控制流



1. **while语句**

while语句重复执行一段代码，直到给定条件为假为止。



2. **for语句**

for语句包含两部分，循环头和循环体。

循环头控制循环次数，由三部分组成：初始化语句，循环条件和表达式。



3. **读取数量不定的输入数据**

使用istream对象作为条件时，效果是检测流的状态。

```c++
while(std::cin >> value)
```

流有效则检测成功；当遇到文件结束符或遇到无效输入时，检测无效。



## 1.5 类简介

在C++中，通过定义类来定义自己的数据结构。

成员函数是定义为类的一部分的函数，有时也被称为方法。



# 第二章   变量和基本类型



## 2.1 基本内置类型

C++定义了包括==算术类型==和==空类型==在内的基本数据类型。

算术类型包括：

* 字符
* 整型数
* 布尔值
* 浮点数



1. **算术类型**

算术类型分两类，整型和浮点型，尺寸在不同机器上有所差别，C++标准规定尺寸的最小值。

布尔型取值是true或false。

基本字符类型是char，一个char类型大小和一个机器字节一样。

==wchar_t==类型用于确保可以存放机器最大扩展字符集中的任意一个字符；==char16_t==和==char32_t==为Unicode字符集服务。



通常，float以一个字来表示，double以两个字来表示。

除布尔型和扩展字符型外，其他整型可分为==signed==和==unsigned==两种。



2. **类型转换**

* 0对应false，否则为true；false对应0，true对应1
* 浮点数转整数仅保留小数点前部分
* 整数转浮点数，小数部分为0
* 赋值超过无符号类型范围时，结果是初始值对无符号类型表示数值总数取模后余数
* 赋值超过带符号类型范围时，结果是未定义的



unsigned变量作为判断条件时，永远大于0可能出现死循环。



3. **字面值常量**

形如42的值被称为字面值常量。

0开头表示八进制，0x开头表示十六进制。

由单引号括起来的一个字符称为char型字面值，双引号括起来的为字符串型字面值。

字符串字面值实际为常量字符构成的数组，且末尾添加'\0'。



两类字符不能直接使用：

* 不可打印字符，如退格或其他字符
* 有特殊含义字符，使用转义序列



添加前缀和后缀可以改变整型、浮点型和字符型字面值的默认类型。



## 2.2 变量

1. **变量定义**

变量定义基本类型：首先是==类型说明符==，跟着一个或多个变量名组成的列表。

当对象在创建时获得了一个特定的值，则称改对象被初始化了。



初始化方式，使用列表初始化{}不会执行转换，使用（）会执行转换。

```c++
int val = 0;
int val = {0};
int val{0};
int val(0);
```



定义变量时未指定初值，则变量被默认初始化。

内置类型变量，定义在函数体外初始化为0，定义在函数体内不被初始化。



2. **变量声明和定义的关系**

C++支持==分离式编译==，允许将程序分割为若干个文件，每个文件可被独立编译。

==声明==使得名字为程序所知，==定义==负责创建与名字关联的实体。



声明规定了变量的类型和名字；定义还申请了存储空间，也可能会为变量赋初始值。

若想声明变量而非定义它，使用关键字==extern==，且不要显式初始化，显式初始化后即为定义。



变量只能被定义一次，但是可以被多次声明。



3. **标识符**

命名规范：

* 能体现实际含义
* 变量名一般用小写字母
* 自定义类名一般大写字母开头
* 单词间应有明显区分



4. 名字的作用域

大多数作用域以花括号分隔。

作用域可以嵌套，允许在内层作用域中重新定义外层作用域已有名字，全局作用域本身没有名字，使用::可以获得全局变量。



## 2.3 复合类型

复合类型是指基于其他类型定义的类型，如引用和指针。

1. **引用**

引用类型为对象起了另外一个名字，将声明符写成&d的形式来定义引用类型，d是声明的变量名。

```c++
int val = 1024;
int &refVal = val; //refVal指向val，是val的另一个名字
int &refVal2; //错误，引用必须被初始化
```



==一般初始化变量时，初始值会被拷贝到新建的对象中；定义引用时，程序把引用和初始值绑定在一起，而不是拷贝==。

初始化后，引用将与初始对象一直绑定，无法重新绑定另一对象。

引用本身不是对象，==不能定义引用的引用==

```c++
int &refVal4 = 10; //错误，引用类型初始值必须是对象
double dVal = 3.14;
int &refVal5 = dval; //错误，此处引用初始值必须为int对象
```



2. **指针**

指针是指向另一种类型的复合类型。

与引用相比：

* 指针本身就是一个对象，允许对指针赋值和拷贝，且可以指向不同对象。
* 指针无需在定义时赋值。



定义指针的方法将声明符写成*d的形式，d为变量名。

```c++
int *ip1, *ip2; //ip1和ip2都是指向int型对象的指针
double dp, *dp2; //dp2是指向double型对象的指针，dp是double型对象
```



指针存放某个对象的地址，获取该地址使用==取地址符&==

```c++
int val = 42;
int *p = &val; //p存放变量val的地址，或者说p是指向变量val的指针
```



指针值应是下列四种状态之一：

* 指向一个对象
* 指向紧邻对象所占空间的下一个位置
* 空指针，未指向任何对象
* 无效指针



若指针指向了一个对象，则允许使用==解引用符*==来访问该对象

```c++
int val = 42;
int *p = &val; //p存放着变量val的地址，或p是指向变量val的指针
cout << *p; //由符号*得到指针p所指对象，输出42
```



空指针不指向任何对象，生成空指针方法：

```c++
int *p = nullptr;
int *p = 0;
int *p = NULL;
```



==void*==是一种特殊的指针类型，可用于存放任意对象的地址

不能直接操作void*对象，因为不知道对象到底是什么类型



3. **理解复合类型的声明**

在定义语句中，类型修饰符*或&仅仅是修饰变量名，对声明语句中的其他变量不产生作用

多个修饰符连写在一起时，按照逻辑关系解释即可，**表示指向指针的指针

```c++
int val = 42;
int *p; //p是一个int型指针
int *&r = p; //r是一个对指针p的引用
r = &val; //r引用了一个指针，因此对r赋值&val，即令p指向val
*r = 0; //解引用r得到p指向的对象，将val改为0
```



从右向左阅读变量的定义。离变量名最近的符号决定变量的类型，因此r为引用，其余部分决定r引用的类型是什么，例子中的*说明r引用的是一个指针。



## 2.4 const限定符

const对象一旦创建后就不能再改变，所以const对象必须初始化

默认情况下，const对象被设定为==仅在文件内有效==

多个文件出现同名const变量时，等同于在不同文件中分别定义了独立变量



希望只在一个文件中定义const，在多个文件中声明并使用它

方法是不管声明还是定义都添加==extern==关键字，这样只需定义一次就可以



1. **const的引用**

对常量的引用不能被用作修改它所绑定的对象

```c++
const int val = 1024;
const int &r1 = val; //正确，引用及其对象都是常量
r1 = 42; //错误，r1是对常量的引用
int &r2 = val; //错误，试图让一个非常量引用指向一个常量对象
```



引用类型必须与引用对象类型一致，有两个例外：

* 初始化常量引用时，允许用任意表达式作为初始值，只要结果能转换成引用类型即可

```c++
int i = 42;
const int &r1 = i; //允许将const int&绑定到一个普通int对象上
const int &r2 = 42; //正确，r2是一个常量引用
const int &r3 = r1 * 2; //正确，r3是一个常量引用
int &r4 = r1 * 2; //错误，r4是一个普通的非常量引用
```



常量引用对于引用的对象本身是不是一个常量未做限定，所以允许通过其他途径改变它的值

```c++
int i = 42;
int &r1 = i; //引用r1绑定对象i
const int &r2 = i; //r2也绑定对象i，但是不允许通过r2修改i的值
r1 = 0; //r1并非常量，i的值修改为0
r2 = 0; //错误，r2是一个常量引用
```



2. **指针和const**

指向常量的指针不能用于改变其所指对象的值

要想存放常量对象的地址，只能使用指向常量的指针：

```c++
const double pi = 3.14; //pi是个常量，它的值不能改变
double *ptr  = &pi; //错误，ptr是一个普通指针
const double *cptr = &pi; //正确
*cptr = 42; //错误，不能给*cptr赋值
```



指向常量的指针也没有规定所指对象必须是一个常量。仅仅要求不能通过指针改变对象的值



常量指针必须初始化，一旦初始化完成则值不能再改变。==把*放在const之前说明指针是常量==

```c++
int errNum = 0;
int *const curErr = &errNum; //curErr将一直指向errNum
const double pi = 3.14159; 
const double *const pip = &pi; //pip是一个指向常量对象的常量指针
```

指针是常量并不意味着不能通过修改其所指对象的值，能否这样做取决于所指对象的类型。



3. **顶层const**

==顶层const==：指针本身是个常量

==底层const==：指针所指对象是个常量

更一般地，可以表示任意对象。



执行对象的拷贝时，顶层const不受影响

而对于底层const，拷入和拷出的对象必须具有相同的底层const资格，或两对象数据类型能转换，一般来说非常量可以转为常量，反之则不行



4. **constexpr和常量表达式**

常量表达式指值不会改变且在编译过程中就能得到计算结果的表达式

字面值属于常量表达式，用常量表达式初始化的const对象也是常量表达式



是不是常量表达式由数据类型和初始值共同决定

```c++
const int max_files = 20; //max_files是常量表达式
const int limit = max_files + 1; //limit是常量表达式
int staff_size = 27; //staff_size不是常量表达式
const int sz = get_size(); //不是常量表达式
```

staff_size数据类型只是普通的int，sz具体值直到运行时才获得



C++11允许将变量声明为constexpr类型以便编译器来验证变量的值是否是一个常量表达式

```c++
constexpr int mf = 20; //20是常量表达式
constexpr int limit = mf + 1; //mf + 1是常量表达式
constexpr int sz = size(); //只有当size是一个constexpr函数时才是一条正确声明
```



对声明constexpr时用到的类型必须有所限制，称为==字面值类型==

算术类型、引用和指针都属于字面值类型

constexpr指针初始值必须是nullptr或0，或是存储于某个固定地址中的对象



函数体内定义变量一般来说并非存放再固定地址中，因此constexpr指针不能指向这样的变量

constexpr仅对指针有效，与指针所指的对象无关：

```c++
constexpr int *q = nullptr; //q是一个指向整数的常量指针
```



## 2.5 处理类型

1. **类型别名**

类型别名是一个名字，是某种类型的同义词

有两种方法可以定义类型别名：

* 使用关键字==typedef==
* 使用==别名声明==

```c++
typedef double wages; //wages是double的同义词
typedef wages base, *p; //base是double的同义词，p是double*的同义词
using SI = Sales_item; //SI是Sales_item的同义词
```



若某类型别名指代复合类型或常量，把它用到声明语句中会产生意想不到的后果。

```c++
typedef char *pstring;
const pstring cstr = 0; //cstr是指向char的常量指针
const pstring *ps; //ps是一个指针，它的对象是指向char的常量指针
```

const是对给定类型的修饰，pstring是指向char的指针，因此const pstring就是指向char的常量指针。



2. **auto类型说明符**

使用auto说明符能让编译器通过初始值来推算变量的类型，因此auto定义的变量必须有初始值。

使用auto声明多个变量时，该语句中所有变量的初始值类型必须一样。



编译器推断出的auto类型有时和初始值并不完全一样，编译器会适当改变结果类型。

如使用引用作为初始值时，编译器以引用对象的类型作为auto的类型。



auto一般会忽略顶层const，保留底层const：

```c++
int i = 0;
const int ci = i, &cr = ci;
auto b = ci; //b是一个整数，ci的顶层const特性被忽略了
auto c = cr; //c是一个整数，cr是ci别名，ci是顶层const
auto d = &i; //d是整型指针，整数的地址就是指向整数的指针
auto e = &ci; //e是一个指向整数常量的指针，对常量对象取地址是一种底层const
```



若希望推断出auto类型是顶层const，需明确指出

```c++
const auto f = ci;
```



还可以为引用类型设为auto

```c++
auto &g = ci; //g是一个整型常量引用，绑定到ci
auto &h = 42; //错误，不能为非常量引用绑定字面值
const auto &j = 42; //正确，可以为常量引用绑定字面值
```



3. **decltype**

有时希望从表达式的类型推断出要定义的变量类型，但是不想用该表达式的值初始化变量

==decltype==，作用是选择并返回操作数的数据类型。

```c++
decltype(f()) sum = x;//sum的类型就是函数f的返回类型
```

编译器并不调用f()，而是使用f的返回值类型作为sum的类型



若decltype使用的表达式是一个变量，返回变量类型（包括顶层const和引用）

```c++
const int ci = 0, &cj = ci;
decltype(ci) x = 0; //x的类型是const int
decltype(cj) y = x; //y的类型是const int&，y绑定到变量x
decltype(cj) z; //错误，z是一个引用，必须初始化
```



若decltype使用的表达式不是一个变量，则返回表达式结果对应的类型

```c++
int i = 42, *p = &i, &r = i;
decltype(r + 0) b; //正确，加法结果是int，因此b是一个未初始化的int
decltype(*p) c; //错误，c是int&，必须初始化
```



若表达式的内容是解引用操作，则decltype将得到引用的类型

变量加括号会得到引用类型



## 2.6 自定义数据结构

1. **定义Sales_data类型**

类以关键字==struct==开始，紧跟着类名和类体，类内部名字必须唯一，但是可以与外部定义的名字重复

```c++
struct Sales_data
{
    string bookNo;
    unsigned units_sole = 0;
    double revenue = 0.0;
};
```

右侧结束花括号必须写一个分号



类体定义类的成员，每个对象有自己的一份数据成员拷贝，修改一个对象的数据成员，不会影响其他对象

可以为数据成员提供一个初始值，没有初始值的成员将被默认初始化



3. **编写自己的头文件**

类通常被定义在头文件中，而且类所在头文件的名字应与类一致

头文件通常包含那些只能被定义一次的实体，如类、const和constexpr变量

头文件也经常用到其他头文件的功能



确保头文件多次包含仍能安全工作的常用技术是预处理器

预处理器是在编译前执行的一段程序，可以部分地改变我们所写的程序

当预处理器看到#include标记时就会用指定的头文件的内容代替#include



C++中用到的一项预处理功能为==头文件保护符==，依赖于预处理变量

预处理变量有两种状态，已定义和未定义

==#define==指令把一个名字设为预处理变量

==#ifdef==当且仅当变量已定义时为真，==ifndef==当且仅当变量未定义时为真

一旦检查结果为真，则执行后续操作直到遇到==#endif==指令

```c++
#ifndef SALES_DATA_H
#define SALES_DATA_H
#include <string>
struct Sales_data
{
    string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
};
#endif
```



一般把预处理变量的名字全部大写



# 第三章   字符串、向量和数组

## 3.1 命名空间的using声明

使用using声明后，可以直接访问所需的名字

如using namespace :: name

```c++
#include <iostream>
using std::cin;
int main()
{
    int i;
    cin >> i;
    std::out << i;
    return 0;
}
```



用到的每个名字都必须有自己的声明语句

头文件不应包含using声明，因为头文件内容会拷贝到引用它的文件中去，若头文件中有using声明，则每个使用该头文件的文件都会有该声明



## 3.2 标准库类型string

string表示可变长字符序列，使用：

```c++
#include <string>
using std::string;
```



1. **定义和初始化string对象**

初始化对象方式：

|      初始化代码      |                         解释                          |
| :------------------: | :---------------------------------------------------: |
|      string  s1      |               默认初始化，s1是一个空串                |
|    string  s2(s1)    |                     s2是s1的副本                      |
|   string  s2 = s1    |              等价于s2(s1)，s2是s1的副本               |
| string  s3("value")  | s3是字面值"value"的副本，除了字面值最后的那个空字符外 |
| string  s3 = "value" |      等价于s3("value")，s3是字面值"value"的副本       |
|  string  s4(n, 'c')  |          把s4初始化为由连续n个字符c组成的串           |



若使用一个＝号初始化一个变量，实际上执行的是==拷贝初始化==，若不使用=号，执行的是==直接初始化==。



2. **string对象上的操作**

|   string操作    |                         解释                         |
| :-------------: | :--------------------------------------------------: |
|     os << s     |             将s写到输出流os当中，返回os              |
|     is >> s     |   从is中读取字符串赋给s，字符串以空白分隔，返回is    |
| getline(is,  s) |             从is中读取一行赋给s，返回is              |
|    s.empty()    |                    s为空返回true                     |
|    s.size()     |                  返回s中字符的个数                   |
|      s[n]       |                返回s中第n个字符的引用                |
|     s1 + s2     |                返回s1和s2连接后的结果                |
|     s1 = s2     |              用s2的副本代替s1中原来字符              |
|    s1 == s2     |         若s1和s2所含字符完全一样，则它们相等         |
| < , <= , > , >= | 利用字符在字典中的顺序进行比较，且对字母的大小写敏感 |



读写string对象：

```c++
int main()
{
    string s; //空字符串
    cin >> s; //将string对象读入s，遇到空白停止
    cout << s << endl; //输出s
    return 0;
}
```



使用getline从给定的输入流中读入内容，直到遇到换行符为止。（读取换行符，但不存）



empty函数判断string对象是否为空，size函数返回string对象长度。

size函数返回的是size_type类型，是一个无符号类型的值。



判断string大小：

* 若两个string长度不同，较短string每个字符都与较长string字符相同，则说较短string小于较长string
* 若两个string在某些对应位置不同，则比较结果为第一个不同字符的比较结果



两个string对象相加得到一个新的string对象。

把string对象和字符字面值及字符串字面值混用时，+两侧运算对象至少有一个是string



cctype头文件中处理字符函数：

|   函数名    |          作用           |
| :---------: | :---------------------: |
| isalnum(c)  |   c是字母或数字时为真   |
| isalpha(c)  |      c是字母时为真      |
| iscntrl(c)  |    c是控制字符时为真    |
| isdigit(c)  |      c是数字时为真      |
| isgraph(c)  | c不是空格但可打印时为真 |
| islower(c)  |    c是小写字母时为真    |
| isprint(c)  |   c是可打印字符时为真   |
| ispunct(c)  |    c是标点符号时为真    |
| isspace(c)  |      c是空白时为真      |
| isupper(c)  |    c是大写字母时为真    |
| isxdigit(c) |  c是十六进制数字时为真  |
| tolower(c)  |      输出c小写形式      |
| toupper(c)  |      输出c大写形式      |



处理每个字符：

```c++
string str("some string");
for(auto c : str)
    cout << c << endl;
```

若想改变string对象中字符的值，必须把循环变量定义成引用类型。



只处理一部分字符：使用下标或迭代器。

使用下标访问指定字符时，首先检查string是否为空。



## 3.2 标准库类型vector

vector表示对象的集合，其中所有对象的类型都相同

每个对象都有一个对应的索引

使用vector，必须包含头文件，#include <vector>



vector是类模板，实例化时需指出应把类实例化为何种类型

引用不是对象，因此不存在包含引用的vector



1. **定义和初始化vector对象**

|     初始化vector方法      |                解释                 |
| :-----------------------: | :---------------------------------: |
|       vector<T> v1        |            v1是空vector             |
|     vector<T> v2(v1)      |       v2包含v1所有元素的副本        |
|     vector<T> v2 = v1     |            等价于v2(v1)             |
|   vector<T> v3(n, val)    |  v3包含n个重复元素，每个值都是val   |
|      vector<T> v4(n)      | v4包含n个重复地执行了值初始化地对象 |
| vector<T> v5{a, b, c...}  |     v5每个元素被赋予相应初始值      |
| vector<T> v5 = {a,b,c...} |         等价于v5{a,b,c...}          |



若使用圆括号，则提供的值是用来构造vector对象的

如果用的是花括号，说明要列表初始化该vector对象



2. **向vector对象中添加元素**

使用push_back将值压到vector对象尾端。



3.**其他vector操作**

其他操作如empty、size等。

vector下标可以访问已存在元素，不能用于添加元素。



## 3.4 迭代器介绍

所有标准库容器都可以使用迭代器，只有少数几种才同时支持下标运算符

有效迭代器指向==某个元素==或==指向容器中尾元素的下一位置==，其他情况都属于无效



迭代器都拥有begin和end成员，begin返回指向第一个元素的迭代器，end返回指向容器尾元素的下一位置的迭代器

标准容器迭代器运算符：

|  迭代器运算符  |                           解释                           |
| :------------: | :------------------------------------------------------: |
|     *iter      |               返回迭代器iter所指元素的引用               |
|   iter->men    | 解引用iter并获取该元素的名为men的成员，等价于(*iter).men |
|     ++iter     |               令iter指示容器中的下一个元素               |
|     --iter     |               令iter指示容器中的上一个元素               |
| iter1 == iter2 |        判断两个迭代器是否相等，指示同一位置则相等        |
| iter1 != iter2 |                                                          |



与指针类似，也能通过解引用迭代器来获取它所指示的元素。

迭代器使用递增运算符（++）从一个元素移动到下一个元素。

使用iterator和const_iterator表示迭代器的类型：

```c++
vector<int>::iterator  it1; //it1能读写vector<int> 元素
string::iterator it2; //it2能读写string对象中的字符
vector<int>::const_iterator it3; //it3只能读元素，不能写元素
```



cbegin和cend返回值都为const_iterator

箭头运算符（->）把解引用和成员访问两个操作结合在一起，即it->mem和(*it).mem表达意思相同。



迭代器支持的运算：

见书



## 3.5 数组

与vector不同的是，数组大小确定不变

数组维度必须是一个常量表达式

```c++
unsigned cnt = 42; //不是常量表达式
constexpr unsigned sz = 42; //常量表达式
int arr[10]; //含有10个整数的数组
int *parr[sz]; //含有42个整型指针的数组
string bad[cnt]; //错误，cnt不是常量表达式
```



数组元素应为对象，因此不存在引用的数组

数组初始化：

```c++
const unsigned sz = 3;
int ial[sz] = {0, 1, 2};
int a2[] = {0, 1, 2};
int a3[5] = {0, 1, 2}; //等价于{0,1,2,0,0}
string a4[3] = {"hi", "bye"}; //等价于{"hi","bye",""}
int a5[2] = {0, 1, 2}; //错误，初始值过多
```



使用字符串字面值初始化字符数组时，字符串字面值结尾处还有一个空字符

```c++
char a1[] = {'C', '+', '+'}; //列表初始化，没有空字符
char a2[] = {'C', '+', '+', '\0'}; //列表初始化，含有显式的空字符
char a3[] = "C++"; //自动添加表示字符串结束的空字符
const char a4[6] = "Daniel"; //错误，没有空间可以存放空字符
```



不能将数组的内容拷贝给其他数组作为其初始值，也不能用数组为其他数组赋值



复杂数组声明：

```c++
int *ptrs[10]; //ptrs是含有10个整型指针的数组
int &refs[10] = /* ？ */; //错误，不存在引用的数组
int (*Parray)[10] = &arr; //Parray指向一个含有10个整数的数组
int (&arrRef)[10] = arr; //arrRef引用一个含有10个整数的数组
```



默认情况下，类型修饰符从右往左读，对数组先由内向外，再由右向左

如Parray，*Parray表示Parray是一个指针，再观察右边知道Parray是个指向大小为10的数组的指针；再观察左边，知道数组中的元素为int



数组可以使用下标访问，下标定义为size_t类型



使用数组时，编译器一般会把它转换成指针

对数组元素使用取地址符就能得到指向该元素的指针：

```c++
string nums[] = {"one", "two"};
string *p = &nums[0];
```



在用到数组名字的地方，编译器会自动将其替换为一个指向数组首元素的指针

因此某些情况下，数组的操作实际上是指针的操作

```c++
int ia[] = {0, 1, 2};
auto ia2(ia); //ia2是一个整型指针，指向ia的第一个元素
ia2 = 42; //错误，ia2是一个指针，不能用int值给其赋值
```



使用decltype关键字时，上述转换不会发生



vector和string迭代器支持的运算，数组的指针全都支持。

```c++
int arr[] = {0, 1, 2};
int *p = arr; //p指向arr的第一个元素
++p; //p指向arr[1]
int *e = &arr[10]; //指向arr为元素的下一位置指针(不存在元素地址)
int *beg = begin(arr);
int *last = end(arr);
```



给指针加上一个整数，得到的新指针仍指向同一数组的其他元素



字符串字面值是C++由C继承而来的C风格字符串

```c++
char str[] = "Hello, World!";
```

按此习惯书写的字符串存放在字符数组中并以空字符（'\0'）结束



cstring头文件中，包含可操作C风格字符串的函数

| C风格字符串函数 |                           作用                            |
| :-------------: | :-------------------------------------------------------: |
|    strlen(p)    |               返回p的长度，空字符不计算在内               |
| strcmp(p1, p2)  | 比较p1和p2的相等性，相等返回0，前者大返回正值，否则为负值 |
| strcat(p1, p2)  |                 将p2附加到p1之后，返回p1                  |
| strcpy(p1, p2)  |                   将p2拷贝给p1，返回p1                    |



使用比较运算符比较C风格字符串时，比较的是指针而非字符串本身



混用string对象和C风格字符串，反过来不成立：

* 允许使用以空字符结束的字符数组来初始化string对象或为string对象赋值
* 在string对象的加法运算中允许使用以空字符结束的字符数组作为其中一个运算对象
* 在string对象的复合赋值运算中允许使用以空字符结束的字符数组作为其右侧的运算对象



string提供一个c_str函数，将string转化为C风格字符串，返回一个指针

可以使用数组初始化vector对象，只需指明拷贝区域的首元素地址和尾后地址即可，也可拷贝一部分：

```c++
int arr[] = {0, 1, 2, 3};
vector<int> subVec(arr+1, arr+4);
```



## 3.6 多维数组

C++中没有多维数组，其实是数组的数组

```c++
int ia[3][4] = {
    {0,1,2,3}, //第一行的初始值
    {4,5,6,7}, //第二行的初始值
    {8,9,10,11} //第三行的初始值
}
```

内层花括号不是必须的



当程序使用多维数组名字时，也会自动将其转换成指向数组首元素的指针

多维数组名转换得来的指针实际是指向第一个内层数组的指针



# 第四章   表达式

## 4.1 基础

1. **基本概念**

表达式需要理解运算符的优先级、结合律及运算对象的求值顺序

表达式求值时，运算对象常常由一种类型转换成另一种类型，小整数类型通常会被提升成较大整数类型



可以自定义运算符作用于类类型的运算符，称为重载运算符

运算对象的类型和返回值的类型，都是由运算符定义的，但运算对象个数、运算符的优先级和结合律是无法改变的



C++表达式要不然是左值，要不然就是右值

==当一个对象被用作右值的时候，用的是对象的值（内容）；当对象被用作左值的时候，用的是对象的身份（在内存中的位置）==

需要右值的地方可以用左值代替，但不能把右值当成左值使用



2. **优先级和结合律**

先考虑优先级，优先级相同考虑结合律

括号无视优先级和结合律



3. **求值顺序**

四种运算符明确规定了运算对象的求值顺序：

* &&，先求左侧运算对象的值，左侧值为真才求右侧的值
* ||
* ？：
* ,



## 4.2 算术运算符

一元运算符优先级最高，接下来是乘法和除法，接下来是加法和减法



## 4.3 逻辑和关系运算符

关系运算符作用于算术类型或指针类型，逻辑运算符作用于任意能转换成布尔值的类型



## 4.4 赋值运算符

赋值运算符左侧对象必须是一个可修改的左值

赋值运算满足右结合律



## 4.5 递增和递减运算符

这两个运算符还可应用于迭代器。



## 4.6 成员访问运算符

点运算符和箭头运算符都可用于访问成员

点运算符获取类对象的一个成员，箭头运算符相当于解引用后获取对象成员

```c++
string s1 = "a"， *p = &s1;
auto n = s1.size(); //获取s1的size成员
n = (*p).size();  //获取p指向对象的size成员
n = p->size();  //等价于(*p).size()
```



## 4.7 条件运算符

```c++
cond ? expr1 : expr2;
```

先求cond的值，若条件为真使用expr1表达式，否则使用expr2表达式



允许在条件运算符的内部嵌套另外一个条件运算符

条件运算符满足右结合律



## 4.8 位运算符

一般来说，如果运算对象是“小整型”，则它的值会被自动提升成较大的整数类型



## 4.9 sizeof运算符

sizeof运算符返回一条表达式或一个类型名字所占的字节数

其所得的值是一个size_t类型



两种形式：

```c++
sizeof(type)
sizeof expr
```



sizeof结果：

* char类型执行sizeof运算，结果为1
* 对引用类型执行sizeof运算得到被引用对象所占空间大小
* 对指针执行sizeof运算得到指针本身所占空间的大小
* 对解引用指针执行sizeof运算得到指针指向的对象所占空间的大小，指针不需有效
* 对数组执行sizeof运算得到整个数组所占空间的大小
* 对string对象或vector对象执行sizeof运算只返回该类型固定部分的大小，不会计算对象中的元素占用了多少空间



```c++
//计算数组元素个数
constexpr size_t sz = sizeof(ia) / sizeof(*ia);
```



## 4.10 逗号运算符

首先对左侧的表达式求值，然后将求值结果丢弃掉

逗号运算符真正的结果是右侧表达式的值



## 4.11 类型转换

C++语言不会直接将两个不同类型的值相加，而是先根据类型转换规则设法将运算对象的类型统一后再求值

上述类型转换是自动执行的，被称为隐式转换



整数提升：负责把小整数类型转换成较大的整数类型



数组转换成指针：在大多数用到数组的表达式中，数组自动转换成指向数组首元素的指针

当数组被用作decltype关键字的参数，或者作为取地址符、sizeof及typeid等运算符的运算对象时，上述转换不会发生



指针转换：

* 常量整数值0或者字面值nullptr能转换成任意指针类型
* 指向任意非常量的指针能转换成void＊
* 指向任意对象的指针能转换成const void＊



# 第五章   语句

## 5.1 简单语句

在块中引入的名字只能在块内部及嵌套在块中的子块里访问



## 5.2 语句作用域

定义在控制结构中的变量只在相应语句的内部可见



## 5.3 条件语句

1.**if语句**

语句形式

```c++
if(condition)
    statement
else if(condition2)
    statement2
else
    statement3
```



在C++中，else与离它最近的尚未匹配的if匹配，从而消除程序的二义性



2. **switch语句**

例如，判断字母是否为元音字母

```c++
switch(ch)
{
    case 'a':
        ...
        break;
    case 'e':
        ...
        break;
    ... 
    default:
        ...
        break;
}
```



switch首先对括号内表达式求值，若表达式和某个case标签的值匹配成功，则执行该标签的语句，直到遇到break为止

case标签必须是整型常量表达式

任何两个case标签的值不能相同

如果没有任何一个case标签能匹配上，则执行default标签后面的语句



## 5.4 迭代语句

while和for语句在执行循环体之前检查条件，do while语句先执行循环体再检查条件。



1. **while语句**

```c++
while(condition)
    statement
```



2. **传统的for语句**

```c++
for(initializer; condition; expression)
    statement
```



for语句头能省略掉initializer、condition和expression中的任何一个或全部，分号必须保留



3. **范围for语句**

可以遍历容器或其他序列的所有元素。

```c++
for(declaration : expression)
    statement
```



expression必须是一个序列，特点是拥有能返回迭代器的begin和end成员

declaration定义一个变量，序列中元素都能转换成该变量的类型

最简单办法是使用auto说明符



4. **do while语句**

不管条件值如何，至少执行一次循环

```c++
do
    statement
while(condition)
```



## 5.5 跳转语句

C++提供四种跳转语句：break、continue、goto和return



1. **break语句**

break语句负责终止离它最近的while、do while、for或switch语句，并从这些语句之后的第一条语句开始继续执行

break语句作用范围仅限于最近的循环或switch



2. **continue语句**

continue语句终止最近的循环中的当前迭代，并立即开始下一次迭代

只有当switch语句嵌套在迭代语句内部时，才能在switch里使用continue



3. **goto语句**

从goto语句无条件跳转到同一函数内的另一条语句

```c++
goto label；
```



label是用于标识一条语句的标识符



## 5.6 try语句块和异常处理

异常指存在于运行时的反常行为，这些行为超出了函数正常功能范围

包括失去数据库连接及遇到意外输入等

异常处理包括：

* throw表达式：异常检测部分使用throw表达式
* try语句块：异常处理部分使用try语句块处理异常。以try开始，一个或多个catch子句结束
* 一套异常类：用于在throw表达式和相关catch子句之间传递异常的具体信息



1. **throw表达式**

throw表达式包含关键字throw和紧随其后的一个表达式，其中表达式的类型就是抛出的异常类型

```c++
throw runtion_error("Data must refer to same ISBN");
```



2. **try语句块**

通用语法

```c++
try
{
    program-statements
}
catch(exception-declaration)
{
    handler-statements
}
```

catch子句包括三部分：

* 关键字catch
* 括号内的异常声明
* 一个块



catch一旦完成，跳转到try语句块最后一个catch子句之后的语句继续执行

没有找到catch子句，或没有try语句定义的异常，系统会调用terminate函数并终止当前程序的执行



3. **标准异常**

异常类所在头文件：

* exception头文件定义了最通用的异常类exception。只报告异常的发生，不提供任何额外信息
* stdexcept头文件定义了几种常用的异常类
* new头文件定义了bad_alloc异常类型
* type_info头文件定义了bad_cast异常类型



# 第六章   函数

## 6.1 函数基础

函数定义包括：

* 返回类型
* 函数名字
* 0或多个形参组成的列表
* 函数体



函数示例：

```c++
int fact(int val)
{
    int ret = 1;
    while(val > 1)
        ret *= val--;
    return ret;
}
```



函数调用：使用实参初始化函数对应形参，将控制权转移给被调函数

遇到return语句函数结束执行过程，返回return语句的值，将控制权转到主调函数

函数返回类型不能是数组类型或函数类型，可以是指向数组或函数的指针



1. **局部对象**

名字有作用域，对象有生命周期，生命周期是程序执行过程中对象存在的一段时间



形参和函数体内部定义的变量统称为局部变量，仅在函数的作用域内可见

把只存在于块执行期间的对象称为自动对象

形参是一种自动对象，函数开始时为形参申请存储空间，函数终止销毁形参



局部静态对象直到程序终止才销毁，即使对象所在的函数结束执行也不会有影响   ==static==



2. **函数声明**

函数名字必须在使用前声明，函数声明也称为函数原型

函数只能定义一次，但可以声明多次

函数的声明无需函数体，用一个分号代替即可

```c++
void print(int a);
```

变量和函数建议在头文件中定义



3. **分离式编译**

分离式编译允许把程序分割到几个文件中去，每个文件独立编译

若需要修改其中一个源文件，只需重新编译那个改动了的文件

Windows的后缀是.obj，UNIX的后缀是.o

接下来编译器把对象文件链接在一起形成可执行文件



## 6.2 参数传递

如果形参是引用类型，它将绑定到对应的实参上；否则将实参的值拷贝后赋给形参

形参为引用类型时，对应的实参被==引用传递==

实参值被拷贝给形参时，形参和实参是两个相互独立的对象。这样的实参为==值传递==



1. **传值参数**

当初始化一个非引用类型的变量时，初始值被拷贝给变量

此时，对变量的改动不会影响初始值，传值参数机理一样

```c++
int n = 0;
int i = n; //i是n的值的副本
i = 42; //i的值改变，n的值不变
```



指针的行为和其他非引用类型一样



2. **传引用参数**

对于引用的操作实际上是作用在引用所引的对象上

```c++
int n = 0;
int &r = n; //r绑定了n
r = 42; //现在n的值是42
```



拷贝大的类类型对象或容器对象比较低效，甚至不支持拷贝操作，函数只能通过引用形参访问该类型的对象

```c++
bool isShorter(const string &s1, const string &s2)
{
    return s1.size() < s2.size();
}
```



使用引用形参可以返回多个结果。在函数中传入多个引用实参



3. **const形参和实参**

用实参初始化形参时会忽略顶层const

把函数不会改变的形参定义为常量引用



4. **数组形参**

尽管不能以值传递的方式传递数组，但是我们可以把形参写成类似数组的形式：

```c++
void print(const int*);
void print(const int[]); 
void print(const int[10]); //维度表示期望数组含有多少元素，实际不一定
```



如果传入是数组，则实参自动地转换成指向数组首元素的指针，数组大小对函数调用没有影响

管理指针形参有三种常用的技术：

* 使用标记指定数组长度

数组本身包含一个结束标记，函数在遇到结束标记后停止：

```c++
void print(const char *cp)
{
    if(cp) //若cp不是一个空指针
        while(cp) //只要指针所指得字符不是空字符
            cout << *cp++; //输出当前字符并将指针向前移动一个位置
}
```



* 使用标准库规范

传递指向数组首元素和尾后元素的指针。

```c++
void print(const int *beg, const int *end)
{
    //输出beg到end之间（不含end）的所有元素
    while(beg != end)
        cout << *beg++ << endl;
}
```



* 显式传递一个表示数组的形参

专门定义一个表示数组大小的形参

```c++
void print(const int ia[], size_t size)
{
    for(size_t i = 0; i != size; ++i)
        cout << ia[i] <<endl;
}
```



C++允许将变量定义成数组的引用

```c++
void print(int (&arr)[10])
{
    for(auto elem : arr)
        cout << elem << endl;
}
```



传递多维数组，后面所有维度的大小都是数组的一部分，不能省略

```c++
void print(int (*matrix)[10], int rowSize)
```



5. **main：处理命令行选项**

命令行通过两个形参传递给main函数：

```c++
int main(int argc, char *argv[])
```



第二个形参argv是一个数组，它的元素是指向C风格字符串的指针，最后一个指针之后的元素值保证为0

第一个形参argc表示数组中字符串的数量



例如，main函数位于可执行文件prog内

```c++
prog -d -o ofile data0
  
//则argc等于5，
argv[0] = "prog";
argv[1] = "-d";
argv[2] = "-o";
argv[3] = "ofile";
argv[4] = "data0";
argv[5] = 0;
```



6. **含有可变形参的函数**

为编写能处理不同数量实参的函数，提供两种主要方法：

* 如果所有的实参类型相同，可以传递一个名为initializer_list的标准库类型
* 如果实参的类型不同，我们可以编写一种特殊的函数，也就是所谓的可变参数模板



initializer_list中元素是常量值，无法改变

```c++
initializer_list<string> ls;
error_msg({"functionX", "okay"});
```



省略符形参只能出现在形参列表的最后一个位置

```c++
void foo(parm_list, ...);
void foo(...);
```



## 6.3 返回类型和return语句

return语句终止当前正在执行的函数并将控制权返回到调用该函数的地方



1. **无返回值函数**

没有返回值的return语句只能用在返回类型是void的函数中

void函数如果想在它的中间位置提前退出，可以使用return语句



2. **有返回值函数**

return语句返回值的类型必须与函数的返回类型相同，或者能隐式地转换成函数的返回类型



==不要返回局部对象的引用或指针==，当函数结束时，临时对象占用的空间也就随之释放掉，将指向不再可用的内存空间

```c++
//严重错误：该函数试图返回局部对象的引用
const string &mainp()
{
    string ret;
    if(!ret.empty())
        return ret; //错误：返回局部对象的引用
    else
        return "Empty"; //错误，"Empty"是一个局部临时量
    
}
```



调用一个返回引用的函数得到左值，其他返回类型得到右值

```c++
char &get_val(string &str, string::size_type ix)
{
    return str[ix];
}
int main()
{
    string s("a value");
    get_val(s,0) = 'A';
}
```



函数可以返回花括号包围的值的列表



如果一个函数调用了它自身，不管这种调用是直接的还是间接的，都称该函数为递归函数（recursive function）



在递归函数中，一定有某条路径是不包含递归调用的；否则，函数将“永远”递归下去



3. **返回数组指针**

因为数组不能被拷贝，所以函数不能返回数组

不过，函数可以返回数组的指针或引用

主要有四种方法：

* 使用类型别名

```c++
typedef int arrT[10];
arrT* func(int i);
```



* 声明一个返回数组指针的函数

如果我们想定义一个返回数组指针的函数，则数组的维度必须跟在函数名字之后

```c++
int (*func(int i)) [10];
// （＊func（int i））意味着我们可以对函数调用的结果执行解引用操作
// ＊func（int i））[10]表示解引用func的调用将得到一个大小是10的数组
// int （＊func（int i））[10]表示数组中的元素是int类型
```



* 使用尾置返回类型

尾置返回类型跟在形参列表后面并以一个->符号开头

为了表示函数真正的返回类型跟在形参列表之后，我们在本应该出现返回类型的地方放置一个auto

```c++
auto func(int i) -> int(*)[10];
```



* 使用decltype

如果我们知道函数返回的指针将指向哪个数组，就可以使用decltype关键字声明返回类型

```c++
int odd[] = {1,3,5,7,9};
int even[] = {0,2,4,6,8};
decltype(odd) *arrPtr(int i)
{
    return (i%2) ? &odd : &even;
}
```

关键字decltype表示它的返回类型是个指针，并且该指针所指的对象与odd的类型一致



decltype并不负责把数组类型转换成对应的指针，所以decltype的结果是个数组，要想表示arrPtr返回指针还必须在函数声明时加一个＊符号



## 6.4 函数重载

如果同一作用域内的几个函数名字相同但形参列表不同，我们称之为重载（overloaded）函数



对于重载的函数来说，它们应该在形参数量或形参类型上有所不同

不允许两个函数除了返回类型外其他所有的要素都相同



不影响传入函数的对象

一个拥有顶层const的形参无法和另一个没有顶层const的形参区分开来

```c++
Record lookup(Phone);
Record lookup(const Phone); //重复声明
```



如果形参是某种类型的指针或引用，则通过区分其指向的是常量对象还是非常量对象可以实现函数重载，此时的const是底层的

```c++
Record lookup(Account*);
Record lookup(const Account*); //新函数，作用于指向常量的指针
```



当我们传递一个非常量对象或者指向非常量对象的指针时，编译器会优先选用非常量版本的函数



函数的参数和返回类型都是const string的引用。我们可以对两个非常量的string实参调用这个函数，但返回的结果仍然是const string的引用

当它的实参不是常量时，得到的结果是一个普通的引用，使用const_cast可以做到这一点

```c++
string &shorterString(string &s1, string &s2)
{
    auto &r = shorterString(const_cast<const string&>(s1), 										const_cast<const string&>(s2));
    return const_cast<string&>(r);
}
```



函数重载三种结果：

* 编译器找到一个与实参最佳匹配（best match）的函数，并生成调用该函数的代码
* 找不到任何一个函数与调用的实参匹配，此时编译器发出无匹配（no match）的错误信息
* 有多于一个函数可以匹配，但是每一个都不是明显的最佳选择。此时也将发生错误，称为二义性调用



如果我们在内层作用域中声明名字，它将隐藏外层作用域中声明的同名实体



## 6.5 特殊用途语言特性

1. **默认实参**

调用含有默认实参的函数时，可以包含该实参，也可以省略该实参

一旦某个形参被赋予了默认值，它后面的所有形参都必须有默认值

如果我们想使用默认实参，只要在调用函数的时候省略该实参就可以了

```c++
string screen(size_type ht = 24, size_ty wid = 80, char backgrnd = '');
```



在给定作用域中，一个形参只能被赋予一次默认实参

局部变量不能作为默认实参



2. **内联函数和constexpr函数**

内联函数可以避免函数调用的开销

将函数指定为内联函数（inline），通常就是将它在每个调用点上“内联地”展开

在函数的返回类型前面加上关键字inline，这样就可以将它声明成内联函数了

```c++
inline const string &
shorterString(const string &s1, const string &s2)
{
    return s1.size() <= s2.size() ? s1 : s2;
}
```



内联机制用于优化规模较小、流程直接、频繁调用的函数



constexpr函数（constexpr function）是指能用于常量表达式的函数

函数的返回类型及所有形参的类型都得是字面值类型，而且函数体中必须有且只有一条return语句

```c++
constexpr int new_sz() { return 42; }
constexpr int foo = new_sz(); //正确，foo是一个常量表达式
```



为了能在编译过程中随时展开，constexpr函数被隐式地指定为内联函数

constexpr函数体内也可以包含其他语句，只要这些语句在运行时不执行任何操作就行

允许constexpr函数的返回值并非一个常量



内联函数和constexpr函数可以在程序中多次定义

对于某个给定的内联函数或者constexpr函数来说，它的多个定义必须完全一致

因此，内联函数和constexpr函数通常定义在头文件中



3. **调试帮助**

程序可以包含一些用于调试的代码，但是这些代码只在开发程序时使用

当应用程序编写完成准备发布时，要先屏蔽掉调试代码

用到两个预处理功能==assert==和==NDEBUG==



assert是一种预处理宏，行为有点类似内联函数

assert使用一个表达式作为它的条件

```c++
assert(expr);
```



如果表达式为0，则assert输出信息并终止程序的执行

如果表达式为真，assert什么也不做



assert的行为依赖于一个名为NDEBUG的预处理变量的状态

如果定义了NDEBUG，则assert什么也不做

默认状态下没有定义NDEBUG，此时assert将执行运行时检查



可以使用#define定义NDEBUG

可以使用变量_ _ _func_ _ _输出当前调试的函数的名字

* _ _ _FILE_ _ _：存放文件名的字符串字面值
* _ _ _LINE_ _ _：存放当前行号的整型字面值
* _ _ _TIME _ _：存放文件编译时间的字符串字面值
* _ _ _DATE_ _ _：存放文件编译日期的字符串字面值



## 6.6 函数匹配

* 函数匹配的第一步是选定本次调用对应的重载函数集，集合中的函数称为候选函数（candidate function）         候选函数特征：
  * 是与被调用的函数同名
  * 是其声明在调用点可见

* 第二步考察本次调用提供的实参，然后从候选函数中选出能被这组实参调用的函数，这些新选出的函数称为可行函数（viable function）    可行函数特征：
  * 形参数量与本次调用提供的实参数量相等
  * 每个实参的类型与对应的形参类型相同，或者能转换成形参的类型

* 第三步寻找最佳匹配



## 6.7 函数指针

函数的类型由它的返回类型和形参类型共同决定，与函数名无关

要想声明一个可以指向该函数的指针，只需要用指针替换函数名即可

```c++
bool lengthCompare(const string &, const string &); //原型
bool (*pf)(const string &, const string &); //未初始化
```



当我们把函数名作为一个值使用时，该函数自动地转换成指针

```c++
pf = lengthCompare; //pf指向名为lengthCompare的函数
```

还可以直接使用指向函数的指针调用该函数，无需提前解引用指针

```c++
bool b1 = pf("hello", "goodbye"); //调用lengthCompare函数
bool b2 = (*pf)("hello", "goodbye"); //等价调用
bool b3 = lengthCompare("hello", "goodbye"); //另一个等价调用
```



在指向不同函数类型的指针间不存在转换规则

可以为函数指针赋一个nullptr，或0表示指针没有指向任何一个函数



指针类型必须与重载函数中的某一个精确匹配



和数组类似，虽然不能定义函数类型的形参，但是形参可以是指向函数的指针

```c++
//可以直接把函数作为实参使用，此时它会自动转换成指针
useBigger(s1, s2, lengthCompare);
```



类型别名和decltype能让我们简化使用了函数指针的代码

```c++
typedef decltype(lengthCompare) *FuncP2;

using PF = int(*)(int*, int); //PF是指针类型
```



必须时刻注意的是，和函数类型的形参不一样，返回类型不会自动地转换成指针

必须显式地将返回类型指定为指针



还可以使用尾置返回类型的方式声明一个返回函数指针的函数

```c++
auto f1(int) -> int (*)(int*, int);
```



如果我们明确知道返回的函数是哪一个，就能使用decltype简化书写函数指针返回类型的过程

```c++
string::size_type sumLength(const string&, const string&);
decltype(sumLength) *getFcn(const string &);
```



## 第七章   类



类的基本思想是==数据抽象==和==封装==。

数据抽象是一种依赖于接口和实现分离的编程技术。

类的接口包括用户所能执行的操作；类的实现则包括类的数据成员、负责接口实现的函数体以及定义类所需的各种私有函数。

封装实现了类的接口和实现的分离。



## 7.1 定义抽象数据类型

抽象数据类型不允许访问它的数据成员，只能通过接口进行操作。



1. **设计Sales_data类**

将Sales_data类变成抽象数据结构



Sales_data接口应该包含以下操作：

* 一个isbn成员函数
* 一个combine成员函数
* 一个名为add的函数
* 一个read函数
* 一个print函数



2. **定义改进的Sales_data类**

成员函数的声明必须在类的内部，它的定义则既可以在类的内部也可以在类的外部

作为接口组成部分的非成员函数，例如add、read和print等，它们的定义和声明都在类的外部



```c++
struct Sales_data{
    std::string isbn() const { return bookNo; }
    Sales_data& combine(const Sales_data&);
    double avg_price() const;
    
    std::string bookNo;
    unsigned units_sold = 0;
    double revenue = 0.0;
}

//非成员接口函数
Sales_data add (const Sales_data&, const Sales_data&);
std::ostream &print (std::ostream&, const Sales_data&);
std::istream &read (std::istream&, Sales_data&);
```



尽管所有成员都必须在类的内部声明，但是成员函数体可以定义在类内也可以定义在类外



当我们调用成员函数时，实际上是在替某个对象调用它

如果isbn指向Sales_data的成员（例如bookNo），则它隐式地指向调用该函数的对象的成员



成员函数通过一个名为this的额外的隐式参数来访问调用它的那个对象

任何对类成员的直接访问都被看作this的隐式引用，也就是说，当isbn使用bookNo时，它隐式地使用this指向的成员

this是常量指针



isbn函数的另一个关键之处是紧随参数列表之后的const关键字，这里，const的作用是修改隐式this指针的类型

默认情况下，this的类型是指向类类型非常量版本的常量指针

例如在Sales_data成员函数中，this的类型是Sales_data ＊const



C++语言的做法是允许把const关键字放在成员函数的参数列表之后，此时，紧跟在参数列表后面的const表示this是一个指向常量的指针

像这样使用const的成员函数被称作常量成员函数



常量对象，以及常量对象的引用或指针都只能调用常量成员函数



编译器分两步处理类：首先编译成员的声明，然后才轮到成员函数体（如果有的话）

因此，成员函数体可以随意使用类中的其他成员而无须在意这些成员出现的次序



类外部定义的成员的名字必须包含它所属的类名

```c++
double Sales_data::avg_price() const 
{
    if(units_sold)
        return revenue / units_sold;
    else
        return 0;
}
```

定义了一个名为avg_price的函数，并且该函数被声明在类Sales_data的作用域内

当avg_price使用revenue和units_sold时，实际上它隐式地使用了Sales_data的成员



返回this对象的函数

```c++
Sales_data& Sales_data:combine(const Sales_data &rhs)
{
    units_sold += rhs.units_sold;
    revenue += rhs.revenue;
    return *this;
}
```



3. **定义类相关的非成员函数**

需要定义一些辅助函数

尽管这些函数定义的操作从概念上来说属于类的接口的组成部分，但它们实际上并不属于类本身



一般来说，如果非成员函数是类接口的组成部分，则这些函数的声明应该与类在同一个头文件内



```c++
Sales_data add(const Sales_data &lhs, const Sales_data &rhs)
{
    Sales_data sum = lhs;
    sum.combine(rhs);
    return suml
}
```

我们用lhs的副本来初始化sum。默认情况下，拷贝类的对象其实拷贝的是对象的数据成员



4. **构造函数**

每个类都分别定义了它的对象被初始化的方式，类通过一个或几个特殊的成员函数来控制其对象的初始化过程，这些函数叫做构造函数



==构造函数的名字和类名相同==

构造函数没有返回类型

构造函数也有一个（可能为空的）参数列表和一个（可能为空的）函数体

 类可以包含多个构造函数，和其他重载函数差不多



不同于其他成员函数，构造函数不能被声明成const的

构造函数在const对象的构造过程中可以向其写值



类通过一个特殊的构造函数来控制默认初始化过程，这个函数叫做默认构造函数（default constructor）

默认构造函数无须任何实参



若没有显式定义构造函数，则编译器会隐式定义一个默认构造函数

编译器创建的构造函数又称为合成的默认构造函数（synthesized default constructor）

* 若存在类内初始值，用它来初始化成员
* 否则，默认初始化该成员



合成的默认构造函数只适合非常简单的类

对于一个普通的类来说，必须定义它自己的默认构造函数

原因：

* 编译器只有在发现类不包含任何构造函数的情况下才会替我们生成一个默认的构造函数

* 对于某些类来说，合成的默认构造函数可能执行错误的操作

  如果定义在块中的内置类型或复合类型（如数组或指针）的对象被默认初始化，则他们的值将是未定义的

* 有的时候编译器不能为某些类合成默认的构造函数

​		例如，如果类中包含一个其他类类型的成员且这个成员的类型没有默认构造函数，那么编译器将无法初始化该成员



如果我们需要默认的行为，那么可以通过在参数列表后面写上= default来要求编译器生成构造函数

```c++
struct Sales_data
{
    Sales_Data() = default;
    ...
}
```



```c++
Sales_data(const std::string &s):bookNo(s) {}
Sales_data(const std:: string &s, unsigned n, double p):
	bookNo(s), units_sold(n), revenue(p*n) {}
```

冒号和花括号之间的代码为构造函数初始值列表（constructor initialize list）

它负责为新创建的对象的一个或几个数据成员赋初值

构造函数初始值是成员名字的一个列表，每个名字后面紧跟括号括起来的（或者在花括号内的）成员初始值



当某个数据成员被构造函数初始值列表忽略时，它将以与合成默认构造函数相同的方式隐式初始化



和其他成员函数一样，当我们在类的外部定义构造函数时，必须指明该构造函数是哪个类的成员

```c++
Sales_data::Sales_data(std::istream &is)
{
    read(is, *this);
}
```



5. **拷贝、赋值和析构**



对象在几种情况下会被拷贝，如初始化变量以及以值的方式传递或返回一个对象等

当我们使用了赋值运算符时会发生对象的赋值操作

当对象不再存在时执行销毁的操作，比如一个局部对象会在创建它的块结束时被销毁



如果我们不主动定义这些操作，则编译器将替我们合成它们



对于某些类来说合成的版本无法正常工作

特别是，当类需要分配类对象之外的资源时，合成的版本常常会失效



很多需要动态内存的类能（而且应该）使用vector对象或者string对象管理必要的存储空间

如果类包含vector或者string成员，则其拷贝、赋值和销毁的合成版本能够正常工作



## 7.2 访问控制与封装

在C++语言中，我们使用访问说明符（access specifiers）加强类的封装性

* 定义在public说明符之后的成员在整个程序内可被访问，public成员定义类的接口
* 定义在private说明符之后的成员可以被类的成员函数访问，但是不能被使用该类的代码访问，private部分封装了（即隐藏了）类的实现细节



构造函数和部分成员函数（即isbn和combine）紧跟在public说明符之后；而数据成员和作为实现部分的函数则跟在private说明符后面

一个类可以包含0个或多个访问说明符



我们使用了class关键字而非struct开始类的定义

这种变化仅仅是形式上有所不同



类可以在它的第一个访问说明符之前定义成员，对这种成员的访问权限依赖于类定义的方式

如果我们使用struct关键字，则定义在第一个访问说明符之前的成员是public的；相反，如果我们使用class关键字，则这些成员是private的



1. **友元**

类可以允许其他类或者函数访问它的非公有成员，方法是令其他类或者函数成为它的友元（friend）

```c++
class Sales_data
{
    //为Sales_data的非成员函数做的友元声明
    friend Sales_data add(const Sales_data&, const Sales_data&)
    friend std::istream &read(std::istream&, Sales_data&)
    friend std::ostream &print(std::ostream&, const Sales_data&);
    
    //其他成员不变
}

//Sales_data接口的非成员函数组成部分的声明
Sales_data add (const Sales_data&, const Sales_data&);
std::ostream &print (std::ostream&, const Sales_data&);
std::istream &read (std::istream&, Sales_data&);
```



友元声明只能出现在类定义的内部

友元不是类的成员也不受它所在区域访问控制级别的约束



封装优点：

* 确保用户代码不会无意间破坏封装对象的状态
* 被封装的类的实现可以随时改变，而无须调整用户级别的代码



数据成员定义为private

* 可以比较自由地修改数据
* 能防止由于用户的原因造成数据被破坏



友元的声明仅仅指定了访问的权限，而非一个通常意义上的函数声明

如果我们希望类的用户能够调用某个友元函数，那么我们就必须在友元声明之外再专门对函数进行一次声明

为了使友元对类的用户可见，我们通常把友元的声明与类本身放置在同一个头文件中（类的外部）



## 7.3 类的其他特性

1. **类成员再探**

定义一对相互关联的类，分别是Screen和Window_mgr

Screen包含一个保存内容的string，三个string::size_type成员分别表示光标位置及屏幕高和宽

还需要添加构造函数，负责移动光标和读取给定位置字符的成员

```c++
class Screen
{
    public:
    typedef std::string::size_type pos;
    Screen() = default;
    Screen(pos ht, pos wd, char c) : height(ht), width(wd),
    contents(ht * wd, c) {}
    
    char get() const { return contents[cursor]; } //隐式内联
    inline char get(pos ht, pos wd) const; //显式内联
    Screen &move(pos r, pos c); //能在之后被设为内联
    
    private:
    pos cursor = 0;
    pos height = 0, width = 0;
    std::string contents;
}
```



在类中，常有一些规模较小的函数适合于被声明成内联函数

定义在类内部的成员函数是自动inline



我们可以在类的内部把inline作为声明的一部分显式地声明成员函数

同样的，也能在类的外部用inline关键字修饰函数的定义

最好只在类外部定义的地方说明inline，这样可以使类更容易理解



和非成员函数一样，成员函数也可以被重载



我们希望能修改类的某个数据成员，即使是在一个const成员函数内

可以通过在变量的声明中加入mutable关键字做到这一点



一个可变数据成员（mutable data member）永远不会是const，即使它是const对象的成员

因此，一个const成员函数可以改变一个可变成员的值



```c++
class Screen
{
    public:
    	void some_member() const;
    private:
    	mutable size_t access_ctr;
};

void Screen::some_member() const
{
    ++access_ctr;
}
```

尽管some_member是一个const成员函数，它仍然能够改变access_ctr的值

该成员是个可变成员，因此任何成员函数，包括const函数在内都能改变它的值



定义一个窗口管理类，表示一组Screen

```c++
class Window_mgr
{
    private:
    std::vector<Screen> screens { Screen(24, 80, ' ') };
};
```



2. **返回*this的成员函数**

```c++
class Screen
{
    public:
    Screen &set(char);
    Screen &set(pos, pos, char);
};

inline Screen &Screen::set(char c)
{
    contents[cursor] = c;
    return *this;
}
inline Screen &Screen::set(pos r, pos col, char ch)
{
    contents[r*width + col] = ch;
    return *this;
}
```



返回引用的函数是左值的，意味着这些函数返回的是对象本身而非对象的副本



定义一个display操作，为const成员，此时this是一个指向const的指针，而*this是const对象



可以通过区分成员函数是否是const的，对其进行重构

因为非常量版本的函数对于常量对象是不可用的，所以我们只能在一个常量对象上调用const成员函数





## 7.4 类的作用域

在类的作用域之外，普通的数据和函数成员只能由对象、引用或者指针使用成员访问运算符来访问

对于类类型成员则使用作用域运算符访问



一旦遇到了类名，定义的剩余部分就在类的作用域之内了



函数的返回类型通常出现在函数名之前

因此当成员函数定义在类的外部时，返回类型中使用的名字都位于类的作用域之外，因此需指明它是哪个类的成员

```c++
class Window_mgr
{
    public: ScreenIndex addScreen(const Screen&);
}

Window_mgr::ScreenIndex Window_mgr::addScreen(const Screen &s)
{
    screens.push_back(s);
    return screens.size() -1;
}
```



1. **名字查找与类的作用域**





## 7.6 类的静态成员

有的时候类需要它的一些成员与类本身直接相关，而不是与类的各个对象保持关联

我们通过在成员的声明之前加上关键字static使得其与类关联在一起



银行账户：

```c++
class Account
{
    public:
    void calculate() { amount += amount * interestRate; }
    static double rate() { return interestRate; }
    static void rate(double);
    
    private:
    std::string owner;
    double amount;
    static double interestRate;
    static double initRate();
}
```

类的静态成员存在于任何对象之外，对象中不包含任何与静态数据成员有关的数据

只存在一个interestRate对象而且它被所有Account对象共享



静态成员函数也不与任何对象绑定在一起，它们不包含this指针

静态成员函数不能声明成const的，而且我们也不能在static函数体内使用this指针



使用作用域运算符直接访问静态成员

```c++
double r;
r = Account::rate();
```



虽然静态成员不属于类的某个对象，但是我们仍然可以使用类的对象、引用或者指针来访问静态成员

```c++
Account ac1;
Account *ac2 = &ac1;
r = ac1.rate();
r = ac2->rate();
```



成员函数能直接使用静态成员





# 第十章   泛型算法

标准库并未给每个容器都定义成员函数来实现这些操作，而是定义了一组泛型算法

generic algorithm

这些算法是通用的，可用于不同类型的容器和不同类型的元素



## 10.1 概述

大多数算法都定义在头文件algorithm中

头文件numeric中还定义了一组数值泛型算法



这些算法并不直接操作容器，而是遍历由两个迭代器指定的一个元素范围

例如，查找vector中是否包含特定值，可以用标准库算法find

它返回指向第一个等于给定值的元素的迭代器；如果范围中无匹配元素，则find返回第二个参数来表示搜索失败。

```c++
int val = 42;
auto result = find(vec.cbegin(), vec.cend(), val);

//指针就像数组上的迭代器一样，可以用find在数组中查找值
int ia[] = {1,2,3,4};
int val = 3;
int *result = find(begin(ia), end(ia), val);

//指定范围查找
auto result = find(ia+1, ia+4, val);
```



迭代器令算法不依赖于容器，但算法依赖于元素类型的操作

泛型算法本身不会执行容器的操作，它们只会运行于迭代器之上，执行迭代器的操作

因此，算法可能改变容器中保存的元素的值，也可能在容器内移动元素，但永远不会直接添加或删除元素



## 10.2 初识泛型算法

除了少数例外，标准库算法都对一个范围内的元素进行操作

接受输入范围的算法总是使用前两个参数来表示此范围，两个参数分别是指向要处理的第一个元素和尾元素之后位置的迭代器



1. **只读算法**

一些算法只会读取其输入范围内的元素，而从不改变元素

find函数、count函数都是

头文件numeric中accumulate函数也是



accumulate函数接受三个参数，前两个指出了需要求和的元素的范围，第三个参数是和的初值

```c++
int sum = accumulate(vec.cbegin(), vec.cend(), 0);
```



由于string定义了+运算符，所以我们可以通过调用accumulate来将vector中所有string元素连接起来

```c++
string sum = accumulate(v.cbegin(), v.cend(), string(""));
```

第三个参数显式地创建了一个string；将空串当做一个字符串字面值传递给第三个参数是不可以的，会导致一个编译错误

```c++
//错误：const char*上没有定义+运算符
string sum = accumulate(v.cbegin(), v.cend(), "");
```



 另一个只读算法是equal，用于确定两个序列是否保存相同的值

此算法接受三个迭代器：前两个（与以往一样）表示第一个序列中的元素范围，第三个表示第二个序列的首元素

```c++
//roster2中元素数目应至少与roster1一样多
equal(roster1.cbegin(), roster1.cend(), roster2.cbegin());
```



2. **写容器元素的算法**

当我们使用这类算法时，必须注意确保序列原大小至少不小于我们要求算法写入的元素数目

算法不会执行容器操作，因此它们自身不可能改变容器的大小



一些算法会自己向输入范围写入元素，本质上并不危险，最多写入与给定序列一样多的元素

例如，fill将给定的这个值赋予输入序列中的每个元素

```c++
fill(vec.begin(), vec.end(), 0);
```



用一个单一迭代器表示第二个序列的算法都假定第二个序列至少与第一个一样长

确保算法不会试图访问第二个序列中不存在的元素是程序员的责任



一些算法接受一个迭代器来指出一个单独的目的位置

例如，函数fill_n接受一个单迭代器、一个计数值和一个值，要保证至少包含这么多个元素

```c++
fill_n(vec.begin(), vec.size(), 0);
```



一种保证算法有足够元素空间来容纳输出数据的方法是使用插入迭代器（insert iterator）

插入迭代器是一种向容器中添加元素的迭代器

当我们通过一个插入迭代器赋值时，一个与赋值号右侧值相等的元素被添加到容器中



back_inserter，它是定义在头文件iterator中的一个函数

back_inserter接受一个指向容器的引用，返回一个与该容器绑定的插入迭代器

```c++
vector<int> vec;
auto it = back_inserter(vec);
*it = 42;
```



常常使用back_inserter来创建一个迭代器，作为算法的目的位置来使用

```c++
vector<int> vec;
fill_n(back_inserter(vec), 10, 0); //添加10个元素到vec
```



拷贝（copy）算法是另一个向目的位置迭代器指向的输出序列中的元素写入数据的算法

此算法将输入范围中的元素拷贝到目的序列中

传递给copy的目的序列至少要包含与输入序列一样多的元素



3. **重排容器元素的算法**

某些算法会重排容器中元素的顺序，一个明显的例子是sort

```c++
void elimDups(vector<string> &words)
{
    sort(words.begin(), words.end());
    
    //返回指向不重复区域之后一个位置的迭代器
    auto end_unique = unique(words.begin(),words.end()); 
    
    words.earase(end_unique, words.end());
}

```



unique并不真的删除任何元素，它只是覆盖相邻的重复元素，使得不重复元素出现在序列开始部分

unique返回的迭代器指向最后一个不重复元素之后的位置

此位置之后的元素仍然存在，但我们不知道它们的值是什么



为了真正地删除无用元素，我们必须使用容器操作，本例中使用erase

删除从end_unique开始直至words末尾的范围内的所有元素



## 10.3 定制操作

很多算法都会比较输入序列中的元素

默认情况下，这类算法使用元素类型的<或==运算符完成比较

标准库还允许我们提供自己定义的操作来代替默认运算符



1. **向算法传递函数**

假定希望在调用elimDups后打印vector的内容

此外还假定希望单词按其长度排序，大小相同的再按字典序排列

为了按长度重排vector，我们将使用sort的第二个版本，此版本是重载过的，它接受第三个参数，此参数是一个谓词（predicate）



谓词是一个可调用的表达式，其返回结果是一个能用作条件的值

谓词分两类：

* 一元谓词：只接受单一参数
* 二元谓词：有两个参数



接受谓词参数的算法对输入序列中的元素调用谓词

因此，元素类型必须能转换为谓词的参数类型

接受一个二元谓词参数的sort版本用这个谓词代替<来比较元素



```c++
bool isShorter(const string &s1, const string &s2)
{
    return s1.size() < s2.size();
}

sort(words.begin(), words.end(), isShorter);
```



对words进行字典序排列后，可以使用stable_sort算法排序不同长度词语，这种稳定排序算法维持相等元素的原有顺序

```c++
//定义相等关系表示具有相同长度
stable_sort(words.begin(),words.end(), isShorter);
```



2. **lambda表达式**

根据算法接受一元谓词还是二元谓词，我们传递给算法的谓词必须严格接受一个或两个参数

有时我们希望进行的操作需要更多参数，超出了算法对谓词的限制



find_if第三个参数为一个谓词，算法对输入序列中的每个元素调用给定的这个谓词

它返回第一个使谓词返回非0值的元素，如果不存在这样的元素，则返回尾迭代器



我们可以向一个算法传递任何类别的可调用对象（callable object）

对于一个对象或一个表达式，如果可以对其使用调用运算符，则称它为可调用的

到目前为止，我们使用过的仅有的两种可调用对象是函数和函数指针

还有两种可调用对象：

* 重载了函数调用运算符的类
* lambda表达式



一个lambda表达式表示一个可调用的代码单元

一个lambda具有一个返回类型、一个参数列表和一个函数体

```c++
[捕获列表](参数列表)-> 返回类型 {函数体}
```



* 捕获列表：capture list，lambda所在函数中定义的局部变量的列表
* 参数列表、返回类型、函数体与普通函数一致

我们可以忽略参数列表和返回类型，但必须永远包含捕获列表和函数体

```c++
//定义了一个可调用对象f，它不接受参数，返回42
auto f = [] {return 42;};

//调用方式与普通函数调用方式相同，都是使用调用运算符
cout << f() << endl;
```



如果忽略返回类型，lambda根据函数体中的代码推断出返回类型



与一个普通函数调用类似，调用一个lambda时给定的实参被用来初始化lambda的形参

lambda不能有默认参数，因此，lambda调用的实参数目永远与形参数目相等

```c++
[](const string &a, const string &b) { return a.size < b.size(); }
```



虽然一个lambda可以出现在一个函数中，使用其局部变量，但它只能使用那些明确指明的变量

一个lambda通过将局部变量包含在其捕获列表中来指出将会使用这些变量



```c++
//查找第一个长度大于等于sz的元素
auto wc = find_if(words.begin(), words.end, 
                 [sz](const string &a) {return a.size >= sz;});
```



捕获列表只用于局部非static变量，lambda可以直接使用局部static变量和在它所在函数之外声明的名字



3. **lambda捕获和返回**

当向一个函数传递一个lambda时，同时定义了一个新类型和该类型的一个对象：传递的参数就是此编译器生成的类类型的未命名对象

类似的，当使用auto定义一个用lambda初始化的变量时，定义了一个从lambda生成的类型的对象



默认情况下，从lambda生成的类都包含一个对应该lambda所捕获的变量的数据成员

lambda的数据成员也在lambda对象创建时被初始化



类似参数传递，变量的捕获方式也可以是值或引用

与传值参数类似，采用值捕获的前提是变量可以拷贝

与参数不同，被捕获的变量的值是在lambda创建时拷贝，而不是调用时拷贝

```c++
void func1()
{
    int v1 = 42;
    auto f = [v1] { return v1; }; //将v1拷贝到名为f的可调用对象
    v1 = 0;
    auto j = f(); //j为42，f保存了创建时v1的拷贝
}
```



我们定义lambda时可以采用引用方式捕获变量

```c++
void func2()
{
    int v1 = 42;
    auto f2 = [&v1] { return v1; };
    v1 = 0;
    auto j = f2();//j为0，f2保存v1的引用，而非拷贝
}
```

v1之前的&指出v1应该以引用方式捕获

一个以引用方式捕获的变量与其他任何类型的引用的行为类似



如果我们采用引用方式捕获一个变量，就必须确保被引用的对象在lambda执行的时候是存在的

lambda捕获的都是局部变量，这些变量在函数结束后就不复存在了

如果lambda可能在函数结束后执行，捕获的引用指向的局部变量已经消失



我们也可以从一个函数返回lambda，此lambda不能包含引用捕获

函数可以直接返回一个可调用对象，或者返回一个类对象，该类含有可调用对象的数据成员



捕获一个普通变量，如int、string或其他非指针类型，通常可以采用简单的值捕获方式

如果我们捕获一个指针或迭代器，或采用引用捕获方式，就必须确保在lambda执行时，绑定到迭代器、指针或引用的对象仍然存在

应该尽量减少捕获的数据量，该避免捕获指针或引用



隐式捕获：除了显式列出我们希望使用的来自所在函数的变量之外，还可以让编译器根据lambda体中的代码来推断我们要使用哪些变量

为了指示编译器推断捕获列表，应在捕获列表中写一个&或=

&告诉编译器采用捕获引用方式，=则表示采用值捕获方式



当我们混合使用隐式捕获和显式捕获时，捕获列表中的第一个元素必须是一个&或=

此符号指定了默认捕获方式为引用或值

```c++
char c = ' ';
for_each(words.begin(),words.end(),
        [&, c](const string &s) { ... });
```



当混合使用隐式捕获和显式捕获时，显式捕获的变量必须使用与隐式捕获不同的方式



默认情况下，对于一个值被拷贝的变量，lambda不会改变其值

如果我们希望能改变一个被捕获的变量的值，就必须在参数列表首加上关键字mutable

```c++
void fcn3()
{
    size_t v1 = 42;
    auto f = [v1]() mutable {return ++v1; };
    v1 = 0;
    auto j = f(); //j为43
}
```



默认情况下，如果一个lambda体包含return之外的任何语句，则编译器假定此lambda返回void

与其他返回void的函数类似，被推断返回void的lambda不能返回值

```c++
//错误，推断lambda返回为void，但它返回了一个int
transform(vi.begin(), vi.end(), vi.begin(),
         [](int i) {if (i<0) return -i; else return i;});

//需要为一个lambda定义返回类型时，必须使用尾置返回类型
transform(vi.begin(), vi.end(), vi.begin(),
         [](int i) ->  {if (i<0) return -i; else return i;})
```



4. **参数绑定**

对于那种只在一两个地方使用的简单操作，lambda表达式是最有用的

如果我们需要在很多地方使用相同的操作，通常应该定义一个函数

如果一个操作需要很多语句才能完成，通常使用函数更好

如果lambda的捕获列表为空，通常可以用函数来代替它



find_if接受一元谓词，因此不能用以下函数作为find_if的一个参数

```c++
bool check_size(const string &s, string::size_type sz)
{
    return s.size() >= sz;
}
```



标准库函数bind，定义在头文件functional中

可以将bind函数看作一个通用的函数适配器

它接受一个可调用对象，生成一个新的可调用对象来“适应”原对象的参数列表



调用形式：

```c++
auto newCallable = bind(callable, arg_list);
```

* arg_list是逗号分隔的参数列表，对应callable的参数，即当我们调用newCallable时，newCallable会调用callable，并传递给它arg_list中的参数
* arg_list中有_n表示占位符，表示newCallable的参数，如 _1表示newCallable的第一个参数



```c++
auto check6 = bind(check_size, _1, 6);
```

此bind调用只有一个占位符，表示check6只接受单一参数



名字_n都定义在一个名为placeholders的命名空间中，而这个命名空间本身定义在std命名空间中

对bind的调用代码假定之前已经恰当地使用了using声明

```c++
using std::placeholders::_1;
```



对每个占位符名字，我们都必须提供一个单独的using声明

可以使用另外一种不同形式的using语句，而不是分别声明每个占位符

```c++
using namespace std::placeholders;
```



可以用bind绑定给定可调用对象中的参数或重新安排其顺序

```c++
auto g = bind(f, a, b, _2, c, _1);
```

生成一个新的可调用对象，它有两个参数，分别用占位符 _2和 _1表示

这个新的可调用对象将它自己的参数作为第三个和第五个参数传递给f



```c++
g(X, Y) 映射为  f(a, b, Y, c,)
```



如果我们希望传递给bind一个对象而又不拷贝它，就必须使用标准库ref函数









# 第十一章   关联容器

关联容器中的元素是按关键字来保存和访问的

两个主要的关联容器（associative-container）类型是map和set

map中的元素是一些关键字-值（key-value）对；set中每个元素只包含一个关键字



标准库提供8个容器，每个容器：

* 或者是一个set，或者是一个map
* 或者要求不重复的关键字，或者允许重复关键字
* 按顺序保存元素，或无序保存

允许重复关键字的容器的名字中都包含单词multi

不保持关键字按顺序存储的容器的名字都以单词unordered开头



* 按关键字有序保存元素：map、set、multimap、multiset
* 无序集合：unordered_map、unordered_set、unordered_multimap、unordered_multiset



## 11.1 使用关联容器

map是关键字-值对的集合，通常被称为关联数组

当从map中提取一个元素时，会得到一个pair类型的对象

pair是一个模板类型，保存两个名为first和second的（公有）数据成员

map所使用的pair用first成员保存关键字，用second成员保存对应的值



## 11.2 关联容器概述

1. **定义关联容器**

当定义一个map时，必须既指明关键字类型又指明值类型

而定义一个set时，只需指明关键字类型，因为set中没有值



```c++
map<string, size_t> word_count; //空容器
set<string> exclude = {"the", "but", "and"};
map<string, string> authors = {
    {"Austen", "Jane"},
    {"Dickens", "Charles"}
};
```



一个map或set中的关键字必须是唯一的

容器multimap和multiset没有此限制，它们都允许多个元素具有相同的关键字



2. **关键字类型的要求**

有序容器的关键字类型：

可以向一个算法提供我们自己定义的比较操作，比较函数需具备以下基本性质：

* 两个关键字不能同时“小于等于”对方；
  * 如果k1“小于等于”k2，那么k2绝不能“小于等于”k1
* 如果k1“小于等于”k2，且k2“小于等于”k3，那么k1必须“小于等于”k3
* 如果存在两个关键字，任何一个都不“小于等于”另一个，那么我们称这两个关键字是“等价”的。
  * 如果k1“等价于”k2，且k2“等价于”k3，那么k1必须“等价于”k3。



用尖括号指出要定义哪种类型的容器，自定义的操作类型必须在尖括号中紧跟着元素类型给出。

例如，我们不能直接定义一个Sales_data的multiset，因为Sales_data没有<运算符



```c++
bool compareIsbn(const Sales_data &lhs, const Sales_data &rhs)
{
    return lhs.isbn() < rhs.isbn();
}

multiset<Sales_data, decltype(compareIsbn)*>
    			bookstore(compareIsbn);
```

用compareIsbn来初始化bookstore对象，这表示当我们向bookstore添加元素时，通过调用compareIsbn来为这些元素排序



3. **pair类型**

pair是一个用来生成特定类型的模板

```c++
pair<string, string> anon;
pair<string, string> author{"James", "Joyce"};
```



|          pair操作          |               解释               |
| :------------------------: | :------------------------------: |
|      pair<T1,  T2> p;      |   p是一个pair，进行了值初始化    |
|  pair<T1, T2> p(v1, v2);   |       使用v1和v2进行初始化       |
| pair<T1, T2> p = {v1, v2}; |               同上               |
|     make_pair(v1, v2);     | 返回一个pair，类型从v1和v2推断出 |



## 11.3 关联容器操作

|      |      |
| :--: | :--: |
|      |      |
|      |      |
|      |      |



# 第十二章   动态内存

全局对象在程序启动时分配，在程序结束时销毁

对于局部自动对象，当我们进入其定义所在的程序块时被创建，在离开块时销毁

局部static对象在第一次使用前分配，在程序结束时销毁



除自动和static对象外，C++还支持动态分配对象

动态分配的对象的生存期与它们在哪里创建是无关的，只有当显式地被释放时，这些对象才会销毁



标准库定义了两个智能指针类型来管理动态分配的对象

当一个对象应该被释放时，指向它的智能指针可以确保自动地释放它



程序存储变量：

* 静态内存：用来保存局部static对象、类static数据成员以及定义在任何函数之外的变量
* 栈内存：用来保存定义在函数内的非static对象
* 堆内存：用来存储动态分配（dynamically allocate）的对象，即那些在程序运行时分配的对象



对于栈对象，仅在其定义的程序块运行时才存在；static对象在使用之前分配，在程序结束时销毁

动态对象的生存期由程序来控制，也就是说，当动态对象不再使用时，我们的代码必须显式地销毁它们



## 12.1 动态内存与智能指针

动态内存管理通过一对运算符完成：

* new：在动态内存中为对象分配空间并返回一个指向该对象的指针，我们可以选择对对象进行初始化
* delete：接受一个动态对象的指针，销毁该对象，并释放与之关联的内存



有时我们会忘记释放内存，在这种情况下就会产生内存泄漏

有时在尚有指针引用内存的情况下我们就释放了它，在这种情况下就会产生引用非法内存的指针



新的标准库提供了两种智能指针（smart pointer）类型来管理动态对象

* shared_ptr：允许多个指针指向同一个对象
* unique_ptr：则“独占”所指向的对象
* weak_ptr：伴随类，它是一种弱引用，指向shared_ptr所管理的对象

都定义在memory头文件中



1. **shared_ptr类**

类似vector，智能指针也是模板

因此创建智能指针时，必须提供指针指向的类型

```c++
shared_ptr<string> p1;
```



默认初始化的智能指针中保存着一个空指针

解引用一个智能指针返回它指向的对象

如果在一个条件判断中使用智能指针，效果就是检测它是否为空

```c++
//如果p1不为空，检查它是否指向一个空string
if(p1 && p1->empty())
    *p1 = "hi";
```



shared_ptr和unique_ptr都支持的操作：

|        操作         |                说明                |
| :-----------------: | :--------------------------------: |
| shared_ptr<T>    sp |   空智能指针，可以指向类型T对象    |
|          p          | 用作条件判断，指向一个对象，为true |
|         *p          |               解引用               |
|       p->mem        |                                    |
|       p.get()       |          返回p中保存指针           |
|     swap(p, q)      |                                    |
|      p.swap(q)      |                                    |



shared_ptr独有操作

|          操作           |                   说明                    |
| :---------------------: | :---------------------------------------: |
| make_shared<T>   (args) |  返回一个shared_ptr，使用args初始化对象   |
|  shared_ptr<T>   p(q)   |  p是shared_ptr  q的拷贝，会递增q中计数器  |
|           p=q           |     会递减p的引用计数，递增q引用计数      |
|       p.unique()        | 若p.use_count()为1，返回true，否则为false |
|      p.use_count()      |        返回p共享对象的智能指针数量        |



最安全的分配和使用动态内存的方法是调用一个名为make_shared的标准库函数

此函数在动态内存中分配一个对象并初始化它，返回指向此对象的shared_ptr

```c
shared_ptr<int> p3 = make_shared<int>(42);

auto p4 = make_shared<string>(10, '9');
```



当进行拷贝或赋值操作时，每个shared_ptr都会记录有多少个其他shared_ptr指向相同的对象

```c++
auto p = make_shared<int>(42); //p指向对象只有p一个引用者
auto q(p); //p和q指向相同对象，此对象有两个引用者
```



可以认为每个shared_ptr都有一个关联的计数器，通常称其为引用计数（reference count）

无论何时我们拷贝一个shared_ptr，计数器都会递增。



当我们给shared_ptr赋予一个新值或是shared_ptr被销毁（例如一个局部的shared_ptr离开其作用域）时，计数器就会递减



一旦一个shared_ptr的计数器变为0，它就会自动释放自己所管理的对象



```c++
auto r = make_shared<int>(42);
r = q; //给r赋值，令他指向另一个地址
	   //递增q指向对象的引用计数
       //递减r原来指向对象的引用计数
       //r原来指向对象没有引用者，自动释放
```



shared_ptr类通过析构函数（destructor）完成销毁工作

类似于构造函数，每个类都有一个析构函数

析构函数一般用来释放对象所分配的资源



对于一块内存，shared_ptr类保证只要有任何shared_ptr对象引用它，它就不会被释放掉

如果你将shared_ptr存放于一个容器中，而后不再需要全部元素，而只使用其中一部分，要记得用erase删除不再需要的那些元素



使用动态内存原因：

* 程序不知道自己需要使用多少对象
* 程序不知道所需对象的准确类型
* 程序需要在多个对象间共享数据



某些类分配的资源具有与原对象相独立的生存期

一般而言，如果两个对象共享底层的数据，当某个对象被销毁时，我们不能单方面地销毁底层数据

```c++
Blob<string> b1; //空blob

{
    //新作用域
    Blob<string> b2 = {"a", "an", "the"};
    b1 = b2; //b1和b2共享相同的元素
}

//b2被销毁了，但b2中元素不能销毁
//b1指向最初由b2创建的元素
```

使用动态内存的一个常见原因是允许多个对象共享相同的状态



定义一个管理string的类，命名为StrBlob

实现一个新的集合类型的最简单方法是使用某个标准库容器来管理元素

采用这种方法，我们可以借助标准库类型来管理元素所使用的内存空间



但是，我们不能在一个Blob对象内直接保存vector，因为一个对象的成员在对象销毁时也会被销毁

为了保证vector中的元素继续存在，我们将vector保存在动态内存中

我们为每个StrBlob设置一个shared_ptr来管理动态分配的vector

此shared_ptr的成员将记录有多少个StrBlob共享相同的vector，并在vector的最后一个使用者被销毁时释放vector



```c++
class StrBlob
{
    public:
    typedef vector<string>::size_type   size_type;
    StrBlob();
    StrBlob(initializer_list<string> il);
    size_type size() const { return data->size(); }
    bool empty() const { return data-> empty(); }
    
    void push_back(const string &t) { data->push_back(t); }
    void pop_back();
    
    string& front();
    string& back(); 
    
    private:
    shared_ptr<vector<string>> data;
    void check(size_type i, const string &msg) const;    
};
```



我们的StrBlob类只有一个数据成员，它是shared_ptr类型

因此，当我们拷贝、赋值或销毁一个StrBlob对象时，它的shared_ptr成员会被拷贝、赋值或销毁

对于由StrBlob构造函数分配的vector，当最后一个指向它的StrBlob对象被销毁时，它会随之被自动销毁



2. **直接管理内存**

C++中使用new分配内存，delete释放new分配的内存

相对于智能指针，使用这两个运算符管理内存非常容易出错，使用智能指针的程序更容易编写和调试



在自由空间分配的内存是无名的，因此new无法为其分配的对象命名，而是返回一个指向该对象的指针

默认情况下，动态分配的对象是默认初始化的

```c++
int *pi = new int; //pi指向一个动态分配的、未初始化的无名对象
```



我们可以使用直接初始化方式来初始化一个动态分配的对象

```c++
int *pi = new int(1024);
vector<int> *pv = new vector<int>{0,1,2,3,4};
```



如果我们提供了一个括号包围的初始化器，就可以使用auto

从此初始化器来推断我们想要分配的对象的类型

```c++
auto p1 = new auto(obj); //p指向一个与obj类型相同的对象
auto p2 = new auto{a, b, c}; //错误，括号中只能有单个初始化器
```



用new分配const对象是合法的

```c++
//分配并初始化一个const int
const int *pci = new const int(1024);

//分配并默认初始化一个const的空string
const string *pcs = new const string;
```

一个动态分配的const对象必须进行初始化

由于分配的对象是const的，new返回的指针是一个指向const的指针



一旦一个程序用光了它所有可用的内存，new表达式就会失败

会抛出一个类型为bad_alloc的异常

```c++
int *p1 = new int; //如果分配失败，new抛出bad_alloc

//如果分配失败，返回一个空指针
//我们称这种形式的new为定位new,定位new表达式允许我们向new传递额外的参数
int *p2 = new (nothrow) int; 
```



通过delete表达式（delete expression）来将动态内存归还给系统

delete表达式也执行两个动作：销毁给定的指针指向的对象；释放对应的内存

```c++
delete p;
```



我们传递给delete的指针必须指向动态分配的内存，或者是一个空指针

释放一块并非new分配的内存，或者将相同的指针值释放多次，其行为是未定义的

```c++
int i, *pi1 = &i, *pi2 = nullptr;
double *pd = new double(33), *pd2 = pd;

delete i; //错误，i不是一个指针
delete pi1; //未定义：pi1指向一个局部变量
delete pd; //正确
delete pd2; //未定义：pd2指向的内存已经被释放
delete pi2; //正确
```



虽然一个const对象的值不能被改变，但它本身是可以被销毁的

```c++
const int *pci = new const int(1024);
delete pci; //正确，释放一个const对象
```



对于一个由内置指针管理的动态对象，直到被显式释放之前它都是存在的



p是指向factory分配的内存的唯一指针

一旦use_factory返回，程序就没有办法释放这块内存了

```c++
void use_factory(T arg)
{
    Foo *p = factory(arg);
}
```



使用new和delete管理动态内存存在三个常见问题：

* 忘记delete内存：忘记释放动态内存会导致“内存泄漏”问题
* 使用已经释放掉的对象：通过在释放内存后将指针置为空，有时可以检测出这种错误
* 同一块内存释放两次：当有两个指针指向相同的动态分配对象时，可能发生这种错误，自由空间可能被破坏



在delete之后，指针就变成了人们所说的==空悬指针==（dangling pointer）

指向一块曾经保存数据对象但现在已经无效的内存的指针

避免方法：在指针即将要离开其作用域之前释放掉它所关联的内存，可以在delete之后将nullptr赋予指针



动态内存的一个基本问题是可能有多个指针指向相同的内存

在delete内存之后重置指针的方法只对这个指针有效，对其他任何仍指向（已释放的）内存的指针是没有作用的

重置p对q没有任何作用

```c++
int *p(new int(42)); //p指向动态内存
auto q = p; //p和q指向相同的内存
delete p; //p和q均变为无效
p = nullptr; //p不再绑定到任何对象
```



3. **shared_ptr和new结合使用**

可以用new返回的指针来初始化智能指针

不能将一个内置指针隐式转换为一个智能指针，必须使用直接初始化形式

```c++
shared_ptr<int> p1 = new int(1024); //错误，必须使用直接初始化形式
shared_ptr<int> p2(new int(1024)); //正确，使用了直接初始化形式
```



p1的初始化隐式地要求编译器用一个new返回的int＊来创建一个shared_ptr

由于我们不能进行内置指针到智能指针间的隐式转换，因此这条初始化语句是错误的



默认情况下，一个用来初始化智能指针的普通指针必须指向动态内存

我们可以将智能指针绑定到一个指向其他类型的资源的指针上，但是为了这样做，必须提供自己的操作来替代delete





shared_ptr可以协调对象的析构，但这仅限于其自身的拷贝（也是shared_ptr）之间

因此推荐使用make_shared而不是new

这样，我们就能在分配对象的同时就将shared_ptr与之绑定，从而避免了无意中将同一块内存绑定到多个独立创建的shared_ptr上









# 第十八章   用于大型程序的工具

# 18.1 异常处理

异常处理（exception handling）机制允许程序中独立开发的部分能够在运行时就出现的问题进行通信并做出相应的处理

异常使得我们能够将问题的检测与解决过程分离开来



1. **抛出异常**

在C++语言中，我们通过抛出（throwing）一条表达式来引发（raised）一个异常

被抛出的表达式的类型以及当前的调用链共同决定了哪段处理代码（handler）将被用来处理该异常



根据抛出对象的类型和内容，程序的异常抛出部分将会告知异常处理部分到底发生了什么错误

当执行一个throw时，跟在throw后面的语句将不再被执行。相反，程序的控制权从throw转移到与之匹配的catch模块



因为跟在throw后面的语句将不再被执行，所以throw语句的用法有点类似于return语句：它通常作为条件语句的一部分或者作为某个函数的最后（或者唯一）一条语句



当抛出一个异常后，程序暂停当前函数的执行过程并立即开始寻找与异常匹配的catch子句



栈展开过程沿着嵌套函数的调用链不断查找，直到找到了与异常匹配的catch子句为止

或者也可能一直没找到匹配的catch，则退出主函数后查找过程终止



假设找到了一个匹配的catch子句，则程序进入该子句并执行其中的代码

当执行完这个catch子句后，找到与try块关联的最后一个catch子句之后的点，并从这里继续执行



如果没找到匹配的catch子句，程序将退出，将调用标准库函数terminate



如果在栈展开过程中退出了某个块，编译器将负责确保在这个块中创建的对象能被正确地销毁

如果某个局部对象的类型是类类型，则该对象的析构函数将被自动调用



析构函数总是会被执行的，但是函数中负责释放资源的代码却可能被跳过，这一特点对于我们如何组织程序结构有重要影响



如果一个块分配了资源，并且在负责释放这些资源的代码前面发生了异常，则释放资源的代码将不会被执行

另一方面，类对象分配的资源将由类的析构函数负责释放

如果我们使用类来控制资源的分配，就能确保无论函数正常结束还是遭遇异常，资源都能被正确地释放



出于栈展开可能使用析构函数的考虑，析构函数不应该抛出不能被它自身处理的异常



























































