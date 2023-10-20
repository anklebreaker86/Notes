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

* STATIC：创建静态库，即编译文件的打包存档，以便在链接其他目标时使用
* SHARED：创建动态库，可以动态链接，在运行时加载的库
* OBJECT：将源码编译到目标文件，不归档到静态库中，也不能将它们链接到共享对象中。若需要一次性创建静态库和动态库，那么使用对象库尤其有用
* MODULE：DSO组，与SHARED不同，不链接到项目中任何目标，不过可以动态加载



CMake还能生成特殊类型的库，不会在构建系统中产生输出：

* IMPORTED：此类库目标表示位于项目外部的库。主要用途是对现有依赖项进行构建，该库被视为不可变的
* INTERFACE：与IMPORTED库类似，不过该类型库可变，没有位置信息。主要用于项目之外的目标构建使用
* ALIAS：这种库为项目中已存在的库定义别名，但不能为IMPORTED库选择别名



本例中使用add_library集合源代码，后续章节将使用target_sources汇集源代码



**更多信息**

OBJECT库使用：

```cmake
project(recipe-03 LANGUAGES CXX)

add_library(message-objs OBJECT Message.h Message.cpp)

set_target_properties(message-objs 
	PROPERTIES 
		POSITION_INDEPENDENT_CODE 1)
		
add_library(message-shared SHARED $<TARGET_OBJECTS:message-objs>)

add_library(message-static STATIC $<TARGET_OBJECTS:message-objs>)

add_executable(hello-world hello-world.cpp)

target_link_libraries(hello-world message-static)
```



需要保证编译的目标文件与生成位置无关。可以通过使用`set_target_properties`命令，设置`message-objs`目标的相应属性来实现

现在，可以使用这个对象库来获取静态库(`message-static`)和动态库(`message-shared`)



## 1.4 用条件句控制编译

条件结构if-else-endif



**具体实施**

1. 将Message.h和Message.cpp构建为一个库，然后链接到hello-world可执行文件中
2. 将Message.h、Message.cpp和hello-world.cpp构建成一个可执行文件，不生成库



* 首先定义CMake版本、项目名称和支持的语言：

```cmake
cmake_minimum_required(VERSION 3.5 FATAL_ERROR)
project(recipe-04 LANGUAGES CXX)
```

* 引入新变量USE_LIBRARY，逻辑变量，值为OFF。还打印其值

```cmake
set(USE_LIBRARY OFF)
message(STATUS "Compile sources into a library? ${USE_LIBRARY}")
```

* 然后引入一个变量_source，包括Message.h和Message.cpp

```cmake
list(APPEND _source Message.h Message.cpp)
```

* 引入if-else语句，若为真，则打包为一个库

```cmake
if(USE_LIBRARY)
	add_library(message ${_source})
	add_executable(hello-world hello-world.cpp)
	target-link_libraries(hello-world message)
else()
	add_executable(hello-world hello-world.cpp ${_source})
endif()
```



**工作原理**

* 若将逻辑变量设为1、ON、YES、true、Y或非零整数，则为true
* 若将逻辑变量设为0、OFF、NO、false、N、IGNORE、NOTFOUND、空字符串，则为false



## 1.5 向用户显示选项

使用option()命令，使该值可以从外部进行修改



**具体实施**

* 用一个选项替换上一个示例中的set(USE_LIBRARY OFF)命令

```cmake
option(USE_LIBRARY "Compile sources into a library" OFF)
```

* 可以通过cmake的 -D选项，将信息传递给CMake

```cmake
cmake -D USE_LIBRARY=ON ..
```

-D开关用于为CMake设置任何类型的变量：逻辑变量、路径等



**工作原理**

option可接受三个参数：

```cmake
option(<option_variable> "help string" [initial value])
```

* <option_variable>表示该选项的变量的名称
* “help string”记录选项的字符串
* [initial value]选项默认值，可以是ON或OFF



**更多信息**

有时选项间会有依赖情况

CMake提供==cmake_dependent_option()==命令用来定义依赖于其他选项的选项

```cmake
include(CMakeDependentOption)

cmake_dependent_option(
	MAKE_STATIC_LIBRARY "Compile sources into a static library" OFF
	"USE_LIBRARY" ON)
	
cmake_depent_option(
	MAKE_SHARED_LIBRARY "Compile sources into a shared library" ON
	"USE_LIBRARY" ON)
```

如果USE_LIBRARY为ON，则MAKE_STATIC_LIBRARY默认值为OFF



CMake有适当的机制，通过包含模块来扩展其语法和功能

本例中包含了一个名为CMakeDependentOption的模块，若没有include这个模块，cmake_depent_option()命令将不可用



