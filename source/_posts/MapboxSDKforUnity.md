---
title: Mapbox SDK for Unity | 官方案例Ⅰ
date: 2024-02-12 20:39:16
tags:
 - mapbox
 - Unity
 - 开源
categories:
 - AR开发
comments: true
photos: https://wallroom.io/img/2880x1800/bg-983381f.jpg
cover: https://wallroom.io/img/2880x1800/bg-983381f.jpg
---
# City simulator

主要是对基础场景的搭建，也包括了基础数据的一些图层的设置；

在瓦片地图之上，也可以进一步加载建筑数据、POI数据、路网数据、实时交通数据；

*location-based game*里面涉及到了Player；
*Data explorer*的demo是对其的补充和优化；
*Zoomable map*的demo实现了基于底图的地点的搜索和缩放；

## city simulator/location-based game

界面

* Camera：Position，侧俯视地面
* Light：Position，这两个部分都需要好好学习一下
* 叠加的数据源来自于MAP LAYERS里的Data Source，可以自定义

脚本

* InitializeMapWithLocationProvider
* ImmediatePositionWithLocationProvider
  初始化mapbox的底图、player的位置、

> 在EditorLocationArrayProvider中定义物体的初始位置，也就是第一个点；
> 但无法实现绕着路走

## Data explorer

**界面**

* 为什么要叠加两个摄像机，以及Camera的设置
* 光照的location改变没啥影响？？？？

**modifier**

modifier都继承自ScriptableObjects

GameObject Modifiers：

> Game object modifiers ran after the mesh modifiers and game object creation.Their main purpose is to work on game object and decorate/improve them in their own ways.They ran for each game object individually.
> It's possible to do lots of different things with GameObject Modifiers.A simple example would be **MaterialModifier, which simply sets random materials to gameobject and submeshes**.A more complicated example would be **SpawnInside Modifier which instantiates prefabs in a polygon, like trees in a park**.
> Any operation, you want to perform on generated entity, that would **require a game object is a good candidate for game object modifiers**. For example, things like adding a collider or animation would require a gameobject hence cannot be done in mesh modifier .
> Game object modifiers is the suggested way of **customizing generated game object** and we expect developers to fully utilize this by creating their own custom game object modifiers.

