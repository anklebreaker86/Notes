# ��һ�� �ӿ�ִ���ļ�����

## 1.1 ������Դ�ļ�����Ϊ��ִ���ļ�

**׼������**

������Դ�����Ϊ������ִ���ļ�

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



**����ʵʩ**

��CMakeָ�������Ϊ==CMakeLists.txt==���ļ�����



1. ����CMakeLists.txt�ļ�
2. ��һ������CMake��Ͱ汾

```cmake
cmake_minimum_required(VERSION 3.5 FATAL_ERROR)
```

3. �ڶ���������Ŀ���ƺ�֧�ֵı������

```cmake
project(recipe-01 LANGUAGES CXX)
```

4. ʹ��CMake������ִ���ļ���hello-world��ͨ�����������Դ�ļ�hello-world.cpp����

```cmake
add_executable(hello-world hello-world.cpp)
```

5. ��CMakeLists.txt��hello-world.cpp������ͬĿ¼��
6. ͨ������buildĿ¼����buildĿ¼����������Ŀ

```cmake
mkdir -p build
cd build
cmake ..
```

7. ���һ��˳������Ŀ��������build������



**����ԭ��**

CMAKE�У�C++��Ĭ�����ԣ�����ʹ��LANGUAGES��ʽ������Ŀ����

cmake --help���������Ϣ



CMake��һ������ϵͳ������

����������ϵͳ����Unix Makefile��Visual Studio�ȣ�Ӧ����β������ܱ������

CMakeΪ��ѡ�Ĺ���ϵͳ������Ӧ��ָ��



GNU/Linux�ϣ�CMakeĬ������Unix Makefile��������Ŀ��

* Makefile��make������ָ����������Ŀ
* CMakefile��������ʱ�ļ���Ŀ¼��CMake���ڼ�����ϵͳ����������
* cmake_install.cmake������װ�����CMake�ű�������Ŀ��װʱʹ��
* CMakeCache.txt��CMake���棬CMake��������������ʱʹ������ļ�



## 1.2 �л�������

**׼������**

CMake��Բ�ͬƽ̨֧�ֱ��ع��������б�

ʹ��cmake --help����ƽ̨���ҵ��������������Լ��Ѱ�װ��CMake�汾



**����ʵʩ**

ʹ��==-G==�л�������

1. ʹ�����²���������Ŀ

```cmake
mkdir -p build
cd build
cmake -G Ninja ..
```



2. ������Ŀ

```cmake
cmake --build .
```



## 1.3 ���������Ӿ�̬��Ͷ�̬��

��Ŀ���ж��Դ�ļ���ͨ���ֲ��ڲ�ͬ��Ŀ¼��

�������ӽ�չʾ��ν�Դ������뵽���У��Լ����������Щ��



**׼������**

�޸�hello-world.cpp������Message.h�е��࣬����ʵ����Message.cpp��



**����ʵʩ**

�����������ļ���Ҫ���룬������Ҫ�޸�CMakeLists.txt

�����У��Ȱ����Ǳ����һ����



1. ������̬�⣬��������Դ���ļ�����ͬ

```cmake
add_library(message
  STATIC
    Message.h
    Message.cpp
  )
```

2. ����hello-world��ִ���ļ���Ŀ�겿�ֲ���Ҫ�޸�

```cmake
add_executable(hello-world hello-word.cpp)
```

3. ��󣬽�Ŀ������ӵ���ִ��Ŀ��

```cmake
target_link_libraries(hello-world message)
```

4. ����Ŀ�������ú͹���

```cmake
mkdir build
cd build
cmake ..
cmake --build .
```



**����ԭ��**

��������������

* add_library�����ɱ�Ҫ�Ĺ���ָ���ָ����Դ����뵽����

  * ��һ������ΪĿ��������ʹ����ͬ���������ÿ�
  * ���ɿ���ݵڶ���������STATIC��SHARED���Ͳ���ϵͳȷ��

  

* target_link_libraries���������ӵ���ִ���ļ���������ǰҪ��ɿ�Ĺ���

����ɹ��󣬹���Ŀ¼����message.lib��̬���hello-world��ִ���ļ�



CMake��������ֵ��Ϊadd_library�ĵڶ�����������Чֵ��������õ���ֵ��

* STATIC��������̬�⣬�������ļ��Ĵ���浵���Ա�����������Ŀ��ʱʹ��
* SHARED��������̬�⣬���Զ�̬���ӣ�������ʱ���صĿ�
* OBJECT����Դ����뵽Ŀ���ļ������鵵����̬���У�Ҳ���ܽ��������ӵ���������С�����Ҫһ���Դ�����̬��Ͷ�̬�⣬��ôʹ�ö������������
* MODULE��DSO�飬��SHARED��ͬ�������ӵ���Ŀ���κ�Ŀ�꣬�������Զ�̬����



