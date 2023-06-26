# 第一章   介绍

CMAKE是一套工具，涵盖了从创建构建到生产，以及发布的 一切操作。



# 第二章   创建项目

CMake为构建项目制定了规则，首先需要有一个名为 CMakeLists.txt的文件，该文件定义了构建什么、如何构建、运行哪些测试以及创建哪些包



源目录和二进制目录概念是CMake的基础

* 源目录：CMakeLists.txt文件所在的文件夹，项目的源文件和构建 所需的其他文件都组织在该位置下
* 二进制目录：生成构建内容的地方，通常称为构建目录，可执行文件、库、测试输出和包都在构建目录中创建

CMake还 在构建目录中创建一个CMakeCache.txt的特殊文件，存储各种信息，以便在后续运行时重用



开发人员处理项目时，需要确定构建目录相对于源目录的位置

有两种方式：源内构建和源外构建



## 2.1 源内构建

源目录和构建目录可以相同，但不推荐这样做，这种方式称为源内构建



## 2.2 源外构建

更可取的方式是让源目录和构建目录不同，这称为源外构建

![image-20230614223908652](C:\Users\ai869\AppData\Roaming\Typora\typora-user-images\image-20230614223908652.png)

## 2.3 生成项目文件

选择生成器来选择要创建的项目文件

CMake支持Visual Studio、Xcode、Ninja等生成器



运行CMake最基本方法是通过CMake命令行

最简单方法是将目录切换到build目录，将生成器类型和源树位置的选项传给CMake

```cmake
mkdir build
cd build
cmake -G "Unix Makefiles" ../source
```



## 2.4 运行构建工具

```c#
cmake --build /some/path/build --config Debug --target MyApp
```

* --build：指向CMake项目生成的构建目录
* --config：指定要构建哪个配置，单配置生成器忽略
* --target：告诉构建工具要构建什么目标



# 第三章   一个简单的项目

所有的CMake项目都从源目录下的CMakeLists.txt文件开始，可以把这个文件看作CMake项目文件

其定义 构建过程中所有的内容，从源码和构建目标，到测试、打包和自定义任务



CMake有自己的语言，有许多程序员熟悉的东西，比如变量、函数、宏、条件、循环、注释等等



下面是一个简单的 CMakeLists.txt文件，会生成一个可执行文件：

```cmake
cmake_minimum_required(VERSION 3.2)
project(MyApp)
add_executable(myExe main.cpp)
```



上面示例中的每一行都会执行一个CMake命令，CMake命令类似于函数调用，但不直接返回值

参数之间可以用空格分隔，也可以跨多行分隔

命令名称也是不区分大小写的



## 3.1 管理CMake版本

项目指定CMake版本行为的详细信息是使用 cmake_minimum_required() 命令

这应该是CMakeLists.txt文件的第 一行，这样项目的需求就会放在在其他事情之前

该命令做做两件事：

* 指定了项目所需的CMake的最低版本
* 强制设置将CMake行为匹配到对应版本



```cmake
cmake_minimum_required(VERSION major.minor[.patch[.tweak]])
```

* VERSION关键字必须出现，提供的版本详细信息必须有major.minor
*  patch 和 tweak 没有必要



## 3.2 project()

每个CMake项目都应该包含一个 project() 命令，它应该在 cmake_minimum_required() 之后出现

```cmake
project(projectName
	[VERSION major[.minor[.patch[.tweak]]]]
	[LANGUAGES languageName...]
	)
```

* 项目名称是必需的，该名字用于项目的生成器，也可运用于其他部分，如项目名称
* LANGUAGES定义了应该为项目启用的编程语言



project() 命令的重要职责是检查每种语言的编译器，并确保它们能够正确地编译和链接，这样编译器和链接器设置的问题会在很早就发现



当CMake执行的编译器和链接器检查成功时，它们的结果会进行缓存，这样就不必在后续的CMake运行中重复



## 3.3 构建可执行文件

add_executable() 命令告诉CMake为一组源文件创建一个可执行文件

```cmake
add_executable(targetName source1 [source2...])
```

这将创建一个可执行文件，可以在CMake项目中作为 targetName 引用



```cmake
add_executable(myApp main.cpp)
```

默认情况下，可执行文件的名称在Windows上是myApp.exe

通过使用不同的名称多次调用 add_executable() ，可以在 CMakeLists.txt文件中定义多个可执行文件



## 3.4 注释

任何 以 # 字符开头的行都视为注释，除了在带引号的字符串中



# 第四章   构建目标

