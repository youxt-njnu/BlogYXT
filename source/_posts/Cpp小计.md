---
title: Cpp小计
date: 2023-05-10 19:45:36
tags:
 - 知识框架
 - 专业课
categories:
 - Cpp
---





# 引言

课程目的：

> 训练软件开发的系统思维；
>
> 让程序更有设计感；
>
> 了解GIS软件的内核结构；
>
> 掌握C++编程的技巧和思维模式；

问题核心：

> 不是语言、算法、数据结构；
>
> 是架构、模式、方法论

GIS内核的模块组成：

> 数据模块：矢量、栅格、索引，数据库系统、文件系统、网络
>
> 投影模块
>
> 空间分析模块
>
> 制图模块，图形API

Cpp

> 特点：编译、静态、大小写敏感、多编程范式
>
> 组成：核心语言、标准库、标准模板库

Cpp与C的区别

# 类和对象

面向对象，UML

构造函数、析构函数、引用计数、内存管理

控制成员变量可访问性：public private protected

函数重载

浅拷贝深拷贝

静态成员

友元

内联函数inline

继承：多重继承

类型转换

多态

虚函数

函数重载和运算符重载

命名空间

标准C++流库iostream

字符串与string类

# 模板与STL

函数模板、类模板

template, typename, class

STL:

> vector, map, list, queue, set
>
> algorithm, numeric
>
> iterator
>
> functional
>
> stack, deque
>
> memory
>
> utility

# 项目组织

c++开发：多文件、多工程、多编译目标、引用第三方库

代码复用：源代码级别、库复用Nuget，包复用vcpkg，组件复用COM

工程

构建系统、元构建系统cmake、IDE如visual studio

静态库.lib、动态库.dll

通用宏、预处理器、链接器、活动配置

# GUI程序

图形用户界面

面向对象开发，MFC、QT

图形API：GDI+, OpenGL, Direct3D

句柄

MFC应用程序

# GDI+

