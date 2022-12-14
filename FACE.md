# 1. 一种符合FACE标准的机载IO软件实现方法

分两部分，架构研究和IO软件设计。



## 1.1 架构研究

IO 服务组件段的具体实现方式有分布式和非分布式两种。

分布式是指 IO 服务组件段作为运行实体具有自己独立的地址空间和调度资源，可单独执行; 非分布式是指 IO 服务组件段没有独立的地址空间和调度资源，以函数库的形式连接到其他进程的地址空间，必 须依附其他进程才能执行。



介绍了两个架构的优缺点。

分布式优点：组件段都有独立运行实体，有更好的可移植性和可管理性。

分布式缺点：使软件模块交互变复杂，数据传递的延时增加。

非分布式优点：最大限度减小了了运行实体的数量，且能够有效降低软件模块间的交互复杂性 和操作时延。

非分布式缺点：需要与应用软件或平台特定服务联编后存在同一地址空间中，这 些函数库的修改会导致分区软件映像的重新链接，使系统集成更加困难。



## 1.2 IO软件设计

从模式设计、架构设计、关键模块设计和接口设计四方面。



1. 模式设计

采用适配器设计模式。（作为两个不兼容接口之间的桥梁，使得原本由于接口不兼容而不能一起工作的那些类可以一起工作）

将具体设备驱动提供的特定接口，通过适配器将特定接口转换为满足 FACE 标准( Target) 的接口，提供给平台特定服务段软件( Client) 使用。

实现上层应用软件与设备驱动间的解耦。

每个适配器实例对应一个 特定的设备驱动，如果要增加新的设备支持只需再增 加一个适配器实例即可，由适配器实例负责将特定的 设备驱动接口转换为 FACE 标准的 IO 服务接口

IO 服务软件应以动态库的形式部署在嵌入 式操作系统的分区里，方便部署。



2. 架构设计

包括 OS ( 操作系 统) 适配器模块、用户接口模块、初始化模块、日志输出 模块、配置管理模块、缓冲区管理模块、驱动管理模块、 连接管理模块、驱动适配器模块九个功能单元。

**OS 适配模块**封装操作系统的接口和头文件，**提高IO 服务软件在不同操作系统上的可移植性**。**用户接 口模块提供 IO 服务初始化、打开连接、关闭连接、读数 据、写数据、获取连接状态等接口**，支持 1394B、离散 量、ARINC 429 等设备。**初始化模块完成 IO 服务软件 库的初始化**，申请 IO 服务软件所需的资源，如内存、信 号量等。**日志输出模块控制 IO 服务软件调试日志的 动态输出**，用户可根据使用场景配置日志的输出级别， 以后可扩展将日志输出到文件。**配置管理模块完成用 户配置数据的解析、检查、打印功能**。**缓冲区管理模块 管理用户数据缓冲区**，提供缓冲区初始化接口和数据 缓冲功能。**驱动管理模块实现驱动的查找、安装、卸载 功能**。**连接管理模块实现应用到设备逻辑连接的管 理**，包括连接的查找、初始化、释放、连接状态的更新以 及连接控制结构的初始化。**驱动适配器模块采用适配 器设计模式，实现驱动接口转换为 IO 服务软件内部接 口**，将驱动数据格式转换为 IO 服务软件内部数据，支 持 1394B、离散量、ARINC 429 等设备驱动



3. 关键模块设计

多个应用同 时访问设备的情况，每个应用建立一条独立的虚拟连 接，每个连接都应该有独立的句柄。

在 IO 服务软件内部维护一套独立的连接描述符句 柄，如图 4 所示，每个连接描述符保存设备的配置信 息、设备状态信息、读写数据状态、设备驱动接口表。



**设备驱动管理结构**

打 开设备列表、设备链表、驱动列表。

流程有以下五步：

* 调用 IO 服务组件段软件 提供的初始化接口，**遍历驱动列表的每一个表项**，进行 设备驱动的注册，在**设备链表中创建对应的表项**，设备 驱动入口函数表加入表项。
* 平台特定服务段软件调用 IO 服务组件段软件 提供的打开接口，**遍历设备链表找到对应的设备及驱 动入口函数表**，在打开设备表中找到空闲表项，**将设备 驱动入口函数表填入打开设备列表**，设备链表对应表 项的引用计数加 1，将打开设备列表对应表项的虚拟 连接描述符返回给调用者
* 平台特定服务段软件**调用 IO 服务组件段软件提 供的读、写接口**，根据打开设备列表的虚拟连接描述符， 找到对应设备驱动入口函数表，调用驱动的读写接口
* 平台特定服务段软件调用 IO 服务组件段软件 提供的关闭接口，根据打开设备列表的虚拟连接描述 符，将打开设备列表的表项清零，在设备链表中找到对 应的表项，将表项的引用计数减 1。
* 平台特定服务段软件调用 IO 服务组件段软件 提供撤销注册接口，遍历设备链表找到对应的表项，如 果表项的引用计数为零，直接删除对应表项，如果引用 计数不为零，返回设备忙。



4. 接口设计

IO 服务组件段软件接口包括调用接口、数据接口 和配置接口。

调用接口：

配置接口：传入配置信息，配置数据用于配 置 IO 通用服务和具体 IO 设备的特性。

数据接口：三级数据缓冲区控制结 构，，IO 服务层数据缓冲区控制结构存放 IO 服务层关心的数据，设备数据缓冲区控制结构 存放特定设备驱动关心的数据，用户数据缓 冲区存放用户数据。



# 二 机载计算机中间件技术研究

中间件特指分布式中间件，是指位于操作系统和应用软件之 间的一层系统软件，用于屏蔽底层硬件、操作系统、编程语言、网 络等平台差异，为上层应用软件提供统一的应用编程接口，可使 得网络环境下的应用方便地交互和协同，一般提供通信支持、并 发支持、公共服务、系统管理等基本功能。

要求：

**支持跨平台实时分布式计算系统**：由有人机和多架无人机组成的作战群的新需求，国外开 展了跨平台的互操作体系结构研究。解决这些异 构、动态的多种平台的互操作，满足动态的组网、动态的任务规划 和作战任务分配、控制层次的动态变化，实现安全的、实时的互操 作能力。



**支持分布式综合化模块化结构 DIMA**：分布式 IMA 在解决小型飞机的 SWap 目标时具有优势。 Topia 技术公司在航电系统中开展了面向服务的体系结构（SOA） 的探索，将整个航空电子系统的处理机和网络以一个整体的异构 平台的形式向航电应用提供服务。



支持网络安全能力：内核层仅仅包含提供分区隔离机制的最 小功能集合 ；其他传统的操作系统功能（如设备驱动、文件系统、 分区安全通信软件等）则以中间件的形式存在。
