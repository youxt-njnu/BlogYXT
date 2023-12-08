---
title: threejs API学习|上
tags:
 - WebGL
 - 3DGIS
categories:
 - 3DGIS
comments: true
date: 2023-12-08 11:06:05
photos: https://wallroom.io/img/3840x2160/bg-81f2d39.jpg
cover: https://wallroom.io/img/3840x2160/bg-81f2d39.jpg
---

# 学习资源

[AxesHelper – three.js docs (threejs.org)](https://threejs.org/docs/#api/zh/helpers/AxesHelper)

[1. threejs文件包下载和目录简介 | Three.js中文网 (webgl3d.cn)](http://www.webgl3d.cn/pages/aac9ab/)

[Releases · mrdoob/three.js (github.com)](https://github.com/mrdoob/three.js/releases)

[系统课—面向人群_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV14r4y1G7h4/?p=2&spm_id_from=pageDriver&vd_source=5270415d668b21206238403450bb29b5)

# 记录

特点：THREE的API/函数都是小驼峰；类/属性都是大驼峰

## 安装使用

npm install three@0.158.0 --save

import * as THREE from 'three';

一般来说，你项目用到那个扩展库，就引入那个，用不到就不需要引入。

```javascript
// 引入扩展库OrbitControls.js
import { OrbitControls } from 'three/addons/controls/OrbitControls.js';
// 引入扩展库GLTFLoader.js
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';
```

```javascript
// 扩展库引入——旧版本，比如122, 新版本路径addons替换了examples/jsm
import { OrbitControls } from 'three/examples/jsm/controls/OrbitControls.js';
```

导入的其他方式：type="module", type="importmap"

### vue+threejs环境创建

> npm create vite@latest 构建项目文件夹
>
> npm i 安装所有默认的依赖
>
> npm run dev 查看vue代码的默认渲染效果
>
> 清空默认的代码和结构
>
> npm install three@0.157.0 -S 安装threejs
>
> 新建index.js，在里面写threejs代码
>
> ```
> import * as THREE from 'three'
> const scene=THREE.Scene();
> const geometry=new THREE.BoxGeometry(2,2,2);
> ......
> ```

> 在main.js里引入 `import './index.js'`
>
> 调整优化：
>
> * 全屏(window.innerWidth; window.innerHeight)
> * 去除画布边距：index.css里，body{ margin: 0px; }

一个JS文件里的内容，如果需要被别的JS文件调用，可以用 `export default 对象名/变量/...;`，然后别的JS文件就可以通过 `import model from '这个文件的路径'`，这个时候model就获取到了

## 几个重要API

（场景，几何体，材质，网格，相机，渲染器，光源；网格Mesh/点Points/线Line=几何体+材质，且在场景中有位置；场景中包括了网格、光源；渲染器需要场景和相机

> **Scene**:Scene()
>
> **Geometry**: BoxGeometry(), CylinderGeometry(), SphereGeometry(),ConeGeometry(),PlaneGeometry(),CircleGeometry()--------->默认正面(threejs里设置正面的Mesh三角形是逆时针的顺序)可见，如果需要双面可见，可以设置Material里的side
>
> **Material**: MeshBasicMaterial(), MeshLambertMaterial(), MeshPhongMaterial(), MeshStandardMaterial(), MeshPhysicalMaterial(), PointsMaterial(), LineBasicMaterial(), SpriteMaterial
>
> ```
> new THREE.MeshBasicMaterial {
>     side: THREE.FrontSide // THREE.DoubleSide
> }
> ```
>
> * MeshPhongMaterial: 镜面反射，.color, .shininess, .specular, (这两个是相互一起作用的)
> * MeshLambertMaterial: 漫反射
>
> **Mesh**: Mesh()---------> mesh.position.set(0,10,0); scene.add(mesh);
>
>> Mesh渲染出来的是面，比较常用，点和线用的不多；
>>
>> new THREE.Points()
>>
>> new THREE.Line(), LineLoop(), LineSegments()
>>
>
> **Camera**: PerspectiveCamera(),OrthographicCamera()----------> camera.position.set(200,200,200); camera.lookAt(0,10,0)
>
> **WebGLRenderer**: WebGLRender())--------->renderer.setSize(), render.render(), renderer.domElement,
>
> ```
> 抗锯齿：
> const renderer = new THREE.WebGLRenderer({
>     antialias: true,
> })
> or
> renderer.antialias = true
>
> 输出画布模糊：
> 需要设置设备的像素比
> renderer.setPixelRatio(window.devicePixelRadio)
>
> 设置背景颜色：
> renderer.setClearColor()
> ```
>
> **light**: PointLight(), DirecitonLight(), SpotLight(), AmbientLight(), .intensity, .decay, .position.set()

```
这些其实就是图形学的原理，new mesh的时候传入的就是几何geometry和材质material;
上面的MeshPhongMaterial，就很容易让人联想到冯
```

### 辅助

> AxesHelper(),note: three.js坐标轴颜色红 **R** 、绿 **G** 、蓝**B**分别对应坐标系的 **x** 、 **y** 、**z**轴，对于three.js的3D坐标系默认**y轴朝上**
>
> PointLightHelper(), 点光源辅助观察
>
> DirectionLightHelper()
>
> GridHelper()，辅助网格地面

### 相关

相机控件OrbitControls

> OrbitControl(camera, renderer.domElement)
>
> .addEventListener('change', function() { })
>
> 可以改变相机的位置、拍摄的角度、旋转、距离模型的距离、看到的视野范围
>
> 例如，在修改camera.lootAt()的值的时候，需要一起修改OrbitControls的target位置

clock时钟对象

> clock.getDelta()*1000 // 将秒转换为毫秒
>
> 帧率（每秒钟渲染多少次）：1000/上面的

Canvas画布

> 动态渲染
>
> window.resize=function() { }
>
> camera.updateProjectionMatrix()

Stats.js性能监视器

> Stats()
>
> .update()
>
> .setMode() 设置显示模式

### 动画

> window.requestAnimationFrame() 执行周期性动画，每秒会调用理想下是60次。里面的参数是函数名
>
> ```
> 渲染函数中，每秒旋转，然后渲染，然后执行周期调用------>渲染循环
>
> const renderer=new THREE.WebGLRenderer();
> document.body.appendChild(renderer.domElement);
>
> function render() {
> 	mesh.rotateY(0.01);
> 	renderer.render(camera,mesh);
> 	requestAnimationFrame(render);
> }
> ```

tips:设置了渲染循环，就不需要相机控件中'change'的时候执行renderer.render(scene,camera)了，因为渲染循环一直在隔帧渲染。

## threejs语法总结

new一个类(构造函数);

里面可以初始化的时候就自定义一些属性的数值，而这些属性外界也可以get和set；

通过对象的方法，也可以改变对象的属性。

# js库补充

## gui.js

可以快速控制三维场景的UI交互界面，调试出来threejs里三维空间的很多参数，省的进行心算；

```
// 引入dat.gui.js的一个类GUI
import { GUI } from 'three/addons/libs/lil-gui.module.min.js';
// 就在examples/jsm/lib....里

```

new GUI()

gui.domElement

在范围内调整值：gui.add(ambieny, 'intensity', 0, 2.0) //控制对象，对象的具体属性，其他参数：

> 数字，数字：交互界面为拖动条，可以在区间内改变属性的数值
>
> 数组/对象：交互界面为下拉菜单
>
> 布尔值：交互界面为单选框，而且属性这个时候直接是'bool'，后面的其他参数为空
>
> .addColor(控制对象，对象的具体属性)  颜色值改变，不需要改变范围

.name('可以修改名字')

.step(可以设置每次修改的步长)

.onChange(function(value) {

//可以把gui改变的属性的值value，传入，实现其他对象一起的同步修改

})

此外，gui还可以进行分组，gui.addFolder()，可以方便进行内容组织，同时也支持再次进行嵌套。

打开关闭菜单: .close(), .open()

# 几何体BufferGeometry

BufferGeometry是BoxGeometry, SphereGeometry这些的基类；

BufferAttribute, 为其添加顶点坐标，就可以具象化几何体的形状；

1. 顶点位置：

> Float32Array()
>
> new THREE.BufferAttribute(vertices,3) //三个值代表一个顶点坐标
>
> or + 顶点索引.index //其他基本不变，删除重复的顶点，加上索引相关的代码
>
> Uint16Array([])
>
> new THREE.BufferAttribute(indexes,1) //一个值就代表一个顶点坐标

通过.attributes.position来进行属性的赋予；

得到的几何体，如果是Points()的geometry参数，则渲染为点，如果是Mesh()的geometry参数，则渲染成面。

2. 顶点的法线/法向量：

.attributes.normal

和顶点类似，也是通过new THREE.BufferAttribute(normals,3)这种方式进行赋予的。

3. 顶点的UV数据

.attributes.uv

6. 顶点的颜色数据

.attributes.color

和position类似的用法；

但在材质里面需要设置vertexColors: true, 这样才会表示要使用顶点颜色进行渲染

材质并不局限于点的，线、网格、曲线的都可以，顶点的颜色设置完了之后得到的模型，就是渐变色的

9. 其他继承BufferGeometry的类，如PlaneGeometry, SphereGeometry....

也都有BUfferGeometry的属性和方法，可以查看属性和方法如.attributes.position, .index(就像矩形，一共六个矩形面、12个三角形、36个顶点，也就是36个index的count为36)

通过线框模式，可以查看对应的三角形结构，设置属性wireframe为true

4. 几何体细分数

新建几何体的时候，传入的参数：宽，高，宽细分数，高细分数

5. 几何变换

.scale(), .translate(), .rotateX(), .rotateY(), .rotateZ(), .center()实现居中

# 模型

模型、材质、vector3这些，都有.clone(), .copy(), 克隆的是一个新的对象，拷贝的是元对象的属性值然后给新的对象 `const v2=v1.clone(); v3.copy(v1);` v1的属性拷贝给v3

模型.clone(): 和原来的模型共享材质和几何体

材质/几何体.clone(): 不和原来的材质和几何体共享

同理对于mesh以及mesh的属性

mesh.clone()

mesh.position.clone()

## Object3D

Points, Line, Mesh的基类

.position, .scale

.translateX(), .translateY(), .translateZ(), .translateOnAxis()

.rotation, .quaternion

> new THREE.Euler()
>
> .rotationX(), .rotationY(), .rotationZ(), .rotateOnAxis()
>
> rotationX是在上一次的基础上旋转；
>
> .position.x是在原始0的基础上调整，如果要实现类似上面的基于上一次的基础，需要+=

Vector3

> .x, .y, .z, .set(),

## 材质

material是所有材质的基类。

.color颜色

> new THREE.Color()
>
> .r, .g, .b
>
> .setRGB(), .setHex(), .setStyle(), .set()
>
> 可以参数里直接16进制，也可以'#16进制代码', 'rgb(0,255,255)'，这两种是前端里设置颜色较常使用的。
>
> .transparent, .opacity, .side,
>
> 插值, .lerp(), .lerpColors(), 之后通过.r, .g, .b可以获取，之后再BufferAttribute()
>
> .clone()

渲染山脉数据应用案例

> 获取gltf模型里的所有y坐标，并完成排序；
>
> 逐个y赋予颜色渐变坐标；
>
> 为几何体赋予顶点颜色；
>
> 构建模型（几何体，材质）

## 查看模型的信息

const mesh=new THREE.Mesh(geometry, material);

mesh.geometry

mesh.material

mesh.geometry.translate()

mesh.material.color.set()

note: 模型如果初始化于同一个材质或几何体，那这个材质或几何体在模型之间会共享，一个改变了另一个也会改变

> const mesh1=new THREE.Mesh(geometry, material)
>
> const mesh2=new THREE.Mesh(geometry, material)
>
> mesh1.material.color.set(0xffff00) //那mesh2的材质颜色也会修改
