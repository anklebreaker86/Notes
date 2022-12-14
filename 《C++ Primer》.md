# 第一章   开始

## 1.1 编写一个简单的C++程序

每个C++都包含一个或多个函数，其中一个必须命名为main，操作系统调用main运行C++程序。

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
    std::cout << "Enter two numbers:" << endl;
    int v1 = 0, v2  = 0;
    std::cin >> v1 >> v2;
    std :: cout << "The sum of" << v1 << "and" << v2
        		<< "is" << v1 + v2 << std::endl;
    return 0;
}
```



尖括号中的名字指出了一个头文件。

使用输出运算符<<打印信息。

双引号包围的字符为==字符串面值常量==。

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



一般初始化变量时，初始值会被拷贝到新建的对象中；定义引用时，程序把引用和初始值绑定在一起，而不是拷贝。

初始化后，引用将于初始对象一直绑定，无法重新绑定另一对象。

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



指针存放某个对象 的地址，获取该地址使用==取地址符&==

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



==void*==是一种特殊的指针类型，可用于存放任意对象的地址。

不能直接操作void*对象，因为不知道对象到底是什么类型。



3. **理解复合类型的声明**

在定义语句中，类型修饰符*或&仅仅是修饰变量名，对声明语句中的其他变量不产生作用。

多个修饰符连写在一起时，按照逻辑关系解释即可，**表示指向指针的指针。

```c++
int val = 42;
int *p; //p是一个int型指针
int *&r = p; //r是一个对指针p的引用
r = &val; //r引用了一个指针，因此对r赋值&val，即令p指向val
*r = 0; //解引用r得到p指向的对象，将val改为0
```



从右向左阅读变量的定义。离变量名最近的符号决定变量的类型，因此r为引用，其余部分决定r引用的类型是什么，例子中的*说明r引用的是一个指针。



## 2.4 const限定符

const对象一旦创建后就不能再改变，所以const对象必须初始化。

默认情况下，const对象被设定为仅在文件内有效。

多个文件出现同名const变量时，等同于在不同文件中分别定义了独立变量。



希望只在一个文件中定义const，在多个文件中声明并使用它。方法是不管声明还是定义都添加==extern==关键字，这样只需定义一次就可以。



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



常量引用对于引用的对象本身是不是一个常量未做限定，所以允许通过其他途径改变它的值。

```c++
int i = 42;
int &r1 = i; //引用r1绑定对象i
const int &r2 = i; //r2也绑定对象i，但是不允许通过r2修改i的值
r1 = 0; //r1并非常量，i的值修改为0
r2 = 0; //错误，r2是一个常量引用
```



2. **指针和const**

指向常量的指针不能用于改变其所指对象的值。

要想存放常量对象的地址，只能使用指向常量的指针：

```c++
const double pi = 3.14; //pi是个常量，它的值不能改变
double *ptr  = &pi; //错误，ptr是一个普通指针
const double *cptr = &pi; //正确
*cptr = 42; //错误，不能给*cptr赋值
```



指向常量的指针也没有规定所指对象必须是一个常量。仅仅要求不能通过指针改变对象的值。



常量指针必须初始化，一旦初始化完成则值不能再改变。==把*放在const之前说明指针是常量==。

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

常量表达式指值不会改变且再编译过程中就能得到计算结果的表达式。

字面值属于常量表达式，用常量表达式初始化的const对象也是常量表达式。



是不是常量表达式由数据类型和初始值共同决定

```c++
const int max_files = 20; //max_files是常量表达式
const int limit = max_files + 1; //limit是常量表达式
int staff_size = 27; //staff_size不是常量表达式
const int sz = get_size(); //不是常量表达式
```

staff_size数据类型只是普通的int，sz具体值直到运行时才获得。



C++11允许将变量声明为constexpr类型以便编译器来验证变量的值是否是一个常量表达式。

```c++
constexpr int mf = 20; //20是常量表达式
constexpr int limit = mf + 1; //mf + 1是常量表达式
constexpr int sz = size(); //只有当size是一个constexpr函数时才是一条正确声明
```



对声明constexpr时用到的类型必须有所限制，称为==字面值类型==

算术类型、引用和指针都属于字面值类型

constexpr指针初始值必须时nullptr或0，或是存储于某个固定地址中的对象



函数体内定义变量一般来说并非存放再固定地址中，因此constexpr指针不能指向这样的变量。

constexpr仅对指针有效，与指针所指的对象无关：

```c++
constexpr int *q = nullptr; //q是一个指向整数的常量指针
```



# 2.5 处理类型

1. **类型别名**

类型别名是一个名字，是某种类型的同义词。

有两种方法可以定义类型别名：

* 使用关键字==typedef==
* 使用==别名声明==

```c++
typedef double wages; //wages是double的同义词
typedef wages base, *p; //base是double的同义词，p是double*的同义词
using SI = Sales_item; //SI是Sales_item的同义词
```



若某类型别名指代复合类型或常量，把它用到声明语句中会产生意想不到的后果。







# 第六章   函数

## 6.1 函数基础

函数定义包括：

* 返回类型
* 函数名字
* 0或多个形参组成的列表
* 函数体

使用调用函数符执行函数，即一对圆括号。

作用于一个表达式，表达式是函数或指向函数的指针；圆括号内是实参列表。



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



函数调用：使用实参初始化函数对应形参，将控制权转移给被调函数。

遇到return语句函数结束执行过程，返回return语句的值，将控制权转到主调函数。

函数返回类型不能是数组类型或函数类型，可以是指向数组或函数的指针。



1. **局部对象**

名字有作用域，对象有生命周期，生命周期是程序执行过程中对象存在的一段时间。



形参和函数体内部定义的变量统称为局部变量，仅在函数的作用域内可见。

把只存在于块执行期间的对象称为自动对象。

形参是一种自动对象，函数开始时为形参申请存储空间，函数终止销毁形参。



局部静态对象直到程序终止才销毁，即使对象所在的函数结束执行也不会有影响。==static==



2. **函数声明**

函数名字必须在使用前声明，函数声明也称为函数原型。

函数只能定义一次，但可以声明多次。函数的声明无需函数体，用一个分号代替即可。

```c++
void print(int a);
```

变量和函数建议在头文件中定义。



3. **分离式编译**

分离式编译允许把程序分割到几个文件中去，每个文件独立编译。

若需要修改其中一个源文件，只需重新编译那个改动了的文件。

Windows的后缀是.obj，UNIX的后缀是.o

接下来编译器把对象文件链接在一起形成可执行文件。



## 6.2 参数传递

如果形参是引用类型，它将绑定到对应的实参上；否则将实参的值拷贝后赋给形参。

形参为引用类型时，对应的实参被==引用传递==

实参值被拷贝给形参时，形参和实参是连个相互独立的对象。这样的实参为==值传递==



1. **传值参数**

当初始化一个非引用类型的变量时，初始值被拷贝给变量。

此时，对变量的改动不会影响初始值，传值参数机理一样。



指针的行为和其他非引用类型一样。



2. **传引用参数**

对于引用的操作实际上是作用在引用所引的对象上。









## 第七章   类



类的基本思想是==数据抽象==和==封装==。

数据抽象是一种依赖于接口和实现分离的编程技术。

类的接口包括用户所能执行的操作；类的实现则包括类的数据成员、负责接口实现的函数体以及定义类所需的各种私有函数。

封装实现了类的接口和实现的分离。



## 7.1 定义抽象数据类型

抽象数据类型不允许访问它的数据成员，只能通过接口进行操作。



