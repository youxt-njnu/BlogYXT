---
title: Mapbox SDK for Unity | 官方案例Ⅱ
date: 2024-02-12 20:45:16
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

# Zoomable map

`<b>`Worldwide Dynamic Zoom & Panning Support `</b>`
它里面用了SpawnOnMap的脚本，来在地图上自定义符号。
This example is a starting point for creating a traditional web-based zoomable map. Go anywhere in the world and check out Mapbox’s high-quality satellite imagery. It also uses the SpawnOnMap script to instantiate custom markers on the map at specified locations.

# Iconic buildings

涉及到对于高精度的模型，怎么放置到地图上；

`<mark>`replace features `</mark>`的demo是这个的具体操作，Globe的demo里也有放置了自定义标签在地图上的操作；

在abstract map里也可以挂一些modifier，然后设置modifier里的参数；

通过这个，可以实现自定义的高精度模型摆放到固定位置，然后点击图标导航到对应的位置；
![](https://s2.loli.net/2023/11/03/7leL9EIJFfqsoBQ.png)

# POI placement

这个里面就是给了POI，然后也能根据POI实现相应的交互；

有一个教程：https://blog.mapbox.com/build-ar-experiences-with-places-data-48a4856c482f

POI点，添加标注，有触发交互；

这些POI点都是真实地理位置的，可选择的，可搜索的；可以通过名称、类别、地址等进行搜索，也比较方便；

有一个简单的添加场景、添加POI标注的教程；

## 详细学习

额外知识点：
[【Unity】对 Center/Pivot 和 Global/Local 的理解_unity global和local-CSDN博客](https://blog.csdn.net/keneyr/article/details/114380509)
[Unity中Update，FixedUpdate，LateUpdate的区别 - 南山砍柴 - 博客园 (cnblogs.com)](https://www.cnblogs.com/dlyedu/p/7648463.html)
[光晕层 (Flare Layer) - Unity 手册](https://docs.unity.cn/cn/2019.4/Manual/class-FlareLayer.html)
[深入了解Unity的QualitySettings类：一份详细的技术指南(三)-CSDN博客](https://blog.csdn.net/qq_33795300/article/details/131492273)

Scene学习：

| GameObject  | 作用         | 组件                                                                                                                                                                                                          |
| ----------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Main Camera | 相机         | ChangeShadowDistance.cs([QualitySettings - Unity 脚本 API](https://docs.unity.cn/cn/2019.4/ScriptReference/QualitySettings.html),修改阴影的绘制距离)；`<br>` CameraMovement.cs(拖拽和缩放地图，以及相机的设置) |
| LabelCamera | 专门标签显示 | Culling Mask的设置💡                                                                                                                                                                                          |

# AstronautGame

`<b>`Astronaut Game `</b>`
This is an enhanced version of the "Location-based Game Starting Point." It features custom styling, procedural vehicle movement based on real road data, and dynamic lighting. Click on the road in the editor to navigate the astronaut character to a point via the Directions API. On a device, this example uses GPS location information to move and rotate the character.

和导航相关的，有物体的移动；
基于**LocationBasedGame**那个预制件，是 `<mark>`location based game `</mark>`那个demo的进阶版；

这个挺有用的，就是点击屏幕上一个点，宇航员会导航过去，同时光照也可以恰当实现，而且地图也会自动放缩，保证人始终在屏幕的中心区域；
但导航的一个问题就是会走到楼里面，会穿越交通；
车辆也比较假；特别是移动的时候；

## map设计

此外这个很支持自定义，包括自定义图层和风格的加入，但是需要构建一个密钥，因为需要访问到我账户里的所有风格url；

对地理要素的显示的筛选也很细致了；
![](https://s2.loli.net/2023/11/03/vu5mz6GfLYCQ297.png)

涉及到自定义风格这种，给了两个案例视频：
https://www.youtube.com/watch?v=RhG1kfDBhgM
https://www.youtube.com/watch?v=-RMdkG0VL4A

[c#的namespace和using](https://www.w3cschool.cn/csharp/csharp-namespaces.html)
[Unity3D中[SerializeField]特性的使用](https://blog.csdn.net/yangyong0717/article/details/71512251)
![](https://s2.loli.net/2023/09/04/kprgie4NDudyM6S.png)

## Canvas

## LocaitonProvider

LocationProviderFactory.cs
DeviceLocationProvider.cs
EditorLocationProvider.cs
LocationArrayEditorLocationProvider.cs
TransformLocationProvider.cs
DeviceLocationProviderAndroidNative.cs

## TrafficAnimator

调整交通流的速度

## Target

ImmediatePositionWithLocationProvider.cs
RotateWithLocationProvider.cs

## MapboxAstronaut

## RaycastPlane

# Playground

对一些API使用的示例：
Directions：输入起始点、目的地，给出两者之间的路线；
ForwardGeocoder：输入地点的名称，给出地点的GeoJSON数据；
LocationProvider：当从device location provider中加载读入的时候，用于debug的工具；
RasterTile：输入一个地点名称，返回该地区的风格化的栅格瓦片styled raster，还可以自定义调节地图的层级
RemoveZ-Fighting
ReverseGeocoder
VectorTile

# VoxelMap像素地图

`<b>`Real World Data, Minecraft-inspired Map `</b>`

This Minecraft-inspired example demonstrates a less traditional way to consume Mapbox data for maps or world construction.
VoxelTile is responsible for fetching both a styled raster tile and a mapbox.terrain-rgb (global elevation) tile. The styled raster pixels are sampled to determine which voxels to generate, via the VoxelFetcher. This is achieved using a nearest color formula. The elevation tile pixels are sampled to determine where to vertically place the voxels.
Zoom: the zoom level at which to request the tiles.
Elevation Multiplier: used to exaggerate the real-world height.
Voxel Depth Padding: determine how many voxels to spawn below the designated height. This helps fill holes in environments with extreme elevation variations.
Tile Width in Voxels: How many voxels to generate across each tile. This will affect the detail of the world. Raster textures are downsampled according to this value.
Voxel Batch Count: The number of voxels to spawn at once. Keep this number low to prevent locking the main thread during construction.
