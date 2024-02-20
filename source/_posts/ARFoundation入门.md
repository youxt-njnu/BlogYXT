---
title: ARFoundation学习
tags:
 - 软件端AR
 - ARFoundation
categories:
 - AR开发
comments: true
date: 2024-02-17 08:44:05
photos: https://4k.wpcoder.cn/wp-content/uploads/2023/09/1_1695198805-1600x780.png
cover: https://4k.wpcoder.cn/wp-content/uploads/2023/09/1_1695198805-1600x780.png
---

# 学习方式

> 加入一些 ar 技术交流群，看一些博客。博客推荐_davidwang_的 arfoundation 之路，csdn 上的。
> 可以读一些官方文档，像 ARCore 和 ARKit 的。每年的 wwdc 苹果全球开发者大会都会有人讲 ARKit 有哪些新的功能，可以去理解底层是如何设计的。unity 官方也会出一些视频讲 ar 方面的内容
> ![](https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/46F5DA628CA079FB5612771E5407D8AE.png)

# 打包教程

 https://www.bilibili.com/video/BV1Qt4y1a7aW?p=11&vd_source=5270415d668b21206238403450bb29b5
 notes:
 ios的打包[教程](https://cloud.tencent.com/developer/article/2098356)，需要有ios的环境（但我们没有苹果电脑，此外我在尝试给实验室工位上的电脑装虚拟机，之前试过一次失败了；可以回头搜一搜哔哩哔哩，找找怎么配置）
 https://blog.51cto.com/myselfdream/2493033
 2

# 实操案例

[AR Foundation | AR Foundation | 5.1.0 (unity3d.com)](https://docs.unity3d.com/Packages/com.unity.xr.arfoundation@5.1/manual/index.html)

AR Foundation手册：
https://docs.unity3d.com/cn/2021.3/Manual/com.unity.xr.arfoundation.html

## Gaze Interaction in Unity+Mapbox🔍

> https://www.youtube.com/watch?v=Xhz3cmnluNo&list=PL88ZsSjUqiTD0CJAdMQ9MO3m43mxVjMoU&index=1
> https://www.youtube.com/watch?v=OE66gtiF8QQ&list=PLNFBI1dJIh36ku4BVYGBEcvDuQAGSrcbn&index=14

安装包：AR Foundation; ARCore XR Plugin; ARKit XR Plugin
删除初始相机，构建XR->AR Session Origin;
构建XR-> AR Session, XR->AR Default Point Cloud, XR->AR Default Plane
为AR Session Origin添加component：AR Point Cloud Manger, AR Plane Manager, AR Raycast Manager,并把之前的Point Cloud和Plane的gameobject挂载上去；

新建3D物体Plane，并确保zero out(就是reset为初始化0)，这个plane就是为了实现AR的shadow和stuff的渲染；删除mesh collider，并重命名为ContentParent; 所有它的子物体都会显示在AR的场景下；

新建子物体Cube，然后设置Scale为(0.2,0.2,.0.2),因为在AR的里面，一个单位对应的是一米，原始的1太大了。此外，把物体的y设置为0.1，这样物体就看着是放置在平面上了。

为了不看到plane,把mesh render设置为非激活状态；

之后添加toggle，来实现对点云呈现与否的切换：

> UI->Canvas, RenderMode:ScreenSpace-Overlay, UIScaleMode: ScaleWithScreenSize
> UI->Toggle: 修改锚点到左上角，Pivot为x=0,y1=1,然后调整PosX=0,PosY=0; Toggle内部的把Text删掉，设置Background锚点下为Stretch，right为0；然后回到Toggle，设置宽高；再回到background里的checkmark，设置为stretch，调整left=0, top=0, right=0,bottom=0

新建Scripts文件夹，构建脚本PlaceContent.cs, ToggleAR.cs

> PlaceContent挂在ContentParent上；
> ToggleAR挂在Toggle上，同时Toggle添加OnValueChanged的事件，拖入的是Toggle的Toggle AR的脚本组件

切换环境到安卓，并添加main到Scenes in Build里；
修改PlayerSettings里的PackageName为com.matt.GazeInteraction; 并同时修改到IOS的包里面；

之后在playsettings里

> IOS: 激活requires arkit support，修改target minimum iOS version为11.0;修改Architecture为arm64；
> Android: 删除Graphics APIs里的Vulkan; 取消勾选Multithreaded rendering;  Minimum API Level为Android 7.0 'Nougat'(API level 24)；修改scripting backend为IL2CPP，勾选ARM64

黑屏问题解决：https://blog.51cto.com/u_15127683/4151076

对Cube修改大小scale为(0.5,0.5,0.5);
create Empty: SectionInfo
create Quard: InfoParent;
略微拉高SectionInfo, 修改Quard的rotation(0,180,0), scale(2,1,1)

新建材质文件夹，构建trans材质，设置为rgb0-1.0,调整透明度为0.7,黑色背景。

对InfoParent构建child object:Cube和Text(TMP)

> 前者拉下来，然后调整scale，添加材质；
> 后者
>
> * 因为一开始添加之后字体扭曲了distorted，所以修改y的scale为2，
> * 在Scene上方找到Gizmos并激活（这里我没有找到，而是默认就激活了，就是那个圆形按钮一样的带一个下拉框的，这个选项是让text上显示黄色线框）；
> * 调整width和height（不要再修改scale了），使得text的边框可以和InfoParent大小一致
> * 设置text里的内容居中，字体大小fontsize随着线框大小变化auto size，最小为0，最大为1000
> * 如果在上y右x的坐标下，text显示在infoParent的里面，则需要把text往外拉；
> * 修改里面的内容为Here is some infos.
> * 关掉线框
> * 为了让SectionInfo缩放的时候，感觉是从cube弹出来的，需要先把InfoParent从子物体拉出来，然后修改SectionInfo的Y大小为0.5，然后再把InfoParent拉进来还原为子物体。这个时候缩放SectionInfo就正常了

让text的Extra Settings里的order in layer设置为1，可以首先显示出来。
新建材质，命名为Blue，修改颜色为蓝色，然后给Cube这个材质；

添加脚本InfoBehaviour，实现SectionInfo的缩放，并给到Cube上（因为Cube上放了Box Collider），也拖入SectionInfo作为transform的那个参数；

为了让SectionInfo会始终面向摄像机，也就是随着相机旋转，添加脚本FaceCamera;

> 我们不希望SectionInfo的x和z轴方向旋转，所以需要设置为0；
> 为了进行验证，加上了[ExecuteInEditMode]，这样就可以在编辑模式里面通过移动摄像机测试是否生效（为了让项目中的摄像机可以被获取到，需要在参数设置面板中设置为main camera

之后新建gaze脚本，挂到AR Camera上（main camera)

> 获取到射线投射到的物体，通过tag判断是否是有信息的可交互的gameobject

```
补充知识点：
forward：Returns a normalized vector representing the blue axis of the transform in world space.
语法：public Vector3 forward;
该属性可以默认设置为局部坐标系Z轴的正向移动，同样的属性在Vector3类也有，与Transform类不同的是Transform.forward在考虑旋转的同时进行移动，而Vector3.foward不考虑旋转因素，若物体在旋转时发生移动，则移动情况会发生改变，也就是Transform.forward使用的时局部坐标系，若要忽略旋转对物体移动的影响，我们使用Vector3.foward，也会是说Vector3.foward用的是世界坐标系，这里补充一个小细节，Unity默认的坐标轴颜色，x轴为红色，z轴为蓝色，y轴为绿色，
```

修改Game为800*480的Landscape，然后看到toggle有点大，所以进行width和height的大小，和location的位置，移动到左上角。

## 入门教程

https://www.bilibili.com/video/BV1VT4y1z7MH?p=1&vd_source=5270415d668b21206238403450bb29b5

> arfoundation教程里具备环境之后的操作：
> 登录：developer.apple.com/download/
> 需要一个新版本的Mac OS，Xcode(ios 13);
> 作者安装了macOS Catalina 10.15 beta, iOS 13 beta

新建场景scene；
新建gameobject：

> ar session
> ar session origin(在原点的地方新建了一个，然后里面的相机就是AR的相机)， add components(ar plane manager(script)
> 新建一个empty gameobject，add components(ar plane, ar plane mesh visulizer, mesh collider, mesh filter, mesh renderer, line renderer),针对line renderer，设置lighting里的shadow\width\materials\corner vertices\en cap vertices\use world space\loop ----------> 也就是xr里的ar default plane
> 在ar session origin里的ar pane manager关联一下内置的prefab: ar default plane
> 新建脚本PlacementController.cs并挂到ar session origin上面
> 添加自定义的物体prefab，然后就可以打包了

Selection 场景

> 添加PlacedObject, 挂载Placement Object.cs
> 在AR Session Origin上，挂载PlacementWithManySelectionController.cs
> notes: 注意，一个是物体的位置设置，可以把相机拉远一点，如果物体不在视锥体里面，是在相机里看不到的；
> 其他部分还是保持默认即可
> 这个最后的效果是以手机处为中心的，所以做不到锚点定位
> 但这个静态的，就可以考虑加上地形的一套

## 室内导航demo

https://www.youtube.com/watch?v=fuHFrMZ4q_s

## ardraw demo

## 官方demo

https://github.com/Unity-Technologies/arfoundation-samples/tree/4.2

## ar云锚点等需要的功能

[bilibili视频](https://www.bilibili.com/video/BV1fX4y137BY/?vd_source=5270415d668b21206238403450bb29b5)

# 报错处理

打包的时候：Unity.IL2CPP.Building.BuilderFailedException

> 切到新版本上(2020->2021)

Building Library\Bee\artifacts\Android\d8kzr\libil2cpp.so failed with output:

> 删掉Unity已经弃用的UnityARKitPlugin 的旧插件
