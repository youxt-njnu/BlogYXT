---
title: Cesium API学习 Ⅰ
tags:
 - Cesium
categories:
 - 3DGIS
 - WebGL
comments: true
date: 2024-06-13 11:06:05
photos: https://static-ali.ihansen.org/app/bg1440/-eptxZKAmRQ.jpg!o
cover: 
---
# 初步了解

**Cesium官网** [Cesium: The Platform for 3D Geospatial](https://cesium.com/)

**CesiumJS:**[CesiumJS Quickstart – Cesium](https://cesium.com/learn/cesiumjs-learn/cesiumjs-quickstart/)

**Cesium for Unity:**[Cesium for Unity – Cesium](https://cesium.com/learn/unity/)

[Hello React Cesium - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/452345666)

[React集成Cesium+ThreeJs流程汇总_react cesium-CSDN博客](https://cuifeng.blog.csdn.net/article/details/122828984?spm=1001.2014.3001.5502)

# 基础教程

[Cesium学习资源汇总](https://www.cnblogs.com/tiandi/p/16580238.html)

[cesium官方案例](https://sandcastle.cesium.com/)

[Cesium相关资料汇总](https://github.com/vtxf/Cesium-Tutorials-Index)、[汇总2](https://zhuanlan.zhihu.com/p/557296232)

[cesium项目](https://pasu.github.io/ExamplesforCesium/examples/examples.html#Primer)

[Cesium中文教程-github](https://github.com/hujiulin/CesiumJS-tutorial/tree/master?tab=readme-ov-file), [cesium中文网](http://cesium.xin/wordpress/archives/16.html)

---

**Cesium源码文件夹**[cesium官方案例](https://sandcastle.cesium.com/)，[官方API文档](https://cesium.com/learn/cesiumjs/ref-doc/)

# Cesium基础知识

[学习链接](https://www.zhihu.com/column/c_1317874447190585344)

**Cesium的项目定位：B/S架构下的客户端应用层面的三维开发框架**

![img](https://pic1.zhimg.com/v2-d566ed3f3b0bb0f8be5d099fb2393b10_r.jpg)

**Cesium的学习路线：**

![img](https://pic4.zhimg.com/v2-7a32e7833314deff496ff31b5f48809f_r.jpg)

**Cesium进阶之路：**

* **Web前端方向：** Cesium与webpack（裁剪以及压缩），Cesium 与vue（框架设计， 嵌入复杂业务系统），Cesium的UI（UI 设计，定制可复用的Cesium交互界面）
* **计算机图形学方向：** WebGL深入，基于Cesium 的可视化定制（视阈、水淹、水面、热力图，流场图、飞线图、扫描图）
* **数据预处理方向：** 投影变换，空间索引，LOD ，3dtile 生成，数据存储，数据分发服务，解决超大空间数据如何在 Cesium上流畅可视化的问题。

## Viewer

![img](https://i.loli.net/2018/08/13/5b70f7cbf3693.jpg)

**Cesium ion是一个提供瓦片图和3D地理空间数据的平台，Cesium ion支持把数据添加到用户自己的CesiumJS应用中。下面我们将使用Sentinal-2二维贴图和Cesium世界地形，二者都需要ion的支持。**

**备注** 在我们使用Cesium的过程中，如果没有申请ion，同时没有自己的数据源用的cesium提供的数据源，viewer的底部常常会提示一行小的英文字母。大意就是需要申请access token.

1. **打开**[https://cesium.com/ion/](https://cesium.com/ion/) 然后注册一个新的账户。
2. **点击"Access Token",** 选择*Default*默认的access token拷贝到contents中。

```
Cesium.Ion.defaultAccessToken =
        'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiI2NDk3OTUzYy01N2RjLTQyNDMtYTE1Zi0yYjgwNTJlZmYwOTAiLCJpZCI6MTk3MzM4LCJpYXQiOjE3MDg2NTc1NTh9.b8_XHsKZtIQVkUyk95dNvHHB4OE5nmebm5e_JFEIIbM'
const viewer = new Cesium.Viewer('cesiumContainer')
```

**Viewer的类组织**

![img](https://img2022.cnblogs.com/blog/237138/202208/237138-20220812154924431-1555328076.png)

### 参数解释

* **infoBox, 点击后弹出的右上方信息**
* **selectionIndicator: 点击后在原地出现的绿色指示框**
* **trackedEntity: Gets or sets the Entity instance currently being tracked by the camera.**
* **orderIndependentTranslucency:**

## 坐标系

* **屏幕坐标，即二维笛卡尔平面坐标系**
* **笛卡尔空间直角坐标/世界坐标**（红X，绿Z，蓝Y；X向前，Y向右，Z向上；
* **地理坐标（弧度制），Cesium.Cartographic(lon, lat, height)，其中经纬度都是用弧度表示的**
* **经纬度坐标，默认WGS84坐标系，**

**坐标变换：**

* 经纬度坐标转世界坐标：Cesium.Cartesian3.fromDegrees(lng, lat, height)
* 世界坐标转经纬度坐标：Cesium.Cartographic.fromCartesian(cartesian3)
* 地理坐标（弧度制）转经纬度坐标：lng=Cesium.Math.toDegrees(cartographic.longitude), lat=Cesium.Math.toDegrees(cartographic.latitude), height=cartographic.height;
* 弧度转经纬度：Cesium.Math.toDegrees(radians)
* 经纬度转弧度：Cesium.Math.toRadians(degrees)
* 屏幕坐标转世界坐标：scene.globe.pick(viewer.camera.getPickRay(windowPosition), scene) —— 一定要保证屏幕坐标在球上，不然得到的就是undefined
* 世界坐标转屏幕坐标：Cesium.SceneTransforms.wgs84ToWindowCoordinates(scene,cartesian3)

![](https://s2.loli.net/2024/04/07/QRdUelrvqxNkWHO.png)![](https://s2.loli.net/2024/04/07/QRdUelrvqxNkWHO.png)

**空间变换：**

`只有转换到笛卡尔坐标系后才能运用计算机图形学中的仿射变换知识进行空间位置变换如平移旋转缩放。Cesium为我们提供了如下几种很有用的变换工具类：`

* Cesium.Cartesian3（相当于Point3D）
* Cesium.Matrix3（3x3矩阵，用于描述旋转变换）
* Cesium.Matrix4（4x4矩阵，用于描述旋转加平移变换）
* Cesium.Quaternion（四元数，用于描述围绕某个向量*旋转一定角度的变换）
* Cesium.Transforms(包含将位置转换为各种参考系的功能)

**Cesium目前支持的坐标系：**

* **WGS1984， Terrain的TileSceme默认是这个**
* **墨卡托投影，Imagery的TileSceme默认是这个**

`viewer.canvas.clientHeight - movement.endPosition.y` :

**在 Cesium 和其他 WebGL 应用中，**`viewer.canvas` 元素的坐标系通常遵循 HTML Canvas 元素的常见布局：

1. **坐标系起点** ：Canvas 坐标系的原点 (0, 0) 位于 Canvas 的左上角。
2. **X轴方向** ：X轴从左向右增长，即坐标值从 0 增加到 `canvas.width`。
3. **Y轴方向** ：Y轴从上向下增长，即坐标值从 0 增加到 `canvas.clientHeight`。

针对代码行 `viewer.canvas.clientHeight - movement.endPosition.y`，这里所表达的含义和坐标参考如下：

* **`viewer.canvas.clientHeight`** ：这是 Canvas 元素的高度，也就是 Y 轴的最大值。
* **`movement.endPosition.y`** ：这通常是鼠标事件（如点击或拖动事件）结束时的 Y 坐标位置。在 Cesium 中，这个坐标也遵循 Canvas 的坐标系，即 Y 坐标从上到下增大。

## ImageryProvider

加载影像底图API: `CesiumImagery.vue, ImageryLoading.vue`

> **Cesium.: ImageryLayer, IonWorldImageryStyle, SingleTileImageryProvider, GridImageryProvider, TileCoordinateImagery**
>
> **viewer:**
>
> * **.scene.imageryLayers: scene里的，更明确**
> * **.imageryLayers: Gets the collection of image layers that will be rendered on the globe. 更简洁，两者功能一样**

**两者对比**

**Imagery Provider使用特定的服务请求Tiles，而Layer表示来自Imagery Provider的displayed tiles。比如说,**

`const layer = layers.addImageryProvider(imageryProvider);`

**是**

```
const layer = new Cesium.ImageryLayer(imageryProvider);
layers.add(layer);
```

我们通常构建一个imagery provider只是为了创建一个layer; 然后我们使用layer的属性(如show、 alpha、brightness和contrast)操作该图层以改变其视觉外观。解耦decoupling imagery provider和layer使得编写新的imagery provider变得更加容易。

与上面示例中的Layer一样，imagery layer collection决定了图层绘制的顺序。layer是根据添加的顺序从底部到顶部绘制的。像Cesium中的任何其他集合一样，使用add, remove and get等函数操作imagery layer collection。此外，layer可以使用 raise、 raiseToTop、 lower 和 lowerTobottom 来重新排序。

Mapbox影像加载：

mapbox://styles/foxziluliu1121/clrzymvix009g01r67bowdcez

[https://api.mapbox.com/styles/v1/foxziluliu1121/clrzymvix009g01r67bowdcez/wmts?access_token=pk.eyJ1IjoiZm94emlsdWxpdTExMjEiLCJhIjoiY2xyYnJ5MXQ3MHN5ZjJrcHFxcXdwemN3bSJ9.zc_BQ5U2vldx1wROrYM4tg](https://api.mapbox.com/styles/v1/foxziluliu1121/clrzymvix009g01r67bowdcez/wmts?access_token=pk.eyJ1IjoiZm94emlsdWxpdTExMjEiLCJhIjoiY2xyYnJ5MXQ3MHN5ZjJrcHFxcXdwemN3bSJ9.zc_BQ5U2vldx1wROrYM4tg)

* **ImageryLayer, ImageryProvider,**
* **Vector Tiles API:  **[https://c.tiles.mapbox.com/v4/foxziluliu1121.66qkqrxn/2/3/0.png?access_token=pk.eyJ1IjoiZm94emlsdWxpdTExMjEiLCJhIjoiY2xyYnJ5MXQ3MHN5ZjJrcHFxcXdwemN3bSJ9.zc_BQ5U2vldx1wROrYM4tg](https://c.tiles.mapbox.com/v4/foxziluliu1121.66qkqrxn/2/3/0.png?access_token=pk.eyJ1IjoiZm94emlsdWxpdTExMjEiLCJhIjoiY2xyYnJ5MXQ3MHN5ZjJrcHFxcXdwemN3bSJ9.zc_BQ5U2vldx1wROrYM4tg)
* **static tiles API:  **`https://api.mapbox.com/styles/v1/foxziluliu1121/clrzymvix009g01r67bowdcez/wmts?access_token=pk.eyJ1IjoiZm94emlsdWxpdTExMjEiLCJhIjoiY2xyYnJ5MXQ3MHN5ZjJrcHFxcXdwemN3bSJ9.zc_BQ5U2vldx1wROrYM4tg`
* **Mapbox的数据格式：**
  * **mapbox.mapbox-streets-v8**
  * `foxziluliu1121.9mqo2ps2`

## terrainProvider

地形图层只能有一个；

地形类型：

* STK World Terrain
* Small Terrain

terrainProvider:

* EllipsoidTerrainProvider
* CustomHeightmapTerrainProvider

Ceisum案例：

```
Cesium.Terrain.fromWorldTerrain({
    requestWaterMask: true,
    requestVertexNormals: true,
})
```

## 加载矢量数据

* CustomDataSource
* CzmlDataSource
* GeoJsonDataSource, GeoJson, TopoJson
* KmlDataSource
* 

## 空间数据可视化

### Entity API

以Graphics结尾：

* billboard, box, corridor, cylinder, ellipse, ellipsoid, label
* model, tileset, path, plane,
* point, polygon, polyline, polylineVolume, rectangle, wall,

**使用注意点：**

在Cesium中，`Cesium.BoxGraphics`是用来定义一个实体（Entity）的盒状几何体的可视化属性的类。这个类本身并不负责将几何体添加到场景中；相反，它被用作描述实体的视觉外观的一部分。为了在Cesium的3D场景中显示一个盒状几何体，你需要将这个几何体与一个实体（Entity）关联，并将该实体添加到Cesium的实体集合中。这就是为什么在实践中，你看到的示例代码通常会使用 `viewer.entities.add()`来创建和添加带有盒状几何体的实体。

这里是一个简化的示例，说明如何使用 **`Cesium.BoxGraphics`通过 `viewer.entities.add()`添加一个盒状几何体到场景中：

```
var viewer = new Cesium.Viewer('cesiumContainer');

var box = viewer.entities.add({
    position: Cesium.Cartesian3.fromDegrees(-100.0, 40.0, 200000.0),
    box: {
        dimensions: new Cesium.Cartesian3(400000.0, 300000.0, 500000.0),
        material: Cesium.Color.BLUE.withAlpha(0.5),
        outline: true,
        outlineColor: Cesium.Color.BLACK
    }
});

viewer.zoomTo(viewer.entities);
```

**在这个示例中：**

* `viewer`是 `Cesium.Viewer`的一个实例，它是Cesium应用的主要界面。
* `viewer.entities.add({...})`是添加新实体到场景的方法。这个方法的参数是一个对象，描述了实体的各种属性，包括位置（`position`）、形状（在这个例子中是一个盒子，通过 `box`属性定义）等。
* `box`属性使用 `Cesium.BoxGraphics`的一个实例来定义盒状几何体的外观，包括尺寸（`dimensions`）、材质（`material`）、轮廓线是否可见（`outline`）以及轮廓线的颜色（`outlineColor`）。

**总之，**`Cesium.BoxGraphics`提供了定义盒状几何体视觉外观的方式，而 `viewer.entities.add()`则是将这样定义的盒状几何体实体添加到Cesium场景中的方法。这种设计允许开发者以灵活的方式创建和管理场景中的对象，同时提供了丰富的API来定义这些对象的视觉外观。

EntityCluster类+PinBuilder类：实现billboards, points, labels的聚合

Entity管理：viewer.entities的类型是EntityCollection，包括了EntityCollection类里面的所有属性和方法，如.add(), .contains(), .getById(), .remove(), .removeAll(), .removeById()

Entity拾取：viewer.scene.pick(), viewer.scene.drillPick()

Entity固定：heightReference属性值，

### Primitive API

加载数据的性能更好，可以更好的加载3D Tiles数据。

Cesium.GeometryInstance()

Cesium.EllipsoidSurfaceAppearance

Cesium.Primitive()

scene.primitives.add()

### 材质

![](https://s2.loli.net/2024/04/04/9bVKQRSEtL6Dnw4.png)

**MaterialProperty:**

* Cesium.ColorMaterialProperty
* Cesium.ImageMaterialProperty
* 

## GLTF格式

![img](https://pic2.zhimg.com/80/v2-a26039ef8457742c7121e9532289da51_1440w.webp)

**Cesium加载gltf:**

* viewer.entities.add()
* viewer.scene.primitives.add(), Cesium.Model.fromGltf()

## 3D Tiles格式

3D Tiles可以理解为带有LOD的glTF,为流式传输和渲染海量3D数据而设计，例如倾斜摄影、3D建筑、BIM/CAD，实例化要素集和点云。3D Tiles定义了一种数据分层结构和一组切片方式。

**组成：**

* 描述瓦片集Tileset的JSON文件 —— tileset.json
* 一组瓦片Tile
  * 瓦片对象的格式可以：b3dm, i3dm, pnts, cmpt

**常用类：**

* Cesium3Dtileset：用于流式传输大量的异构3D地理空间数据集；
* Cesium3DTileStyle：瓦片集样式；
* Cesium3DTile：数据集中的一个瓦片;
* Cesium3DTileContent：瓦片内容；
* Cesium3DTileFeature：瓦片集要素，用于访问Tile中批量表中的属性数据，可通过scene.pick方法来获取一个 BATCH，即三维要素。Cesium3DTileFeature.getPropertyNames() 方法获取批量表中所有属性名，Cesium3DTileFeature.getProperty(string Name) 来获取对应属性名的属性值。

**操作：**

1. 加载：viewer.scene.primitives.add(new Cesium.Cesium3DTileset())

### 参数

#### Cesium3DTileset

[官网链接](https://cesium.com/learn/cesiumjs/ref-doc/Cesium3DTileset.html?classFilter=Cesium3DTileset)

`featureIdLabel` 属性在 Cesium 的 `Cesium3DTileset` 对象中用于指定用于拾取（picking）和样式化（styling）的特征 ID 集合。以下是对这段文档的详细解释和它的作用：

1. **选择特征 ID 集合**：
   * 在 `Cesium3DTileset` 中，`featureIdLabel` 用来标识和选择特定的特征 ID 集合。这对于执行拾取操作和应用样式至关重要，尤其是当模型包含多个特征 ID 集合时。
2. **EXT_mesh_features 和 EXT_feature_metadata**：
   * 对于遵循 `EXT_mesh_features` 扩展的数据，`featureIdLabel` 可以是特征 ID 的 `label` 属性，如果未指定，则默认为 `"featureId_N"`（N 是 featureIds 数组中的索引）。
   * 对于旧的 `EXT_feature_metadata` 扩展，没有 `label` 字段，所以特征 ID 集总是以 `"featureId_N"` 标识，其中 N 是所有特征 ID 列表中的索引，特征 ID 属性在特征 ID 纹理之前列出。
3. **特征 ID 标签的自动转换**：
   * 如果 `featureIdLabel` 被设置为一个整数 N，它将自动转换为字符串 `"featureId_N"`。这意味着你可以简单地指定一个索引，Cesium 会自动解析为正确的特征 ID 标签。
4. **优先级**：
   * 如果同时存在每个基元（per-primitive）和每个实例（per-instance）的特征 IDs，实例特征 IDs 有优先权。这对于决定在冲突时哪组特征 ID 应该被使用是重要的。

**作用**

* **拾取和样式化**：
  * 通过指定 `featureIdLabel`，开发者可以精确控制哪一组特征 ID 被用于交互（如鼠标拾取）和视觉样式化。这在处理包含复杂数据集的大型 3D Tiles 模型时尤其有用。
* **灵活性和控制**：
  * 提供了对模型特征的更精细的控制，允许开发者基于具体的特征 ID 集来应用样式和执行拾取，特别是在模型中嵌入了多种数据层（如不同的物理部分或不同类型的数据标注）时。
* **实验性特性**：
  * **重要的是要注意，**`featureIdLabel` 是一个实验性特性，属于 3D Tiles 规范的一部分，该规范尚未最终确定。因此，使用这个特性时需要谨慎，因为它可能会在没有标准弃用政策的情况下改变。

`featureIdLabel` 是一个强大的工具，用于在复杂的 3D Tiles 数据集中进行精确的交互和视觉表示。它使得开发者可以根据特定的特征 ID 集来定制模型的表现和行为，增强了模型与用户交互的能力和灵活性。但是，由于它的实验性质，使用时需要关注 Cesium 更新和变更。

## Cesium的Property

**基本property**

* CompositeProperty
* ConstantProperty
* SampledProperty
* TimeIntervalCollectionProperty

**Material的property**

**PositionProperty**

**其他**

![img](https://pic4.zhimg.com/80/v2-9cb0b7aa0c14747709a9dc455c7dccf7_720w.webp)

## 组件重写

homeButton回到主要页面；

Geocoder组件通过OSM服务实现；

自定义BaseLayerPicker:

* viewer.baseLayerPicker.viewModel.imageryProviderViewModels
* viewer.baseLayerPicker.viewModel.terrainProviderViewModels

## 事件监听

let handler = new Cesium.ScreenSpaceEventHandler(viewer.canvas)

let eventType = Cesium.ScreenSpaceEventType.LEFT_CLICK

handler.setInputAction((event) => {}, eventType)

Keyboard: camera.moveForward, .moveBackward, .moveUp, .moveDown, .moveLeft, .moveRight

场景渲染: scene.preUpdate, .postUpdate, .preRender, .postRender

## 相机控制

viewer.flyTo(target, options)

viewer.zoomTo(target, offset)

viewer.camera.flyTo(options)

viewer.camera.flyToBoundingSphere(boundingSphere, options)

viewer.camera.lookAt(target, offset)

viewer.camera.lookAtTransform(transform, offset)

viewer.camera.setView(options)

## 测量

距离

面积

handler.setInputAction(func(), Cesium.ScreenSpacEventType.RIGHT_CLICK)

## 调试面板

CesiumInspector

> 了解Cesium渲染效果以及性能调优
>
> viewer.extend(Cesium.viewerCesiumInspectorMixin)

Cesium3DTilesInspector

> 用于监视3D Tiles数据的监视器
>
> viewer.extend(Cesium.viewerCesium3DTilesInspector)

## 粒子系统

new Cesium.ParticleSystem()

## 场景后处理

postProgress类

postProgressStage

postProgressStageLibrary

postProgressStageCollection

## 与第三方库集成

ThreeJS

1. 初始化两个容器，和各自的渲染器
2. 调整各自的渲染频率和相机一致
3. 加入要展示的内容

ECharts

Heatmap

Turf

## 源码打包

gulp, webpack