> Behavior modifiers are [`ScriptableObjects`](https://docs.unity3d.com/Manual/class-ScriptableObject.html) that work with meshes and game objects to further allow you to decorate, enhance, or make modifications to your game objects.

一个相关的问题：https://www.appsloveworld.com/gamedev/100/how-to-find-intersections-using-mapbox-unity-sdk?expand_article=1
一个视频教程：https://youtu.be/cy49zMBZvhg?si=IDRboEIbW-wRPmjY
一个挺好的文字版教程：[Unity进阶：ScriptableObject使用指南-CSDN博客](https://blog.csdn.net/qq_46044366/article/details/124310241)

**CreateAssetMenu**

> 类上面添加一行
> [CreateAssetMenu(fileName = "Bullet", menuName = "New Bullet", order = 1)]
> 作用在 Assets 文件夹下，鼠标右键，菜单栏中添加一个按钮项，菜单名为 menuName，并执行生成名为 fileName 的脚本，order 为按钮显示顺序
> https://blog.csdn.net/lizhenxiqnmlgb/article/details/95603822

**map layers**

mapbox.3d-buildings,mapbox.mapbox-streets-v7,mapbox.mapbox-traffic-v1,mapbox.mapbox-terrain-v2
地址：https://studio.mapbox.com/tilesets/
涉及图层：

> v7的已经找不着了，现在是v8的；
> 其他的也有更新，然后可以在mabox studio的tileset里，或者是mapbox预设的style里找到；但第一个3d_buildings感觉是mapbox官方账号自己定义的一个Tileset
> 下面的是以POI Label为例，v8里面的filterrank为1的显示为紫色的label
> ![](https://s2.loli.net/2023/10/09/mNjZlvcTKGfg5Ww.png)
> POI Label里还有一个prefab modifier，是scriptableobject的自定义子类，实现对label的批量设置；通过编写PrefabModifier，然后里面有三个变量。构建这个的实例是通过右键菜单里找自己定义的路径来实现的
> ![202310091154803.png](https://s2.loli.net/2023/10/09/1ITDPpornREuX35.png)
> PrefabModifier实现了对Prefab的初始化到清除的生命流程；
> LabelPrefab里面
> ![](https://s2.loli.net/2023/10/11/ByH5LGlu1TkzeFd.png)

**Pois**
![img](https://s2.loli.net/2023/10/11/npbivGUcyPQXJqo.png)
PoiDemo3dPoiModifier(Prefab Modifier): 还是继承自Prefab Modifier，里面的Prefab是下面这样的：
![](https://s2.loli.net/2023/10/11/pm4WBKrvJ2RMoLu.png)
FeatureBehaviourModifier(Feature Behaviour Modifier): 继承自Feature Behaviour Modifier
![](https://s2.loli.net/2023/10/11/6depCAaUzYlI3Qo.png)

**主要功能体现**

![](https://s2.loli.net/2023/10/13/XtVmPIbZWaQh8rS.png)

[UGUI（三）- Graphic Raycaster - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/102057291)
[Unity 几种排版方式：Layout Group_unity layout group-CSDN博客](https://blog.csdn.net/xinzhilinger/article/details/111186083)
![](https://s2.loli.net/2023/10/15/PlW6df23cHAbBhv.png)

### 实现脚本

**ReloadMap.cs**

1. Awake的时候，暂存相机起始位置，将当前地图更新到给定的缩放值，如果slider值发生了变化则reload地图
2. 如果查询了地址，则进入函数，函数能让地图在缩放比例下，并以查询到的经纬度为地图的中心
3. 

❗不知道HeroBuildingSelectionUserInput这个写在里面的作用
这个后面有案例涉及到的时候再看，还有一个就是点击出现对应名称的实现没看到❗

**ForwardGeocodeUserInput.cs**

1. 保证输入框有Input Field的属性
2. Awake的时候获取到输入框，并对输入框的更改完成状态添加监听器
3. 初始化用于查询的“源”
4. mapbox的Geocoder api，根据输入框的内容进行地理编码
5. 如果没查到，输入框的内容就是”没有编码响应“
6. 如果查到了，则获取到坐标、响应、OnGeocoderResponse

涉及到的知识点：
[【Unity小知识】RequireComponent-CSDN博客](https://blog.csdn.net/huoyixian/article/details/90386326)

### POI的逻辑

在POI结构里的两个脚本

* PoiMarkerHelper.cs

  > 实现对poi信息点击后，打印出来
  >
* FeatureBehaviour.cs

> 初始化参数；
> 创建primitive下的物体，设置名称、位置、父物体、位置
> 展现DebugData信息

在POI数据里的两个modifier

* PrefabModifier.cs

> 创建右键菜单
> 构建POI的组织结构
> 设置位置
> 设置清除的方式

* FeatureBehaviourModifier.cs

> 给poi挂载FeatureBehaviour的component

## InteractiveStyledVectorMap

`<b>`Styling Based on Vector Tile Data `</b>`
可以和特定的建筑物进行交互，展示他们对应的属性信息；
差异化对建筑物进行渲染贴图；
This is a vector tile map where you can interact with individual buildings to show their associated information (contained in the vector tile).
This example also shows how to procedurally decorate land use and how to render building types differently based on their classification.

### LoadingScreenCanvas

实现加载界面字颜色的变换

### Map设计❗

（表示代码和细节还没看，现在看的只是逻辑和实现；
后面所有的案例就都是这样的）
![](https://s2.loli.net/2024/02/12/7X85hElzSJi1ks6.png)

modifiers:

* ObjectInspector(Object Inspector Modifier)
* HighLightModifier(Add Behaviours Modifier)
* AddToTreeCollectionModifier(Add To Collection Modifier)
* ParkLayerModifier(Layer Modifier)
* SpawnInParkModifier(Spawn Inside Modifier)
* DisableMeshRendererModifier(Disable Mesh Renderer Modifier)

gameobject上的结构：

* RangeTileProvider.cs
* HighlightFeature.cs
* FeatureSelectionDetector.cs
* FeatureUiMarkder.cs

### InteractiveSelectionCanvas❗

实现点击对应物体，呈现出对应的信息
这个主要是在Map里的InteractiveSelectionCanvas里实现的

## TrafficAndDirections

`<b>`Traffic and Directions `</b>`
通过交通矢量数据图层，提供交通信息。
通过简单的三维几何对交通进行了可视化；
给出了两个点，可以任意移动两个点，采用Direction API会返回两点之间的最短距离。
Mapbox provides traffic information through the traffic vector layer. In this example you can see an additional traffic map id added to Abstract Map under Map Layers segment. It's then visualized by a simple 3D geometry. The Directions API returns the shortest route between two points. You can drag & drop the two markers to update the route in this example. The Directions API call can also be used for walking directions.

### Map设计(地图)

Modifiers:

* TrafficLoft(Loft Modifier)
* LowTrafficMaterial(Material Modifier)
* ModerateTrafficMateral(Material Modifier)
* HeavyTrafficMaterial(Material Modifier)
* Severe Traffic Material(Material Modifier)

### TrafficUvAnimator(速度)

控制交通流的速度

### Directions(点)

有两个点
DirectionsFactory.cs
DragableDirectionWaypoint.cs

direction waypoint entity(最短路线)

在运行时动态生成，