除了可执行文件之外，开发人员还需要构建和链接库

CMake支持几种不同类型的库，包括静态、动态、模 块和框架



## 4.1 可执行文件

add_executable() 命令的完整形式如下

```cmake
add_executable(targetName [WIN32] [MACOSX_BUNDLE]
				[EXCLUDE_FROM_ALL]
				source [source2...]
)
```

* WIN32：在Windows平台上构建可执行文件时，此选项表示CMake将该可执行文件构建为Windows GUI应用程序

* MACOSX_BUNDLE：它会引导CMake在苹果平台上构建应用程序包

* EXCLUDE_FROM_ALL：项目定义了许多目标，但默认情况下只需要构建一部分

  当在构建没有指定目标时，将构建默认的 ALL 目标

  如果可执行文件 用EXCLUDE_FROM_ALL 定义，就不会包含在默认的 ALL 目标中



## 4.2 定义库

库目标使用add_library()命令构建，命令中有许多参数

```cmake
add_library(targetName [STATIC | SHARED | MODULE]
			[EXCLUDE_FROM_ALL]
			source1 [source2...]
)
```



使用 targetName 来引用库，默认构建库的名称源于 targetName

EXCLUDE_FROM_ALL 关键字具有与 add_executable() 相同的效果，避免库包含在默认 ALL 目标中



构建库的类型由其余的三个关键字指定： STATIC 、 SHARED 或 MODULE

* STATIC：指定为静态库，在Windows构建为targetName.lib，在Unix平台上为libtargetName.a
* SHARED ：指定为动态库，在Windows上库名称是targetName.dll，在Apple平台上是libtargetName.dylib，在类Unix平台上是libtargetName.so
* MODULE：这种方式类似于动态库，不同的是在运行时动态加载



## 4.3 目标链接

库之间存在几种不同类型的依赖关系：

* PRIVATE：指定A库自己使用B库，其他链接到A库的目标不需要知道B库
* PUBLIC：指定A库不仅在内部使用B库，还在其他接口中使用B库，因此任何使用A库的目标也会依赖B库
* INTERFACE：为了使用A库，也必须使用B库的一部分



```cmake
target_link_libraries(targetName
		<PRIVATE | PUBLIC | INTERFACE> item1 [item2...]
		[<PRIVATE | PUBLIC | INTERFACE> item3 [item4...]]
)
```



```cmake
add_library(collector src1.cpp)
add_library(algo src2.cpp)
add_library(engine src3.cpp)
add_library(ui src4.cpp)
add_executable(myApp main.cpp)
target_link_libraries(collector
	PUBLIC ui
	PRIVATE algo engine
)
target_link_libraries(myApp PRIVATE collector)
```

该例中，ui库链接到collector，因此即使myApp只直接连接到collector，由于这种关系，myApp也将链接到ui

algo和engine作为PRIVATE链接到collector，所以myApp不会直接链接



target_link_libraries所使用的targetName，必须在add_executable()或add_library() 命令之后调用，所连接的库目标需要在同一目录中进行定义



## 4.4 非目标链接

上一节中，所有的链接项都是当前CMake的目标，但 target_link_libraries() 命令要灵活很多



除了CMake目标之外， target_link_libraries() 也可以通过以下内容指定

* 库文件的完整路径：CMake将把库文件添加到链接器命令中
* 库名：如果只给出库的名称而没有路径，链接器将搜索该库
* 连接标识：有种特殊情况，需要以-l或-framework以外的连字符开头的形式添加到链接器命令中的标志，CMake会警告， 因为这些应该只用于 PRIVATE 构建



## 4.5 老式CMake

target_link_libraries() 还有其他一些形式，这些选项有助于理解理解老式CMake项目，但不推荐在新项目中使用



```cmake
target_link_libraries(targetName item [item..])
```

应该在命令中使 用 PRIVATE 、 PUBLIC 和 INTERFACE ，从而更准确地表达依赖关系

上述形式通常等价于使用了 PUBLIC，但在某些情况下，可能视为 PRIVATE

如果项目混合使用新旧命令形式 定义库的依赖关系，那么老式的方式通常会视为 PRIVATE 



另一种不推荐形式：

```cmake
target_link_libraries(targetName
 LINK_INTERFACE_LIBRARIES item [item...]
)
```

可以使用新形式：

```cmake
target_link_libraries(targetName
 <LINK_PRIVATE|LINK_PUBLIC> lib [lib...]
 [<LINK_PRIVATE|LINK_PUBLIC> lib [lib...]]
)
```



