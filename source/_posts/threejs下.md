---
title: threejs API学习 Ⅲ
tags:
 - WebGL
 - 3DGIS
categories:
 - 3DGIS
comments: true
date: 2024-01-07 15:56:05
photos: https://wallroom.io/img/3840x2160/bg-81f2d39.jpg
cover: https://wallroom.io/img/3840x2160/bg-81f2d39.jpg
---
# 学习资源

[AxesHelper – three.js docs (threejs.org)](https://threejs.org/docs/#api/zh/helpers/AxesHelper)

[1. threejs文件包下载和目录简介 | Three.js中文网 (webgl3d.cn)](http://www.webgl3d.cn/pages/aac9ab/)

[Releases · mrdoob/three.js (github.com)](https://github.com/mrdoob/three.js/releases)

[系统课—面向人群_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV14r4y1G7h4/?p=2&spm_id_from=pageDriver&vd_source=5270415d668b21206238403450bb29b5)

# 相机

基类：Camera

子类：OrthographicCamera, PerspectiveCamera, CubeCamera

基类属性：.matrixWorldInverse, .projectionMatrix.

子类：除了初始化方式不同，其他都可以参考Camera/PerpectiveCamera

Camera的position和far的设置不当，会看不到、看不清、找不到物体；

OrthographicCamera:

> (left, right, top, bottom, near, far)
>
> 在一些不需要透视的场景（如看一张中国地图、2D可视化）的时候，可以使用
>
> 设置此时的Canvas随窗口变化
>
> ```
> window.onresize=function() {
> 	获取window的宽高
> 	renderer.setSize(width, height);
> 	更新相机参数k, camera.left, camera.right
> 	camera.updateProjectionMatrix();
> }
> ```

包围盒

> THREE.Box3()
>
> box3.min
>
> box3.max
>
> 获取对象的最小包围盒子 box3.expandByObject(mesh)
>
> box3.getSize(), box3.getCenter(), 里面直接传入接收结果的vector3坐标

渲染简单地图

> 经纬度坐标转Vector2，存入数组，转为shape，转为ShapeGeometry，形成一个多边形平面几何体
>
> 使用正视相机进行渲染
>
> 利用Box3计算模型的中心位置和尺寸
>
> 优化Camera里的position.set(), .lookAt() ,实现地图在中间，地图基本填充满画布，地图平行于Canvas
>
> 如果设置了OrbitControls，记得也进行更新

相机动画

> 设置循环，循环中改变相机的位置和朝向，并更新渲染

UI交互实现不同方向的投影视图

> 给UI设置.addEventListener('click', function() { 设置camera的position和lookAt })

相机旋转渲染

> camera.up.set(), 修改相机的上方向
>
> 之后再重新执行下.lookAt()

相机在管道内漫游

> 构建曲线，构建管道；
>
> 加载贴图，完善管道外观；
>
> 管道上取点，设置相机位置和lookAt的点

# 灯光

聚光灯SpotLight

> .intensity, .angle, .decay, .position.set
>
> THREE.SpotLightHelper()

阴影

> 点光源、平行光、聚光灯都可以产生阴影，但环境光AmbientLight没有方向，不会产生阴影
>
> 平行光阴影计算设置：
>
> * 设置产生阴影的模型和光源：mesh.castShadow=true; directionLight.castShadow=true;
> * 设置接收阴影的模型：planeMesh.receiveShadow=true;
> * 设置渲染器的阴影贴图：renderer.shadowMap.enabled=true;
> * 平行光阴影的相机的渲染范围设置：directionLight.shaodw.camera
>
>   * 可以通过cameraHelper进行可视化，就传入上面这个，然后添加到scene里
>   * 先根据模型的数量级，确定阴影计算范围的数量级
>   * .shadow.camera的属性和OrthographicCamera正投影相机一样
>   * directionLight.shadow.camera.left, .right, .top, .bottom, .near, .far, .updateProjectionMatrix() 通过这些来进行渲染范围的设置，设置不当可能会出现看不到阴影或阴影不完整的现象
>   * 此外，调整光源的位置directionLight.position.set，也可以把渲染范围调的好一些（.shadow.camera.position=directionLight.position）
> * 平行光阴影的贴图尺寸 light.shadow.mapSize.set()，提升边缘渲染效果，设置数值一般$2^n$
>
>   > `.shadow.mapSize`和 `.shadow.camera`总结
>   >
>   > * 在能覆盖包含阴影渲染范围的情况下，`.shadow.camera`的尺寸尽量小。
>   > * 如果你增加 `.shadow.camera`长方体尺寸范围，阴影模糊锯齿感，可以适当提升 `.shadow.mapSize`的大小
>   >
> * 平行光阴影的半径 light.shadow.radius，弱化模糊阴影的边缘

案例——工厂、园区等3D场景中的光照设置

> 环境贴图：obj.material.envMap, obj.material.envMapIntensity
>
> 环境光：ambientLight.intensity
>
> 平行光模拟的太阳光：DirectionLight, DirectionLightHelper
>
> gui进行交互微调，其中更新完属性（如.left这些）后，可以直接.upateProjectionMatrix()，对于Helper对象需要进行更新.update()，实现姿态的同步变化
>
> 太阳光阴影：注意所有的物体都有产生阴影和接受阴影，需要对gltf里的obj进行遍历设置castShadow和receiveShadow
>
> 如果模型表面产生了条纹影响渲染效果，可以设置renderer.shadowMap.type=THREE.VSMShadowMap;

# 精灵模型Sprite

继承自父类Object3D，和Points, Line, Mesh处于一个级别的地位；

SpriteMaterial继承自父类Material，和其他Material如MeshBasicmaterial处于一个级别的地位；

new THREE.Sprite(spriteMaterial) 不需要传入Geometry, 默认是矩形形状，长宽为1，在坐标原点的位置。

Sprite在旋转3D场景的时候，始终会平行于Canvas画布/屏幕，而矩形平面PlaneGeometry不一定平行于Canvas.

Sprite: .position, .scale,

SpriteMaterial: .rotation, .color, .map, .transparent, .opacity

应用：

* Sprite+贴图来标注三维场景，相对矩形Mesh+贴图的标注而言始终平行于Canvas画布
* Sprite模拟下雨、下雪，可以减少三角形定点数量，提高threejs渲染性能
  * 随即创建多个sprite，并设置随机位置
  * 改变sprite的y的位置，实现下落（可以周期性调整、或者根据时间进行调整）
  * 注意调整相机的near,避免出现near太近，然后距离近的雨滴太大了

# 后处理

后处理通道扩展库：

> OutlinePass.js 高亮发光描边
>
> UnrealBloomPass.js Bloom发光
>
> GlitchPass.js 画面抖动效果

案例：

> 导入: 后处理扩展库(EffectComposer.js)，渲染器通道(RenderPass.js), OutlinePass.js...
>
> const composer = new EffectComposer(renderer) 指定需要后处理的渲染器
>
> const renderPass = new RenderPass(scene, camera) 只当后处理对应的场景和相机
>
> const outlinePass=new OutlinePass(画布大小Vector2, scene, camear)
>
> outlinePass.selectObjects=[], .visibleEdgeColor.set(), .edgeThickness, .edgeStrength, .pulsePeriod
>
> const bloomPass=new UnrealBloomPass(画布大小Vector2);
>
> .strength
>
> const glitchPass = new GlitchPass();
>
> composer.addPass(renderPass);
>
> composer.addPass(outlinePass);
>
> composer.addPass(.......)
>
> composer.render(); // 不需要再renderer.render(scene, camera);

针对gltf后处理之后颜色发生异常的处理：

> renderer.outputColorspace=THREE.sRGBColorspace会失效
>
> 采用GammaCorrectionShader进行伽马校正
>
>> import GammaCorrectionShader, ShaderPass
>>
>> composer.addPass(new ShaderPass(GammaCorrectionShader))
>>

抗锯齿FXAAPass, SMAAPass

> import
>
> renderer.setPixelRatio()
>
> renderer.getPixelRatio()
>
> FXAAPass.uniforms.resolution.value.x, .y

# 射线

射线ray

> const ray = new THREE.Ray()
>
> ray.origin, .direction(需单位向量，不然需要.normalize()),
>
> .intersectTriangle()

射线投射器raycaster

> const raycaster=new Raycaster()
>
> raycaster.ray, .ray.origin, .ray.diection,
>
> const intersects = raycaster.intersectObjects([]) 选中几个返回几个，返回的对象按照先后顺序排序
>
> intersects[i].point, .object, .distance

## 屏幕坐标系

网页坐标系clientX, clientY

鼠标涉及元素坐标系offsetX, offsetY

threejs画布的标准坐标系

## 鼠标点击选中

点击处坐标转化；➡️ 此时要注意Canvas的大小尺寸会发生变化，参数设置里要更新Canvas的时候也更新相机也更新鼠标点击点

射线计算：raycaster.setFromCamera(new THREE.Vector2(x,y), camera);

射线交叉计算：raycaster.intersectObjects();

## 拾取层级模型

点击到的可能是一个层级模型下的子mesh，如果要给这个层级模型添加例如选中描边这种的效果，就需要遍历所有的子物体，给子物体添加一个属性指向这个层级模型

> group.traverse(function(obj) { if (obj.isMesh) { obj.ancestors=group;}});
>
> if(intersects.length>0) { outlinePass.selectedObjects=[intersects[0].object.ancestors];}

补充：除了Mesh，也可以识别选中Sprite