CMake���������������͵Ŀ⣬�����ڹ���ϵͳ�в��������

* IMPORTED�������Ŀ���ʾλ����Ŀ�ⲿ�Ŀ⡣��Ҫ��;�Ƕ�������������й������ÿⱻ��Ϊ���ɱ��
* INTERFACE����IMPORTED�����ƣ����������Ϳ�ɱ䣬û��λ����Ϣ����Ҫ������Ŀ֮���Ŀ�깹��ʹ��
* ALIAS�����ֿ�Ϊ��Ŀ���Ѵ��ڵĿⶨ�������������ΪIMPORTED��ѡ�����



������ʹ��add_library����Դ���룬�����½ڽ�ʹ��target_sources�㼯Դ����



**������Ϣ**

OBJECT��ʹ�ã�

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



��Ҫ��֤�����Ŀ���ļ�������λ���޹ء�����ͨ��ʹ��`set_target_properties`�������`message-objs`Ŀ�����Ӧ������ʵ��

���ڣ�����ʹ��������������ȡ��̬��(`message-static`)�Ͷ�̬��(`message-shared`)



## 1.4 ����������Ʊ���

�����ṹif-else-endif



**����ʵʩ**

1. ��Message.h��Message.cpp����Ϊһ���⣬Ȼ�����ӵ�hello-world��ִ���ļ���
2. ��Message.h��Message.cpp��hello-world.cpp������һ����ִ���ļ��������ɿ�



* ���ȶ���CMake�汾����Ŀ���ƺ�֧�ֵ����ԣ�

```cmake
cmake_minimum_required(VERSION 3.5 FATAL_ERROR)
project(recipe-04 LANGUAGES CXX)
```

* �����±���USE_LIBRARY���߼�������ֵΪOFF������ӡ��ֵ

```cmake
set(USE_LIBRARY OFF)
message(STATUS "Compile sources into a library? ${USE_LIBRARY}")
```

* Ȼ������һ������_source������Message.h��Message.cpp

```cmake
list(APPEND _source Message.h Message.cpp)
```

* ����if-else��䣬��Ϊ�棬����Ϊһ����

```cmake
if(USE_LIBRARY)
	add_library(message ${_source})
	add_executable(hello-world hello-world.cpp)
	target-link_libraries(hello-world message)
else()
	add_executable(hello-world hello-world.cpp ${_source})
endif()
```



**����ԭ��**

* �����߼�������Ϊ1��ON��YES��true��Y�������������Ϊtrue
* �����߼�������Ϊ0��OFF��NO��false��N��IGNORE��NOTFOUND�����ַ�������Ϊfalse



## 1.5 ���û���ʾѡ��

ʹ��option()���ʹ��ֵ���Դ��ⲿ�����޸�



**����ʵʩ**

* ��һ��ѡ���滻��һ��ʾ���е�set(USE_LIBRARY OFF)����

```cmake
option(USE_LIBRARY "Compile sources into a library" OFF)
```

* ����ͨ��cmake�� -Dѡ�����Ϣ���ݸ�CMake

```cmake
cmake -D USE_LIBRARY=ON ..
```

-D��������ΪCMake�����κ����͵ı������߼�������·����



**����ԭ��**

option�ɽ�������������

```cmake
option(<option_variable> "help string" [initial value])
```

* <option_variable>��ʾ��ѡ��ı���������
* ��help string����¼ѡ����ַ���
* [initial value]ѡ��Ĭ��ֵ��������ON��OFF



**������Ϣ**

��ʱѡ�������������

CMake�ṩ==cmake_dependent_option()==����������������������ѡ���ѡ��

```cmake
include(CMakeDependentOption)

cmake_dependent_option(
	MAKE_STATIC_LIBRARY "Compile sources into a static library" OFF
	"USE_LIBRARY" ON)
	
cmake_depent_option(
	MAKE_SHARED_LIBRARY "Compile sources into a shared library" ON
	"USE_LIBRARY" ON)
```

���USE_LIBRARYΪON����MAKE_STATIC_LIBRARYĬ��ֵΪOFF



CMake���ʵ��Ļ��ƣ�ͨ������ģ������չ���﷨�͹���

�����а�����һ����ΪCMakeDependentOption��ģ�飬��û��include���ģ�飬cmake_depent_option()���������



## 1.6 ָ��������

**����ʵʩ**

CMake�����Եı������洢��CMAKE_ <LANG> _COMPILER�У�<LANG>���κ�һ������

* ʹ�� -Dѡ��

```cmake
cmake -D CMAKE_CXX_COMPILER=clang++ ..
```

* ͨ��������������

```cmake
env CXX=clang++ cmake ..
```



��������裬�����������ڱ�׼·���п��ã��������ã���Ҫ��������������ִ���ļ�·�����ݸ�CMake



