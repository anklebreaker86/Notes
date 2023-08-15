# 第一章 从可执行文件到库

## 1.1 将单个源文件编译为可执行文件

**准备工作**

将下列源码编译为单个可执行文件

```c++
#include <cstdlib>
#include <iostream>
#include <string>

std::string say_hello()  {return std::string("Hello!");}

int main()
{
    std::cout << say_hello() << std::endl;
    return EXIT_SUCCESS;
}
```



**具体实施**

将CMake指令放入名为==CMakeLists.txt==的文件夹中



1. 创建CMakeLists.txt文件
2. 第一行设置CMake最低版本

```cmake
cmake_minimum_required(VERSION 3.5 FATAL_ERROR)
```

3. 第二行声明项目名称和支持的编程语言

```cmake
project(recipe-01 LANGUAGES CXX)
```

4. 使用CMake创建可执行文件：hello-world，通过编译和链接源文件hello-world.cpp生成

```cmake
add_executable(hello-world hello-world.cpp)
```

5. 将CMakeLists.txt和hello-world.cpp放在相同目录中
6. 通过创建build目录，在build目录下来配置项目

```cmake
mkdir -p build
cd build
cmake ..
```

7. 如果一切顺利，项目配置已在build中生成



**工作原理**

CMAKE中，C++是默认语言，可以使用LANGUAGES显式声明项目语言

cmake --help输出帮助信息



CMake是一个构建系统生成器

将描述构建系统（如Unix Makefile、Visual Studio等）应当如何操作才能编译代码

CMake为所选的构建系统生成相应的指令



GNU/Linux上，CMake默认生成Unix Makefile来构建项目：

* Makefile：make将运行指令来构建项目
* CMakefile：包含临时文件的目录，CMake用于检测操作系统、编译器等
* cmake_install.cmake：处理安装规则的CMake脚本，在项目安装时使用
* CMakeCache.txt：CMake缓存，CMake在重新运行配置时使用这个文件



## 1.2 切换生成器

**准备工作**

CMake针对不同平台支持本地构建工具列表

使用cmake --help可在平台上找到生成器名单，以及已安装的CMake版本



**具体实施**

使用==-G==切换生成器

1. 使用以下步骤配置项目

```cmake
mkdir -p build
cd build
cmake -G Ninja ..
```



2. 构建项目

```cmake
cmake --build .
```



## 1.3 构建和链接静态库和动态库

项目中有多个源文件，通常分布在不同子目录中

下面例子将展示如何将源代码编译到库中，以及如何链接这些库



**准备工作**

修改hello-world.cpp，调用Message.h中的类，函数实现在Message.cpp中



**具体实施**

这里有两个文件需要编译，所以需要修改CMakeLists.txt

本例中，先把它们编译成一个库



1. 创建静态库，库名称与源码文件名相同

```cmake
add_library(message
  STATIC
    Message.h
    Message.cpp
  )
```

2. 创建hello-world可执行文件的目标部分不需要修改

```cmake
add_executable(hello-world hello-word.cpp)
```

3. 最后，将目标库链接到可执行目标

```cmake
target_link_libraries(hello-world message)
```

4. 对项目进行配置和构建

```cmake
mkdir build
cd build
cmake ..
cmake --build .
```



**工作原理**

引入两个新命令

* add_library：生成必要的构建指令，将指定的源码编译到库中

  * 第一个参数为目标名，可使用相同名称来引用库
  * 生成库根据第二个参数（STATIC或SHARED）和操作系统确定

  

* target_link_libraries：将库链接到可执行文件，在链接前要完成库的构建

编译成功后，构建目录包含message.lib静态库和hello-world可执行文件



CMake接受其他值作为add_library的第二个参数的有效值，本书会用到的值：



























