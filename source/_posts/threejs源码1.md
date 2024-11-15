---
title: threejs源码学习
tags:
 - threejs
categories:
 - WebGL
comments: true
date: 2024-10-25 10:34:05
photos: https://wallroom.io/img/3840x2160/bg-81f2d39.jpg
cover: https://wallroom.io/img/3840x2160/bg-81f2d39.jpg
---

# 说明

对很多知识点只有模糊的概念，除了日常写写demo，就来看看源码培养些感觉；

从BoxGeometry入手;

现阶段的大纲图：

![1729864326918.jpg](https://s2.loli.net/2024/10/25/iQyVkM8svcFLNKG.png)

# BoxGeometry

标了阅读思路，主要是理解buildPlane函数的参数是怎么设定的、顶点是怎么计算的、坐标系是怎么放的、UV是怎么算的，还有就是怎么利用索引来减少存储。


![e081babf6c7c640380917d4eb24af47.jpg](https://s2.loli.net/2024/10/25/Hj4Tq2ofp37be5a.jpg)

主要是懒，而且写个完整思路和笔记博客太费时间了。。。。

高清版在[GitHub](https://github.com/youxt-njnu/ProjectSnapshot/blob/master/threejs-BoxGeometry.pdf)自取