## 1.6 指定编译器

**具体实施**

CMake将语言的编译器存储在CMAKE_ <LANG> _COMPILER中，<LANG>是任何一种语言

* 使用 -D选项

```cmake
cmake -D CMAKE_CXX_COMPILER=clang++ ..
```

* 通过导出环境变量

```cmake
env CXX=clang++ cmake ..
```



在这里假设，其他编译器在标准路径中可用，若不可用，需要将完整编译器可执行文件路径传递给CMake



**工作原理**

配置时，CMake会进行平台测试，确定哪些编译器可用，是否适合当前项目



**更多信息**

查看可用的编译器

```cmake
cmake --system-information information.txt
```



CMake提供额外变量与编译器交互：

* CMAKE _ _<LANG>_ _COMPILER_LOADED：若项目启用语言<LANG>，则设为true
* CMAKE __<LANG>_ _COMPILER_ID：编译器标识字符串，供应商特有

（）



## 1.7 切换构建类型

CMake可以配置构建类型，如Debug、Release等，可以设置相关选项或属性

控制生成构建系统使用的配置变量是==CMAKE_BUILD_TYPE==，默认为空



CMake识别值为：

* Debug：没有优化条件下，使用带有调试符号构建库或可执行文件
* Release：用于构建的优化的库或可执行文件，不包含调试符号
* RelWithDebInfo：构建较少的优化库或可执行文件，包含调试符号
* MinSizeRel：用于不增加目标代码大小的优化方式，来构建库或可执行文件



**具体实施**

* 定义CMake版本、项目名称等

```cmake
cmake_minimum_required(VERSION 3.5 FATAL_ERROR)
project(recipe-07 LANGUAGES C CXX)
```

* 设置一个默认的构建类型Release，并打印一条信息，该变量设为缓存变量，可以通过缓存进行编辑

```cmake
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type" FORCE)
endif()
message(STATUS "Build type : ${CMAKE_BUILD_TYPE}")
```

* 验证配置输出

```cmake
mkdir -p build
cd build
cmake ..
```

* 切换构建类型

```cmake
cmake -D CMAKE_BUILD_TYPE=Debug ..
```



**更多信息**

CMake也支持复合配置生成器，最显著是visual studio和Xcode，它们可以同时处理多个配置

可用==CMAKE_CONFIGURATIONTYPES==变量对这些生成器的可用配置类型进行调整

对Visual Studio的CMake调用：

```cmake
mkdir -p build
cd build
cmake .. -G"Visual Studio 12 2017 Win64" 
	-D CMAKE_CONFIGURATION_TYPES="Release;Debug"
```



将为Release和Debug配置生成一个构建树，然后可以使用--config标志决定构建哪个

```cmake
cmake --build . --config Release
```



## 1.8 设置编译器选项

CMake为调整或扩展编译器标志提供了很大的灵活性：

* CMake将编译选项视为目标属性，因此可以根据每个目标设置编译选型
* 使用 -D标志直接修改==CMAKE_ <LANG> _FLAGS _<CONFIG>变量



有一个计算不同几何形状面积代码，computer_area.cpp

函数实现分布在不同文件中，每个几何形状有一个头文件和源文件，总共4个头文件和5个源文件

```cmake
├─ CMakeLists.txt
├─ compute-areas.cpp
├─ geometry_circle.cpp
├─ geometry_circle.hpp
├─ geometry_polygon.cpp
├─ geometry_polygon.hpp
├─ geometry_rhombus.cpp
├─ geometry_rhombus.hpp
├─ geometry_square.cpp
└─ geometry_square.hpp
```



**具体实施**

* 设置CMake最低版本

```cmake
cmake_minimum_required(VERSION 3.5 FATAL_ERROR)
```

* 声明项目名称和语言

```cmake
project(recipe-08 LANGUAGES CXX)
```

* 打印当前编译器标志

```cmake
message("C++ compiler flags: ${CMAKE_CXX_FLAGS}")
```

* 为目标准备标志列表，其中一些将无法在Windows上使用

```cmake
list(APPEND flags "-fPIC" "-Wall")
if(NOT WIN32)
	list(APPEND flags "-Wextra" "-Wpedantic")
endif()
```

* 添加geomerty库

```cmake
add_library(geometry
  STATIC
    geometry_circle.cpp
    geometry_circle.hpp
    geometry_polygon.cpp
    geometry_polygon.hpp
    geometry_rhombus.cpp
    geometry_rhombus.hpp
    geometry_square.cpp
    geometry_square.hpp
  )
```

* 为该库目标设置编译选项

