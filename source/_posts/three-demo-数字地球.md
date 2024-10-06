---
title: demo| 数字地球星空
tags:
 - threejs
 - vue
 - react
categories:
 - 3d
comments: true
date: 2024-03-23 12:02:05
photos: https://s2.loli.net/2023/08/29/NgxX5HO2tsJ6GRo.png
cover: https://s2.loli.net/2023/08/29/NgxX5HO2tsJ6GRo.png
---


# 数字地球星空

参考教程：https://juejin.cn/post/7145064095178293285

1. 构建（线框风的地球earth、）环ring、卫星satellite
2. 构建500个随机分布的星星stars
3. 初始化场景，设置光照，关联dom
4. 加载地球的模型和贴图
5. 添加动画



## **页面中不显示canvas的内容**

1. 检查对应的template里的div元素的大小，如果设置不当，就看不到物体；
2. 如果没有把下面的代码放到init里，这个时候container的div还没有创建，获取不到对应的宽高

```js
let container = document.getElementById('container')

let sizes = {
  width: container.clientWidth,
  height: container.clientHeight,
}
```

## **页面中看不到物体**

1. 确认设置了div的大小: 如果 `container`的大小为0或非常小，那么 `Three.js`渲染的内容可能就无法被看到。确保 `.fullSize`样式被正确应用，并且容器元素确实覆盖了可见区域
2. 确定场景的材质颜色不和背景一样
3. 确认材质是否需要加光照
4. 调整相机.lookAt() : 如果场景中没有添加任何可见物体或者物体和摄像机的位置设置不当，可能导致看不到任何内容。试着添加一个简单的几何体，确保它在摄像机视野内
5. 调整物体.position.set()
6. 调整相机的位置.position.set()
7. 调整物体的大小 size
8. **渲染调用时机** ：你在 `initThree`函数中只调用了一次 `renderer.render(scene, camera)`，这意味着场景只渲染了一次，且在没有添加环境纹理之前。通常，我们会在添加完所有初次所需的场景内容后再进行渲染，或者使用动画循环（如 `requestAnimationFrame`）不断渲染场景。

此外，从vue迁移到react里的时候，原先是通过下面来实现的

```
    renderer = new Three.WebGLRenderer({ alpha: true, antialias: true })
    renderer.setSize(sizes.width, sizes.height)
    container.current.appendChild(renderer.domElement);
```

但搬过来就实现不了了，需要使用这样子实现；

```
    renderer = new Three.WebGLRenderer({ canvas: container.current, alpha: true, antialias: true })
    renderer.setSize(sizes.width, sizes.height)
    renderer.render(scene, camera);
```

但又会报这种错误：

```
chunk-A6I3RWFE.js?v=776dc4a7:16638 Uncaught TypeError: Cannot read properties of null (reading 'matrixWorldAutoUpdate')
    at WebGLRenderer.render (chunk-KKQC3OGW.js?v=776dc4a7:17854:17)
    at initThree (EarthStar.jsx:47:14)
    at EarthStar.jsx:120:5
    at commitHookEffectListMount (chunk-A6I3RWFE.js?v=776dc4a7:16883:34)
    at commitPassiveMountOnFiber (chunk-A6I3RWFE.js?v=776dc4a7:18124:19)
    at commitPassiveMountEffects_complete (chunk-A6I3RWFE.js?v=776dc4a7:18097:17)
    at commitPassiveMountEffects_begin (chunk-A6I3RWFE.js?v=776dc4a7:18087:15)
    at commitPassiveMountEffects (chunk-A6I3RWFE.js?v=776dc4a7:18077:11)
    at flushPassiveEffectsImpl (chunk-A6I3RWFE.js?v=776dc4a7:19458:11)
    at flushPassiveEffects (chunk-A6I3RWFE.js?v=776dc4a7:19415:22)
```

需要把`renderer.render(scene, camera);`放到scene初始化的后面；

此外，需要放在scene添加模型之后，这样子模型才会被渲染到场景里；


## **Uncaught TypeError: Cannot read properties of null (reading 'width')**

```js
let sizes = {
  width: container.width,
  height: container.height,
}
```

当你看到错误消息 "Uncaught TypeError: Cannot read properties of null (reading 'width')"，这表明你试图访问的对象（在这个例子中是 `container`）为 `null`。这通常发生在尝试访问一个DOM元素的属性，但该元素在代码执行时还未被成功选中或者根本不存在于DOM中。