**����ԭ��**

����ʱ��CMake�����ƽ̨���ԣ�ȷ����Щ���������ã��Ƿ��ʺϵ�ǰ��Ŀ



**������Ϣ**

�鿴���õı�����

```cmake
cmake --system-information information.txt
```



CMake�ṩ��������������������

* CMAKE _ _<LANG>_ _COMPILER_LOADED������Ŀ��������<LANG>������Ϊtrue
* CMAKE __<LANG>_ _COMPILER_ID����������ʶ�ַ�������Ӧ������

����



## 1.7 �л���������

CMake�������ù������ͣ���Debug��Release�ȣ������������ѡ�������

�������ɹ���ϵͳʹ�õ����ñ�����==CMAKE_BUILD_TYPE==��Ĭ��Ϊ��



CMakeʶ��ֵΪ��

* Debug��û���Ż������£�ʹ�ô��е��Է��Ź�������ִ���ļ�
* Release�����ڹ������Ż��Ŀ���ִ���ļ������������Է���
* RelWithDebInfo���������ٵ��Ż�����ִ���ļ����������Է���
* MinSizeRel�����ڲ�����Ŀ������С���Ż���ʽ������������ִ���ļ�



**����ʵʩ**

* ����CMake�汾����Ŀ���Ƶ�

```cmake
cmake_minimum_required(VERSION 3.5 FATAL_ERROR)
project(recipe-07 LANGUAGES C CXX)
```

* ����һ��Ĭ�ϵĹ�������Release������ӡһ����Ϣ���ñ�����Ϊ�������������ͨ��������б༭

```cmake
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type" FORCE)
endif()
message(STATUS "Build type : ${CMAKE_BUILD_TYPE}")
```

* ��֤�������

```cmake
mkdir -p build
cd build
cmake ..
```

* �л���������

```cmake
cmake -D CMAKE_BUILD_TYPE=Debug ..
```



**������Ϣ**

CMakeҲ֧�ָ�����������������������visual studio��Xcode�����ǿ���ͬʱ����������

����==CMAKE_CONFIGURATIONTYPES==��������Щ�������Ŀ����������ͽ��е���

��Visual Studio��CMake���ã�

```cmake
mkdir -p build
cd build
cmake .. -G"Visual Studio 12 2017 Win64" 
	-D CMAKE_CONFIGURATION_TYPES="Release;Debug"
```



��ΪRelease��Debug��������һ����������Ȼ�����ʹ��--config��־���������ĸ�

```cmake
cmake --build . --config Release
```



## 1.8 ���ñ�����ѡ��

CMakeΪ��������չ��������־�ṩ�˺ܴ������ԣ�

* CMake������ѡ����ΪĿ�����ԣ���˿��Ը���ÿ��Ŀ�����ñ���ѡ��
* ʹ�� -D��־ֱ���޸�==CMAKE_ <LANG> _FLAGS _<CONFIG>����



��һ�����㲻ͬ������״������룬computer_area.cpp

����ʵ�ֲַ��ڲ�ͬ�ļ��У�ÿ��������״��һ��ͷ�ļ���Դ�ļ����ܹ�4��ͷ�ļ���5��Դ�ļ�

```cmake
���� CMakeLists.txt
���� compute-areas.cpp
���� geometry_circle.cpp
���� geometry_circle.hpp
���� geometry_polygon.cpp
���� geometry_polygon.hpp
���� geometry_rhombus.cpp
���� geometry_rhombus.hpp
���� geometry_square.cpp
���� geometry_square.hpp
```



**����ʵʩ**

* ����CMake��Ͱ汾

```cmake
cmake_minimum_required(VERSION 3.5 FATAL_ERROR)
```

* ������Ŀ���ƺ�����

```cmake
project(recipe-08 LANGUAGES CXX)
```

* ��ӡ��ǰ��������־

```cmake
message("C++ compiler flags: ${CMAKE_CXX_FLAGS}")
```

* ΪĿ��׼����־�б�����һЩ���޷���Windows��ʹ��

```cmake
list(APPEND flags "-fPIC" "-Wall")
if(NOT WIN32)
	list(APPEND flags "-Wextra" "-Wpedantic")
endif()
```

* ���geomerty��

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

* Ϊ�ÿ�Ŀ�����ñ���ѡ��

```cmake
target_compile_options(geometry PRIVATE ${flags})
```

* ������compute-areas��ִ���ļ���Ϊһ��Ŀ��

```cmake
add_executable(compute-areas compute-areas.cpp)
```

* Ϊ��ִ��Ŀ�����ñ���ѡ��

```cmake
target_compile_options(compute-areas PRIVATE "-fPIC")
```

* ��󽫿�ִ���ļ����ӵ�geometry��

```cmake
target_link_libraries(compute-areas geometry)
```



**��ι���**