```cmake
target_compile_options(geometry PRIVATE ${flags})
```

* 将生成compute-areas可执行文件作为一个目标

```cmake
add_executable(compute-areas compute-areas.cpp)
```

* 为可执行目标设置编译选项

```cmake
target_compile_options(compute-areas PRIVATE "-fPIC")
```

* 最后将可执行文件链接到geometry库

```cmake
target_link_libraries(compute-areas geometry)
```



**如何工作**

编译选项可以添加三个级别的可见性：

* PRIVATE：编译选项只应用于指定目标，不会传递给与目标相关的目标
* INTERFACE：给定编译选项将只应用于指定目标，并传递给与目标相关的目标
* PUBLIC：应用于指定目标和使用它的目标



这种方式添加编译选项，不会影响CMake全局变量CMAKE_ <lang> _FLAGS _<CONFIG>



一种方法是，使用CMake将额外的参数传递给本地构建工具

```cmake
mkdir -p build
cd build
cmake ..
cmake --build . -- VERBOSE=1
```

第二种方法，使用CMake参数进行配置，该命令禁用异常和运行时类型标识

```cmake
cmake -D CMAKE_CXX_FLAGS="-fno-exception -fno-rtti" ..
```

（）



## 1.9 为语言设定标准

3.1版本中，CMake引入了一个独立于平台和编译器的机制，用于为C++和C设置语言标准

为目标设置<LANG> _STANDARD属性



**准备工作**

示例代码定义了动物的多态，使用std::unique_ptr作为结构中的基类

```c++
std::unique_ptr<Animal> cat = CAT("Simo");
std::unique_ptr<Anima> dog = Dog("Marlow");
```

使用工厂方式实现构造函数

```c++
typedef std::function<std::unique_ptr<Animal>(const 
std::string &)> CreateAnimal;
```



**具体实施**

* 声明CMake版本等

```cmake
cmake_minimum_required(VERSION 3.5 FATAL_ERROR)
project(recipe-09 LANGUAGES CXX)
```

* 要求在Windows上导出所有库符号

```cmake
set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)
```

* 编译源代码为动态库

```cmake
add_library(animals
	SHARED
	Animal.cpp
	...
)
```

* 为目标设置属性，以避免在使用一些编译器构建DSO时出现问题

```cmake
set_target_properties(animals
	PROPERTIES
	CXX_STANDARD 14
	CXX_EXTENSIIONS OFF
	CXX_STANDARD_REQUIRED ON
	POSITION_INDEPENDENT_CODE 1)
```

* 然后，为可执行文件添加一个新目标，并设置属性

```cmake
add_executable(animal-farm animal-farm.cpp)
set_target_properties(animal-farm
	PROPERTIES
	CXX_STANDARD 14
	CXX_EXTENSION OFF
	CXX_STANDARD_REQUIRED ON)
```

* 将可执行文件链接到库

```cmake
target_link_libraries(animal-farm animals)
```



**工作原理**

* CXX_STANDARD 会设置我们想要的标准
* CXX_EXTENSIONS 告诉CMAKE，只启用ISO C++标准的编译器标志，而不使用扩展
* CXX_STANDARD_REQUIRED 指定所选标准的版本，当设为OFF时，将寻找下一标准的最新版本。首先查找C++14，然后C++11，然后C++98



## 1.10使用控制流

CMake还提供创建循环的语言工具：==foreach endforeach==和==while endwhile==

两者都可以与break结合使用



**准备工作**

重用几何示例



**具体实施**

* 指明版本等

```cmake
cmake_minimum_required(VERSION 3.5 FATAL_ERROR)
project(recipe-10 LANGUAGES CXX)
add_library(geometry
	STATIC
	geometry_circle.cpp
	...)
```

* 使用-03编译器优化级别编译库，对目标设置一个私有编译器选项

```cmake
target_compile_options(geometry
	PRIVATE
		-03)
```

* 生成一个源文件列表，以较低的优化选项进行编译

```cmake
list(APPEND sources_with_lower_optimization
	geometry_circle.cpp
	geometry_rhombus.cpp)
```

* 循环这些源文件，将它们的优化级别调到-02，

```cmake
message(STATUS "Setting source properties using IN LISTS syntax:")
foreach(_source IN LISTS sources_with_lower_optimization)
	set_source_files_properties(${_source} PROPERTIES COMPILE_FLAGS -02)
	message(STATUS "Appending -02 flag for ${_source}")
endforeach()
```

* 最后，添加compute-areas可执行目标，并将geometry库连接上去

```cmake
add_executable(compute-areas compute-areas.cpp)
target_link_libraries(compute-)
```





