改用 `.clientWidth`和 `.clientHeight`是正确的修正方式，但前提是你首先需要确保 `container`不是 `null`。这两个属性是DOM元素的标准属性，分别表示元素的内部宽度（不包括边框）和高度，适用于获取元素的可视尺寸。

你可以通过以下步骤确保 `container`已正确选中并非 `null`：

1. 确保你的代码在DOM元素实际被加载到页面上之后执行。如果你的代码在元素被加载之前执行，那么即使选择器正确，也会返回 `null`。通常，将JavaScript代码放在文档的底部，或者使用 `document.addEventListener('DOMContentLoaded', function() { /* 代码 */ })`来确保在DOM完全加载后再运行代码。
2. 检查选择器是否正确。确保你用来选中 `container`的选择器（比如ID或类名）是正确的，并且与HTML中的元素匹配。

使用 `container.width` 不适用于 `div` 元素，因为 `div` 没有 `width` 属性。`width` 属性通常用于 `canvas`、`img` 等元素，这些元素具有直接可读写的宽度和高度属性。对于 `div` 和其他没有宽高属性的块级元素，你需要使用 `clientWidth` 和 `clientHeight` 来获取它们的视觉呈现尺寸，这些尺寸包括了元素的内边距（padding）但不包括边框（border）和滚动条。

这就是为什么在Three.js中，当你设置相机参数的时候需要使用 `container.clientWidth` 和 `container.clientHeight` 而不是 `container.width` 和 `container.height`：

* `clientWidth` 和 `clientHeight` 给你提供了元素的实际内容区域的尺寸，这对于Three.js中确保相机的长宽比与渲染容器匹配是非常重要的。
* 这些属性反映了 `<div>`容器当前的大小，包括任何由于页面布局或样式动态变化导致的尺寸更改。
* 如果使用 `width` 和 `height`，你可能会得到 `undefined`，因为这不是标准的 `div` 属性，这会导致你的相机设置不正确。

## 动画中位置的设置

在threejs中设置动画里面，satellite模型的位置如下

```js
satellite.position.set(
        140 * Math.cos(radian),
        50 * Math.sin(radian),
        20 * Math.sin(radian)
      )
```

其中

```js
rot += Math.random() * 0.8
let radian = (Math.PI * rot) / 180
```

为什么satellite动画的位置要这么设置

在这段代码中，`satellite.position.set(x, y, z)` 正在设置卫星模型在三维空间中的位置，其中 `x`，`y`，和 `z` 是根据一定规则计算出来的坐标值。这些值是通过正弦（`sin`）和余弦（`cos`）函数的变化来动态计算的，这些函数是从圆和波的几何关系中派生出来的，因此它们非常适合用于创建循环和振荡的动画效果，如卫星绕地球运行的轨迹。

- `140 * Math.cos(radian)` 是在计算卫星在x轴上的位置。使用余弦函数意味着卫星在x轴上的运动会形成一个正弦波，其值在-140到+140之间变化，创建了水平方向上的循环运动。
- `50 * Math.sin(radian)` 是在计算卫星在y轴上的位置。正弦函数决定了卫星在垂直方向上的运动，创建了一个范围在-50到+50之间的波动。
- `20 * Math.sin(radian)` 是在计算卫星在z轴上的位置。这同样使用了正弦函数，但是影响卫星在“深度”方向上的运动。这个值的变化范围在-20到+20之间，这个变化范围小于y轴，意味着在这个方向上的振动幅度较小。
- `rot += Math.random() * 0.8` 这部分代码是为了在每次动画循环时增加 `rot`的值，而且每次增加的值是随机的，最大不超过0.8度。这将为卫星运动添加一个随机性，使其轨迹更加自然，而不是完美的圆形或者规则循环。
- `let radian = (Math.PI * rot) / 180` 这行将角度 `rot`转换为弧度。

为什么x轴使用 `Math.cos` 而y轴和z轴使用 `Math.sin` 呢？这是因为在2D参数化曲线中，常常使用 `cos(t)` 表示x坐标，`sin(t)` 表示y坐标，这样可以创建一个圆。在三维空间中，我们可以使用相同的正弦函数来控制另外两个维度（在这种情况下是y和z），这样我们可以将2D圆形运动扩展到3D空间。

