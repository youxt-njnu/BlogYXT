---
title: 基于位置的场景切换:mapbox+unity
tags:
  - AR
  - mapbox
categories:
  - Unity
comments: true
date: 2023-09-11 09:46:05
photos: https://wallroom.io/img/3840x2160/bg-4101f05.jpg
cover: https://wallroom.io/img/3840x2160/bg-4101f05.jpg
---

> 实验参考教程：
> 
> https://youtu.be/RfvVZ5bq5FU?si=Fbfq5eMf28HTW99M
> 
> How to make a location based (map) game in unity tutorial
> 
> - Intro
> 
> - Importing to Unity; Changing Unity Layout; Switching Platforms
> 
> - Location Provider; Importing Game Object
> 
> - Spawn on Map; Assigning Events; Creating Event Pointer
> 
> - Rotating Pointer; Rotating Script; Transfer Script; Rotation Effect; Testing
> 
> - Checking Distance
> 
> - Accessing Event Location
> 
> - UI Manager

# 初始设置

新建项目、选择Unity版本、导入mapbox sdk for unity和assetbundle browser；
设置项目开发环境（Android），页面布局（2/3）；
打开LocationbasedGame的预制场景，隐藏dismiss的panel；

# 添加event和交互

调整物体位置的提供脚本 ![](https://s2.loli.net/2023/09/05/JLxHkXZ5c7wIegM.png) 添加物体预想的位置 ![](https://s2.loli.net/2023/09/05/tWjfD6x9aShEwK8.png) 调整map ![](https://s2.loli.net/2023/09/05/Yp2ROQEwmyaFBM6.png)

❗❗❗map的locaiton设置、locationarray的设置均无效；

> 参考[解决方案](https://github.com/mapbox/mapbox-unity-sdk/issues/1618) 就是把Location Provider Factory(Script)里Editor Locaiton Provider的Script改成“EditorLocaitonArrayProvider"

❗❗❗Google Map上的位置无法对应到实际物体应该处在的位置；

> 使用qgis里加载的mapbox或者OSM底图获取坐标，此时坐标系对应的是WGS84

添加标注及其组件 ![](https://s2.loli.net/2023/09/05/rXkPENtnszQ9Bbe.png)

> ⭐创建的空物体可以命名前面加上-，像'-EventSpawner'
> Marker Prefab里面我选择了MapboxPin
> 作者使用的Pointer来自于[link→Location Pointer and Geo location library](https://learn.gold.ac.uk/course/view.php?id=21324) 里面的GeoCoordinate.cs可以获取于[作者的Github](https://github.com/Nima-Jamalian/GeoCoordinatePortable) 此外，子物体已有mesh collider，是否要给Pointer添加Box Collider，可以参考的[链接1](https://www.cnblogs.com/hangzhe/p/7425871.html)、[链接2](https://discussions.unity.com/t/mesh-collider-vs-box-collider/39038/2)

调整标注位置

> 修改EventPoint.cs，实现对物体的旋转和浮动 [UnityEngine.Time](https://docs.unity.cn/cn/2019.4/ScriptReference/Time.html)

初始化预制件prefab

> 修改SpawnOnMap.cs，获取到位置

鼠标点击事件

> 使用的prefab里挂了脚本，所以有报错
> 给prefab的mesh collider取消激活，给MapboxPin添加了Box Collider然后才会激活鼠标点击事件

![](https://s2.loli.net/2023/09/05/z3sONPtYdHpZ9cm.png)

获取player的位置

> 修改Canvas里的LocationStatus.cs脚本
> 求解player和event之间的距离

UI界面管理

> 设置Canvas的大小模式为缩放 ![](https://s2.loli.net/2023/09/05/tHkuK63yrXfFj9M.png) 新建MenuUIManager.cs，挂到Canvas上
> Canvas里new一个Panel为EventPanelUseInRange，调整大小、设备自适应设置居中
> 添加child text(font size, width/height, scale)和两个btn(width/height, text的font size)，调整大小 ![](https://s2.loli.net/2023/09/05/ZxO7XmcHtu8kLCy.png) 同理构建第二个UI界面 ![](https://s2.loli.net/2023/09/06/3wPfob7pzdKcsNW.png)

UI界面切换

> 编辑MenuManager.cs和EventPointer.cs
> 根据距离判断展示哪个UI界面
> 进行UI界面状态判断，并给按钮挂载点击事件
> 记得把两个界面拖进unity里面

新建两个UI切换的场景

> 先给eventpoint添加ID属性，存储起来
> 构建切换场景，solid color, text(size, width/height, anchor) ![](https://s2.loli.net/2023/09/06/wMdhpY7sbaKPlIU.png)

构建脚本，实现进入新场景

> 新建物体、关联脚本EventManager.cs ![](https://s2.loli.net/2023/09/06/3gKVrZAaniP7mQx.png) 存储eventID，并挂载加入btn的函数

利用SceneManagement进行场景切换

> 编写代码，unity里拖入
> build settings里添加三个场景

场景返回

> 添加返回按钮
> EventUIManager里实现场景返回

XLua转换
