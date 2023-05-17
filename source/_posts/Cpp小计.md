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

图形设备接口

通过C++类实现

组成：二维矢量图形（几何、画笔、画刷）、图像、文字与布局

常用的类/对象：

> Pen
>
> Color
>
> Brush
>
> HashBrush
>
> LinearGradientBrush
>
> PathGraidentBrush
>
> TextureBrush
>
> ......

坐标、矩阵

# GIS内核结构和基础环境

UML统一建模语言

> 类图class diagram
>
> object diagram
>
> use case diagram
>
> sequence diagram
>
> collaboration diagram
>
> state chart diagram
>
> acitivity diagram
>
> component diagram
>
> deployment diagram

设计模式

面向对象的设计模式：接口编程、优先使用对象组合而非继承

常用模式

> 组合模式
>
> 迭代器模式
>
> 观察者模式

OGC Simple Feature规范

# 地图渲染与符号模型

绘图环境

> 绘图句柄
>
> 坐标变换
>
> 绘图范围

简单符号系统的实现

> 管理符号
>
> GDI+原生绘图能力进行绘制
>
> 重载和继承进行扩展
>
> 支持序列化
>
> 符号的编辑和预览

# 交互式绘图

变换模型

> 平面映射
>
> 矩阵旋转

用户交互响应：鼠标事件、坐标事件

用户编辑

# 序列化技术

把对象转变为字节序列（文件、网络流、内存数据块）

文本（JSON、XML、Yaml）、二进制

# 组件技术基础

COM: 组件对象模型，component object model

COM核心概念：系统注册表、COM组件库、COM运行库

COM线程套间

接口定义语言interface define language

类型库typelib

全局统一标识符global unique identifier

COM接口

ATL，activeX template library

#import

MSXML

# 矢量数据存储

基于ACCESS

元数据表、图层数据表、

ADO: activeX data object. 数据访问技术

* connection
* command
* recordset

OGR，最通用的矢量数据访问开源库

* wkb(well known binary), wkt(well know text)
* spatial reference
* feature
* feature definition
* layer
* data source
* drivers
* ogrsfdriverregister

# 坐标系统

分离查询与渲染职责

经纬度坐标与笛卡尔坐标系之间的变换

地图投影：PROJ算法库

栅格渲染与显示、逆向重采样