# 第五章   变量

## 5.1 基本变量

定义变量的基本方法是使用set()

```cmake
set(varName value...[PARNET_SCOPE])
```

CMake变量有的作用域

CMake将所有变量都作为字符串处理

当设置一个变量时，CMake不要求这些值用引号括起来，除非值中包含空格



如果给定多个值，这些值将用 分号连接在一起，生成的字符串就是CMake列表

```cmake
set(myVar a b c) # myVar = "a;b;c"
set(myVar a;b;c) # myVar = "a;b;c"
set(myVar "a b c") # myVar = "a b c"
set(myVar a b;c) # myVar = "a;b;c"
set(myVar a "b c") # myVar = "a;b c"
```



变量的值通过 ${myVar} 获得

使用未定义的变量只会替换一 个空字符串，而不会出现错误或警告

```cmake
set(foo ab) # foo = "ab"
set(bar ${foo}cd) # bar = "abcd"
set(baz ${foo} cd) # baz = "ab;cd"
set(myVar ba) # myVar = "ba"
set(big "${${myVar}r}ef") # big = "${bar}ef" = "abcdef"
set(${foo} xyz) # ab = "xyz"
set(bar ${notSetVar}) # bar = ""
```

字符串不限于单行，可以包含换行字符，还可以包含引号，不过需要使用反斜杠进行转义



可以调用 unset() 或 set() 来解除变量的设置



## 5.2 环境变量

环境变量通过 $ENV{varName} 获得，可以在 任何使用 ${varName} 的地方使用

```cmake
set(ENV{PATH} "$ENV{PATH}:/opt/myDir")
```

设置这样的环境变量只会影响当前的CMake实例

一旦CMake运行结束，对环境变量的更改就会丢失



## 5.3 缓存变量

缓存变量的生命周期仅限于 CMakeLists.txt文件的处理，缓存变量存储在名为CMakeCache.txt的特殊文件中，并且在CMake运行期间持久存在



缓存变量的值与普通变 量的检索方法完全相同(即 ${myVar} 形式)，但 set() 命令在设置缓存变量时有所不同:

```cmake
set(varName value... CACHE type "docstring" [FORCE])
```



# 第7章   子目录

## 7.1 add_subdirectory()

add_subdirectory() 允许项目将另一个目录引入到构建中

该目录必须有CMakeLists.txt文件

调 用 add_subdirectory() 处理时，在项目的构建中会为其创建相应的目录

```cmake
add_subdirectory(sourceDir [binaryDir] [EXCLUDE_FROM_ALL])
```



* 将 sourceDir 指定为绝对路径或相对路 径
* 通常，不需要指定 binaryDir ，当省略这个目录时，CMake会创建一个具有相同名称的目录 sourceDir
* EXCLUDE_FROM_ALL 关键字，用于控制子目录中的目标是否包含在项目的 ALL 目标中



1. **源目录和构建目录**

* CMAKE_SOURCE_DIR：源的最顶层目录(即最顶层CMakeLists.txt文件所在的位置)
* CMAKE_BINARY_DIR：构建的最顶层目录
* CMAKE_CURRENT_SOURCE_DIR：当前处理的CMakeLists.txt文件的目录
* CMAKE_CURRENT_BINARY_DIR：当前处理的CMakeLists.txt文件对应的构建目录



2. **作用域**

add_subdirectory() 的效果是CMake为处理该目录的CMakeLists.txt文件 创建一个新的作用域

* 调用作用域中定义的所有变量对子作用域是可见的
* 子作用域中创建的任何新变量对调用作用域都不可见
* 对子作用域内变量的任何更改都是该子作用域的局部更改



## 7.2 include()

include有以下两种形式：

```cmake
include(fileName [OPTIONAL] [RESULT_VARIABLE myVar] [NO_POLICY_SCOPE])
include(module [OPTIONAL] [RESULT_VARIABLE myVar] [NO_POLICY_SCOPE])
```



include() 读入的是文件名，而 add_subdirectory() 读入一个目录，并在目录中查找CMakeLists.txt文件

include() 没有引入新的变量范围，而 add_subdirectory() 引入了新的变量范围

当 include() 处理文件时， CMAKE_CURRENT_SOURCE_DIR 和 CMAKE_CURRENT_BINARY_DIR 的值不会改变， 而 add_subdirectory() 会改变这两个变量



## 7.3 提前终止处理

return() 命令可以完成该功能，但不能返回值，它唯一的作用是结束 当前范围的处理







# 第13章   构建类型









