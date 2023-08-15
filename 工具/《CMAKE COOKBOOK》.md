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



