通过这样设置，你可以让卫星在x和y轴上做圆周运动，而z轴的运动则创建了高度的变化，这可以模拟例如倾斜的轨道或者其它不同于标准圆形轨道的路径。通过改变各轴上正弦和余弦函数前的系数，可以创建出不同的椭圆轨道，这就是为什么在不同轴上的运动使用不同的正弦余弦值，以及它们前面的系数不同。

![](https://s2.loli.net/2024/03/25/Ioq7PRSjt3cJ8mG.png)

## 相机

通过设置相机，可以改变物体呈现的角度，也就是在调整物体位置的方案之外，也可以调整相机的位置。

## 外部资源加载

```
EarthStar.jsx:69 THREE.OBJLoader: Unexpected line: "<!DOCTYPE html>"
Promise.then		
(anonymous)	@	EarthStar.jsx:69
Promise.then		
EarthStar	@	EarthStar.jsx:64
Show 20 more frames
```

需要将模型放到public目录里面，同时vite.config.js的配置里加入：`assetsInclude: ['**/*.glb', '**/*.mtl', '**/*.gltf'],`

---

如果你在使用 Vite 和 Vue，并且看到网络请求的状态码为 `304 Not Modified`，这意味着浏览器已经缓存了 `earth.obj` 文件，并且服务器告诉浏览器自从上次请求以来文件没有被修改，所以浏览器可以使用本地缓存的版本。

但是，如果你遇到 `THREE.OBJLoader: Unexpected line: "<!doctype html>"` 的警告，那说明 OBJLoader 实际上解析到的是一个 HTML 文件，而不是 OBJ 文件。这通常说明在请求OBJ文件时发生了错误，服务器返回了一个HTML错误页面而不是文件内容。

由于你看到的是304状态码，而不是200或404，这表明可能存在以下情况：

1. **Vite的静态文件处理** ：Vite在开发模式下可能对静态资源的处理有所不同，确保 `earth.obj`文件放在了 `public`目录下，而不是 `src`目录下。在Vite中，`public`目录下的文件不会被处理，可以直接访问。
2. **缓存问题** ：可能浏览器缓存了错误的内容。尝试清除浏览器缓存，或者在开发者工具中禁用缓存（在Network标签页，勾选Disable cache），然后再次加载页面。
3. **错误页面** ：尽管状态码为304，但是如果服务器在之前的请求中返回了错误，浏览器可能缓存了错误页面。检查Vite服务器的配置或控制台输出以确定是否有错误。
4. **正确的MIME类型** ：Vite应该自动为 `.obj`文件提供正确的MIME类型，但是如果有什么配置不当，可能需要手动检查。

在你的 Vite 项目中，assetsInclude 的配置项用于指定哪些额外的文件类型应被视为静态资源进行处理。默认情况下，Vite 已经内置了对常见的静态资源文件类型的处理，如图片和媒体文件。对于 3D 模型文件如 .glb、.gltf、.obj 和 .mtl，需要特别指定，以便 Vite 能够正确地处理它们。

你提到 .glb 类型的模型可以正常加载，而其他类型的模型需要放到 public 目录下才可以正常加载，这可能与几个因素有关：

* Vite 配置：确保 assetsInclude 的路径匹配是正确的。你的配置已经包括了对这些文件类型的全局匹配。检查是否有任何拼写错误或路径错误。通配符 **/* 表示匹配任意深度的文件夹中的指定文件类型。

* 模块解析：Vite 在开发环境下使用 JavaScript 模块解析来处理资源。如果你的 .obj 或 .mtl 文件没有被正确地识别为模块，它们可能不会通过 Vite 的处理流程。确保这些文件在导入时的路径是正确的，并且相对于项目的根目录。

* 资源引用：检查在项目中引用这些资源的方式。如果你是使用 ES 模块语法（如 import model from './path/to/model.obj'），Vite 将尝试将其视为模块来处理。如果资源没有按预期处理，可能需要调整引用方式或确认文件路径是否正确。

* 缓存问题：有时候，开发服务器的缓存可能导致资源加载行为不一致。尝试清除缓存或重启 Vite 服务器看是否有改进。

* Vite 插件：对于一些特殊类型的文件，可能需要使用特定的 Vite 插件来支持。例如，对于 3D 模型文件，你可能需要使用适合处理这些文件的插件。

最后通过import方式引入，就可以了，不需要放到public目录里了；

但打包的时候，也可能会被忽略（参考你不得以放进去的gltfModels文件夹???

## 球体贴图没贴上

mtl文件里关联了贴图，所以贴图也要一起拷贝过来；