����ѡ����������������Ŀɼ��ԣ�

* PRIVATE������ѡ��ֻӦ����ָ��Ŀ�꣬���ᴫ�ݸ���Ŀ����ص�Ŀ��
* INTERFACE����������ѡ�ֻӦ����ָ��Ŀ�꣬�����ݸ���Ŀ����ص�Ŀ��
* PUBLIC��Ӧ����ָ��Ŀ���ʹ������Ŀ��



���ַ�ʽ��ӱ���ѡ�����Ӱ��CMakeȫ�ֱ���CMAKE_ <lang> _FLAGS _<CONFIG>



һ�ַ����ǣ�ʹ��CMake������Ĳ������ݸ����ع�������

```cmake
mkdir -p build
cd build
cmake ..
cmake --build . -- VERBOSE=1
```

�ڶ��ַ�����ʹ��CMake�����������ã�����������쳣������ʱ���ͱ�ʶ

```cmake
cmake -D CMAKE_CXX_FLAGS="-fno-exception -fno-rtti" ..
```

����



## 1.9 Ϊ�����趨��׼

3.1�汾�У�CMake������һ��������ƽ̨�ͱ������Ļ��ƣ�����ΪC++��C�������Ա�׼

ΪĿ������<LANG> _STANDARD����



**׼������**

ʾ�����붨���˶���Ķ�̬��ʹ��std::unique_ptr��Ϊ�ṹ�еĻ���

```c++
std::unique_ptr<Animal> cat = CAT("Simo");
std::unique_ptr<Anima> dog = Dog("Marlow");
```

ʹ�ù�����ʽʵ�ֹ��캯��

```c++
typedef std::function<std::unique_ptr<Animal>(const 
std::string &)> CreateAnimal;
```



**����ʵʩ**

* ����CMake�汾��

```cmake
cmake_minimum_required(VERSION 3.5 FATAL_ERROR)
project(recipe-09 LANGUAGES CXX)
```

* Ҫ����Windows�ϵ������п����

```cmake
set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)
```

* ����Դ����Ϊ��̬��

```cmake
add_library(animals
	SHARED
	Animal.cpp
	...
)
```

* ΪĿ���������ԣ��Ա�����ʹ��һЩ����������DSOʱ��������

```cmake
set_target_properties(animals
	PROPERTIES
	CXX_STANDARD 14
	CXX_EXTENSIIONS OFF
	CXX_STANDARD_REQUIRED ON
	POSITION_INDEPENDENT_CODE 1)
```

* Ȼ��Ϊ��ִ���ļ����һ����Ŀ�꣬����������

```cmake
add_executable(animal-farm animal-farm.cpp)
set_target_properties(animal-farm
	PROPERTIES
	CXX_STANDARD 14
	CXX_EXTENSION OFF
	CXX_STANDARD_REQUIRED ON)
```

* ����ִ���ļ����ӵ���

```cmake
target_link_libraries(animal-farm animals)
```



**����ԭ��**

* CXX_STANDARD ������������Ҫ�ı�׼
* CXX_EXTENSIONS ����CMAKE��ֻ����ISO C++��׼�ı�������־������ʹ����չ
* CXX_STANDARD_REQUIRED ָ����ѡ��׼�İ汾������ΪOFFʱ����Ѱ����һ��׼�����°汾�����Ȳ���C++14��Ȼ��C++11��Ȼ��C++98



## 1.10ʹ�ÿ�����

CMake���ṩ����ѭ�������Թ��ߣ�==foreach endforeach==��==while endwhile==

���߶�������break���ʹ��



**׼������**

���ü���ʾ��



**����ʵʩ**

* ָ���汾��

```cmake
cmake_minimum_required(VERSION 3.5 FATAL_ERROR)
project(recipe-10 LANGUAGES CXX)
add_library(geometry
	STATIC
	geometry_circle.cpp
	...)
```

* ʹ��-03�������Ż��������⣬��Ŀ������һ��˽�б�����ѡ��

```cmake
target_compile_options(geometry
	PRIVATE
		-03)
```

* ����һ��Դ�ļ��б��Խϵ͵��Ż�ѡ����б���

```cmake
list(APPEND sources_with_lower_optimization
	geometry_circle.cpp
	geometry_rhombus.cpp)
```

* ѭ����ЩԴ�ļ��������ǵ��Ż��������-02��

```cmake
message(STATUS "Setting source properties using IN LISTS syntax:")
foreach(_source IN LISTS sources_with_lower_optimization)
	set_source_files_properties(${_source} PROPERTIES COMPILE_FLAGS -02)
	message(STATUS "Appending -02 flag for ${_source}")
endforeach()
```

* ������compute-areas��ִ��Ŀ�꣬����geometry��������ȥ

```cmake
add_executable(compute-areas compute-areas.cpp)
target_link_libraries(compute-)
```





































