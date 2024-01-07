---
title: threejs API学习 Ⅱ
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

# Group层级模型

group、mesh这些都继承自Object3D，所以其实都可以实现Group的作用，就是进行.add()

```
const group=new THREE.Group();
group.add(mesh1);
group.add(mesh2);
// group.add(mesh1,mesh2);
scene.add(group);

group.children
scene.children
```

就类似于unity里建父gameobject、拖进一个gameobject里形成子gameobject，这种的形式。同理可以理解下面的：

group.name，设置了name后，就可以通过.getObjectByName()来获取到这个名字下的物体，更多的方法可以查看基类Object3D

group.traverse()可以实现递归遍历group的所有子物体，内部还可以添加匿名函数。

obj.isMesh 类似于obj.type === 'Mesh'

父对象的位置改变也会影响子对象，所以子对象的本地坐标/局部坐标就是它的position，而子对象的世界坐标就是考虑所有他的父物体下的position；

a.getWorldPosition(b), 读取a的师姐坐标，存到b里面去；

可视化局部坐标系，可以给a添加一个axesHelper：`a.add(new THREE.AxesHelper(表示大小的参数))

geometry的中心位置变化，但mesh没变，旋转的时候还是照着mesh的坐标原点来旋转；

如果想从父物体里面移除子物体，可以使用.remove()，和.add()作用相反，但用法一样。

可见性的设置：

> Object3D基类有属性.visible，通过设置这个的bool值，实现Object3D所有子类的实例的显示隐藏；
>
> 如果一个子类的实例是多个模型共享的（例如多个mesh共享一个material），这个时候设置一个的visible也会作用在其他模型上。

# 贴图

texloader = new THREE.TextureLoader() --->texture = texloader.load('.earth.jpg') ---> new THREE.MeshLambertMaterial({ map: texture, }) ---or--- material.map = texture

颜色纹理贴图map：注意，map属性和color属性不需要都设置，因为两者都是获取RGB赋予到material上，如果都设置会将两者进行混合；

## UV坐标

颜色纹理贴图在不同的几何体上的渲染效果不同，与UV坐标有关；

const uvs=new Float32Array([]) //定义范围

geometry.attributes.uv = new THREE.BufferAttribute(uvs,2) // 可以理解为几何体对应的顶点坐标下，对应到uv的的哪个位置；

针对圆形平面，其UV坐标默认提取的就是一个圆形轮廓，UV坐标会对颜色纹理贴图的.map进行提取；

如何实现地面瓷砖效果：

> Texture有阵列功能，+ 矩形平面几何体
>
> 对texture设置阵列模式：
>
> ```
> texture.wrapS=THREE.RepeatWrapping; //设置阵列模式，水平，U
> texture.wrapT=THREE.RepeatWrapping; //设置阵列模式，垂直，V
> texture.repeat.set(12,12); //设置uv两个方向的纹理重复数量，传入的就是二维向量
> ```

如何设置透明贴图：

> material的transparent属性设置为true，geometry为矩形平面

UV动画实现：

> texture的偏移.offset, .x, .y，通过设置这个实现贴图UV顶点坐标的偏移；
>
> 将offset和wrapS/wrapT结合，可以把偏移出去的内容接到原来的上面去；
>
> 利用requestAnimationFrame来实现隔帧的偏移和拼接，最后呈现出动画的效果
>
> 再和阵列结合，对原始图像进行重复纹理，这样能进一步减少贴图的像素。但这个时候设置的offset其实是原始贴图的大小的比例，而不是阵列下的大的贴图的比例，所以一般相对上面的需要调大。

## PBR材质

不同的材质，使用的光照模型不同，材质模拟Mesh反射光照的代码算法不同

整体上来看，渲染表现能力越强，占用的计算机硬件资源更多。

MeshStandardMaterial和MeshPhysicalMaterial都是PBR材质；

属性：

* .metalness
* .roughness
* .envMap
* .envMapIntensity

环境贴图：

* textureCuve=new THREE.CubeTextureLoader，一次性加载六张图片，返回一个立方体纹理对象CubeTexture，父类是Texture
* 如果希望环境贴图能影响场景scene中所有Mesh，可以直接设置scene.environment=textureCube; 也可以对场景中所有OBJ进行遍历，设置material的envmap

此外，注意设置纹理和渲染器颜色空间要保持一致；

MeshPhysicalMaterial在继承上面的同时，还进行了进一步扩展：

> .clearcoat清漆层，.clearcoatRoughness
>
> .transmission透光率
>
> .reflectivity反射率
>
> .sheen光泽
>
> .ior折射率，非金属材料的折射率从1.0到2.333。默认值为1.5。不同材质的折射率，你可以百度搜索

一般美术那边导出的时候，gltf模型会带有相关属性；在threejs解析的时候，会先判断是不是MeshStandardMaterial，然后再判断是不是MeshPhysicalMaterial，基本的属性都会自动解析；最后程序员只需要设置环境贴图和环境贴图强度属性即可。

# 外部模型

常见建模语言：Blender, 3dmax, maya, C4D

3D数据标准：

> * [Universal Scene Description, USD](https://zhuanlan.zhihu.com/p/439681342)：处理制作数字图形电影和游戏，支持对任意数量的资产进行组装并组织到场景、快照或世界中
> * [glTF](https://zhuanlan.zhihu.com/p/443862875): 以3D场景为根节点组织数据，对场景结构进行描述，会定义网格、材质、相机、动画、皮肤、纹理等节点。
> * [3D Tiles](https://zhuanlan.zhihu.com/p/447983314): 基于glTF和大数据量网络传输的解决方案，针对三维地理空间数据如三维建筑、BIM/CAD、点云进行流处理和渲染而开发的数据格式，基于传输可渲染的层级数据结构和瓦片格式集

模型格式：

> gltf: .glb/.gltf，类似JSON格式内容，通过键值对表达模型的信息；.bin文件是对其中buffers里一些顶点数据等的存储；---> .gltf; .gltf+.bin+贴图; .glb
>
> obj:

模型的命名（程序和美术的协作）：

> 命名可以英文、拼音、中文这种形式，中文有时候会乱码；
>
> 通过打印gltf.scene可以查看gltf的模型目录树；
>
> gltf.scene.traverse(function(obj) { if(obj.isMesh) { console.log(obj.material); } })
>
> .children, .name
>
> const obj = gltf.scene.getObjectByName()
>
> obj.children.forEach(function(mesh) { mesh.material.color.set(0xffff00); })

材质共享问题

> 通过name来标记材质，进行mesh的判断；obj1.material.name='building material'; console.log(obj2.material.name);
>
> 1. 三维建模软件中设置，需要代码改变材质的Mesh不要共享材质，要独享材质。
> 2. 代码批量更改：克隆材质对象，重新赋值给mesh的材质属性
>
> ```
> if(obj.isMesh) {
> 	ojb.material=obj.material.clone();
> }
> ```

## 加载

GLTFLoader.js扩展库

```
const loader=new GLTFLoader();
loader.load('tmp.gltf',function(gltf) {
	console.log('gltf对象场景属性',gltf.scene);
	scene.add(gltf.scene);
})
```

尺寸和单位

> 开发的时候一般不需要具体的值，在blender里可以打开gltf模型来测量尺寸；
>
> threejs里没有单位，只有数字大小，obj和gltf格式的模型只有尺寸也不包含单位信息；
>
> 在实习项目开发的时候，一般甲方、前端和美术之间需要协调来统一一个单位。
>
> 此外需要设置合适的相机参数。

纹理贴图颜色偏差：

> 修改webgl渲染器默认的编码方式,renderer.outputColorSpace = THREE.SRGBColorSpace; //文档里面搜索GLTFLoader
>
> 需要保证颜色贴图.colorSpace和渲染器.outputColorSpace保持一致
>
> obj.material.map.colorSpace

纹理贴图错位：

> texture.flipY
>
> obj.material.map.flipY
>
> gltf的flipY默认为false，纹理对象texture默认为true，需要这两个保持一致

加载流程：

> 导入扩展库；
>
> 加载模型；
>
> blender内进行基本大小的测量；
>
> 根据目的思考把相机放在哪里，设置camera.position
>
> 把哪个位置放在中间那就设置那个位置为camera.lookAt(), 此外要注意OrbitControls的影响
>
> 此外，相机的近裁截面要尽量小，远裁截面要尽量大。
>
> 可以使用OrbitControls来辅助设置相机的参数，就可以在界面上旋转、平移、缩放，复制camera.position, controls.target的数值，在代码里设置就行

层级模型遍历

> .traverse()
>
> ```
> gltf.scene.traverse( function(obj) {})
> ```

gltf.scene.children[0].geometry.index获取几何体顶点的索引属性

gltf.scene.children[0].geometry.attributes.position.count获取几何体顶点的数量, .getX(), .getY(), .getZ()获取几何体特定顶点的坐标，.setX(), .setY(), .setZ()

# 渲染器和前端UI

## Canvas

threejs基于webgl进行的封装，所以也要依赖Canvas，将三维场景渲染到Canvas画布上；

根据window窗口设置Canvas的宽高；当窗口大小变化的时候，设置Canvas随之变化window.onresize=function() { 更新宽高；更新渲染器大小和相机视野；}

Canvas叠加HTML元素可能遇到的问题：

> 画布被挤下来：设置画布的绝对定位 `renderer.domElement.style., .position, .top, .left`
>
> z-index控制谁在上谁在下：默认的定位是position: index,此时设置z值无效，需要p调整为relative；
>
> ```js
> renderer.domElement.style.zIndex=-1;
> ```
>
> ```html
> <div style="color: #ff0000; z-index:2; position: relative;"> red </div>
> ```

> 或者也可以设置div元素绝对定位，通过设置插入的div元素绝对定位，这样也可以把div元素叠加到canvas画布上
>
> ```html
> <div style="color: #ff0000; z-index:2; position: absolute;"> red </div>
> ```

UI交互按钮和场景交互，例如点击button实现mesh的material的切换；

如果要将threejs背景设为透明，

> 1 `renderer.setClearAlpha(0.8);`
>
> 2 `renderer.alpha=true;`
>
> 3 `renderer.setClearColor(0f0000,0.4);` 第二个参数表示透明度

如果要将threejs渲染结果保存为图片，可以通过超链接元素a来下载文件

> 1 添加下载按钮，点击实现保存文件saveFile()，里面的实现逻辑就是触发超链接a
>
> 2 配置webgl渲染器，renderer.preserveDrawingBuffer=true;
>
> 3 Canvas的.toDataURL()，const canvas=renderer.domElement, 可以设置超链接的保存数据link.href=canvas.toDataURL("image/png"), or jpeg, or bmp.....

**模型闪烁/深度冲突/z-fighting**

当模型有面重合的时候，电脑GPU不知道哪个在前哪个在后，所以会发生深度冲突。

可以调整模型之间的距离，但也不能间隙特别小，因为电脑GPU精度有限；mesh.position.z=1; 在建模的时候也需要注意，尽量避免两个Mesh完全重合，可以考虑适当偏移一定距离；

此外，透视投影相机也会产生深度冲突问题：当相机的位置特别远，而两个mesh之间的距离又挺小的情况下，就还是会发生深度冲突问题。

> 设置对数深度缓冲区，来进行优化；
>
> renderer.logarithmicDepthBuffer=true
>
> 不过这个的作用也是有限的，如果间距还是过小，那还是会发生这个问题。

**显示模型的加载进度**

> loader.load("模型路径",加载完成后执行的函数，加载过程中执行的函数)

# 生成曲线、几何体

分段连线，构成圆弧；

构建BufferGeometry()的时候，除了用顶点数据传给geometry.attributes.position外，还可以用geometry.setFromPoints()方法，可以把THREE.Vector3或THREE.Vector2的数组传入进去；

## 曲线

threejs的API：

> 2D: LineCurve, ArcCurve, EllipseCurve, SplineCurve, QuadraticBezierCurve, CubicBezierCurve
>
> 3D: LineCurve3, CatmullRomCurve3, QuadraticBezierCurve3, CubicBezierCurve3

属性：

> .getPoints()
>
> .setFromPoints()
>
> .getSpacedPoints()

组合：

> new THREE.CurvePath().curves.push()
>
> 要注意push进去的curve的顺序

应用：

* 样条函数、贝塞尔曲线：表示飞行曲线轨迹

基于曲线的其他几何体

> THREE.TubeGeometry() 第一个参数就是上面的曲线
>
> THREE.LatheGeometry() 第一个参数可以就是上面的曲线的.getPoints()
>
> THREE.Shape() 传入点数组
>
> THREE.ShapeGeometry() 传入shape
>
> THREE.ExtrudeGeometry() 传入shape,传入上面的曲线的话就能扫描成型

Path：Shape的父类

> 相关绘制API：.lineTo(), .arc(), .absarc(), .splineThru(), .bezierCurveTo(), .quadraticCurveTo()
>
> 相关属性：.currentPoint, .moveTo(), .lineTo(), .holes.push()

边线几何体EdgesGeometry

> THREE.EdgesGeometry()，传入的是Geometry
>
> THREE.LineSegments()，传入EdgeGeometry和THREE.LineBasicMaterial类似的material
>
> 应用：针对gltf模型，可以显示里面的geometry对应的边线几何体
