---
title: demo| 光影人物
tags:
 - threejs
 - vue
 - react
categories:
 - 3d
comments: true
date: 2024-04-02 10:02:05
photos: https://s2.loli.net/2023/08/29/NgxX5HO2tsJ6GRo.png
cover: https://s2.loli.net/2023/08/29/NgxX5HO2tsJ6GRo.png
---


# 光影人物

参考教程：https://juejin.cn/post/7145064095178293285

1. 两个渲染器、相机，一个场景，添加光照；
2. 处理窗口缩放
3. 构建页面结构
4. css写在外部文件里
5. 构建加载管理器，实现加载页面的下移、消失、移除
6. 加载glb模型，并使用draco进行处理
7. 使用intersection observer进行异步观察
8. 处理鼠标移动下的span效果
9. 调整相机
10. 添加事件监听

注意点：
* renderer1.outputColorSpace = Three.SRGBColorSpace // API已经改成了ColorSpace

* why lds-roller has 8 divs? ,用于形成loading旋转的时候的八个点

## IntersectionObserver

`IntersectionObserver` 是一个强大的 Web API，用于异步观察一个元素与其祖先元素或顶级文档视窗（viewport）的交叉状态。简单来说，它允许你配置一个回调函数，当被观察的元素以某种方式进入或离开另一个元素的视域时，这个回调函数会被执行。这个功能特别适合于实现像懒加载图片、无限滚动、动画触发等功能，而无需依赖繁重的滚动事件监听，从而提高页面性能和用户体验。

在你的代码示例中，`IntersectionObserver` 被用于监测一个名为 `.second` 的元素何时成为可见的，以及它的可见程度（即交叉比率）：

```javascript
let secondContainer = false;
const ob = new IntersectionObserver(payload => {
  secondContainer = payload[0].intersectionRatio > 0.05;
}, { threshold: 0.05 });
ob.observe(document.querySelector('.second'));
```

- **回调函数**：当观察的元素进入或退出交叉区域时，回调函数会被调用。`payload` 参数（通常命名为 `entries`）是一个数组，包含了被观察元素的交叉状态信息。这个示例中只观察了一个元素，因此通过 `payload[0]` 访问了这个元素的信息。
- **`intersectionRatio`**：这是一个 0 到 1 之间的值，表示观察的元素当前可见部分的比例。在这个例子中，如果这个比例大于 0.05，`secondContainer` 将被设置为 `true`，表示元素至少有 5% 是可见的。
- **选项对象**：在这个例子中，选项对象 `{ threshold: 0.05 }` 指定了触发回调的交叉比率阈值。当元素的可见部分超过 5% 时，回调函数将被执行。`threshold` 可以是一个单一的值或一个值的数组，允许你在元素可见性达到多个不同级别时触发回调。
- **`.observe()` 方法**：这个方法用于开始观察一个元素。在这里，它观察一个类名为 `.second` 的元素。当这个元素的可视状态发生变化，且满足设定的阈值时，将触发上面定义的回调函数。

使用 `IntersectionObserver` 相比传统的滚动事件监听，可以大大减少对性能的影响，因为浏览器可以优化对交叉状态的检测，无需在每次滚动事件发生时执行复杂的计算。

## `window.scroll`

`window.scroll(0, 0)` 是 JavaScript 中的一个方法调用，用于将浏览器窗口或标签页的滚动位置设置到页面的最顶部。这里的两个参数 `(0, 0)` 分别表示水平和垂直方向的滚动位置。

- 第一个参数 `0` 表示在水平方向上的滚动距离。在这个例子中，`0` 意味着页面将滚动到最左边。
- 第二个参数 `0` 表示在垂直方向上的滚动距离。在这个例子中，`0` 意味着页面将滚动到最顶部。

因此，调用 `window.scroll(0, 0)` 会使得页面滚动到左上角，即页面的起始位置。这个方法经常在需要将用户的视图重置到页面顶部的情况下使用，比如在用户点击“返回顶部”按钮时。

## 动画位移

```
// 更新点光源的位置
      fillLight.position.y -=
        (parallaxY * 9 + fillLight.position.y - 2) * deltaTime
      fillLight.position.x +=
        (parallaxX * 8 - fillLight.position.x) * 2 * deltaTime
      // 更新第一个相机所在相机组的位置
      cameraGroup.position.z -=
        (parallaxY / 3 + cameraGroup.position.z) * 2 * deltaTime
      cameraGroup.position.x +=
        (parallaxX / 3 - cameraGroup.position.x) * 2 * deltaTime
```

这段代码通过结合用户输入（例如鼠标移动产生的视差效果，即 `parallaxX` 和 `parallaxY`）和时间变化量（`deltaTime`）来动态更新点光源和相机组的位置。这种方法可以创建平滑和响应式的动画效果，使得场景元素（如光源和相机）能够根据用户的交互或时间的推移进行移动。

这里使用的是一种常见的动画和模拟技术，即在每一帧根据时间差（`deltaTime`）调整位置，以实现平滑的运动效果。`deltaTime` 通常表示自上一帧以来经过的时间，用于确保动画的速度不受帧率变化的影响。

```javascript
fillLight.position.y -= (parallaxY * 9 + fillLight.position.y - 2) * deltaTime;
fillLight.position.x += (parallaxX * 8 - fillLight.position.x) * 2 * deltaTime;
```

- 对于 `y` 轴位置，代码计算了一个目标值，这个目标值由 `parallaxY` 的9倍与光源当前 `y` 值与2的差值共同决定，然后通过 `deltaTime` 调整速度和平滑度。这样做的效果是让光源在垂直方向上根据视差值平滑移动，并且试图维持在一个**相对于原点偏移2**的位置。
- 对于 `x` 轴位置，类似地，目标位置由 `parallaxX` 的8倍减去当前 `x` 值决定，然后乘以2和 `deltaTime` 来调整。这让光源在水平方向上也根据视差值平滑地移动。

```javascript
cameraGroup.position.z -= (parallaxY / 3 + cameraGroup.position.z) * 2 * deltaTime;
cameraGroup.position.x += (parallaxX / 3 - cameraGroup.position.x) * 2 * deltaTime;
```

- 这里对相机组的 `z` 和 `x` 轴位置进行更新，逻辑类似于光源位置的更新，**但视差值被除以3，意味着相对移动幅度更小**，创建了一种不同的视差效果。这种效果通常用来给用户一种深度感，即背景元素相对于前景元素移动得更慢。

？如何恰当的设置，在鼠标移动下，页面元素的流畅偏移效果

## CSS

```
.lds-roller div {
  animation: lds-roller 1.2s cubic-bezier(0.5, 0, 0.5, 1) infinite;
  transform-origin: 40px 40px;
} 
```

这段 CSS 代码定义了一个动画效果，通常用于创建加载指示器（如旋转的圆环或球）。让我们逐步解析这段代码的含义：

```css
.lds-roller div {
  ...
}
```

这里的选择器 `.lds-roller div` 指的是所有类名为 `lds-roller` 的元素内部的 `div` 元素。对这些 `div` 元素应用以下样式和动画。

```css
animation: lds-roller 1.2s cubic-bezier(0.5, 0, 0.5, 1) infinite;
```

- **动画名称 (`lds-roller`)**：这是动画的关键帧名称，它引用了在某处定义的 `@keyframes lds-roller`，该关键帧定义了动画的开始、结束状态以及可能的中间状态。
- **持续时间 (`1.2s`)**：动画从开始到结束的时长为 1.2 秒。
- **缓动函数 (`cubic-bezier(0.5, 0, 0.5, 1)`)**：这是一个贝塞尔曲线，定义了动画的加速度曲线。这个特定的曲线意味着动画开始时加速度较快，结束时减速。这样可以让动画看起来更自然、流畅。
- **重复次数 (`infinite`)**：动画会无限次重复。

```css
transform-origin: 40px 40px;
```

- **变换原点 (`transform-origin`)**：这个属性定义了元素变形的原点。在这个例子中，原点被设置在了元素的 `40px 40px` 的位置，通常意味着元素的变换（如旋转）将围绕这个点进行。这个设置有助于创建环绕中心点旋转的效果，特别是当动画效果是旋转时。

综上所述，这段 CSS 为 `.lds-roller` 内部的每个 `div` 应用了一个持续 1.2 秒、无限重复的平滑动画，该动画的具体行为由 `@keyframes lds-roller` 定义。动画的变换原点设置在每个 `div` 的 `40px 40px` 处，使得旋转动画能围绕该点进行，创建出一个加载动画的视觉效果。

为了完整地实现这个动画效果，你还需要定义相应的 `@keyframes lds-roller` 关键帧动画，指明动画开始、结束时，以及可能的中间步骤的具体样式变化。

```
loadingCover.style.setProperty(
              'transform',
              `translate(0,${yPosition.y}%)`
            )
```

在这段代码中，`loadingCover.style.setProperty` 方法被用于动态修改 `loadingCover` 元素的 CSS `transform` 属性。这个方法允许你直接通过 JavaScript 更改元素的样式，其中 `setProperty` 接受两个参数：第一个参数是要修改的 CSS 属性名称，第二个参数是该属性的新值。

- **第一个参数 (`transform`)**: 指定了要修改的 CSS 属性，这里是 `transform`。`transform` 属性允许你对元素进行变形，比如旋转、缩放、移动（平移）或倾斜。
- **第二个参数 (`translate(0,${yPosition.y}%)`)**: 是 `transform` 属性的新值，这里使用了 `translate` 函数来移动元素。`translate` 函数接受两个参数，分别对应 X 轴和 Y 轴的移动距离。在这个例子中，`translate(0,${yPosition.y}%)` 表示在 X 轴方向上不移动（`0`），在 Y 轴方向上移动 `${yPosition.y}%` 的距离，其中 `${yPosition.y}` 是一个动态计算的值，表示移动距离的百分比。使用百分比单位可以根据元素大小的不同而相对地移动不同的距离。

这段代码的目的是根据 `yPosition.y` 的值（可能是通过某些计算得到的），在 Y 轴方向上动态移动 `loadingCover` 元素。这种技术常用于创建动态的交互效果，比如根据用户的滚动或鼠标移动来移动页面上的元素，从而增加页面的动态性和互动性。

假设 `yPosition.y` 的值为 `50`，则 `translate(0,${yPosition.y}%)` 将解析为 `translate(0,50%)`，意味着 `loadingCover` 元素将沿 Y 轴方向下移其高度的 50%。这样的移动效果可以用于各种动画和过渡效果，使得页面元素的移动看起来更平滑和自然。

## innerHeight, clientHeight

当想获取一个DOM元素的宽度和高度时，通常会使用 `clientHeight`和 `clientWidth`。这两个属性提供了元素的内部高度和宽度（包括填充，但不包括边框、滚动条或外边距）。

对于你的具体例子，如果你需要在组件挂载后获取这个 `div`元素的宽度和高度，

```html
<script setup>
import { ref, onMounted } from 'vue';

const container = ref(null);

onMounted(() => {
  if (container.value) {
    console.log('Width:', container.value.clientWidth);
    console.log('Height:', container.value.clientHeight);
  }
});
</script>

<template>
  <div ref="container" class="fullSize"></div>
</template>
```

- **clientHeight**：元素的内部高度（包括padding，但不包括水平滚动条、border和margin）。
- **clientWidth**：元素的内部宽度（同样，包括padding但不包括垂直滚动条、border和margin）。
- **offsetHeight**：元素的外部高度（包括padding、border和水平滚动条，但不包括margin）。
- **offsetWidth**：元素的外部宽度（包括padding、border和垂直滚动条，但不包括margin）。
- **scrollHeight**和**scrollWidth**：包括了因为滚动而不可见内容的整体尺寸。

通常来说，`clientHeight`和 `clientWidth`是获取元素可见部分尺寸的最常用属性。如果你的 `div`具有滚动条，或者你关心内容溢出的情况，`scrollHeight`和 `scrollWidth`也可能是有用的。如果需要考虑元素的边框，在某些情况下可能会选择使用 `offsetHeight`和 `offsetWidth`。

在Web开发中，`innerHeight` 并不是一个用来获取DOM元素尺寸的属性；而是一个属于 `window`对象的属性，它表示浏览器窗口的视图区域（视口）的高度，包括水平滚动条（如果存在的话）。

`window.innerHeight` 通常用于处理与视口大小相关的逻辑，如下几种典型用途：

1. **响应式设计**：JavaScript中可以使用 `window.innerHeight`来帮助确定浏览器窗口的当前高度，进而调整网页布局或功能以适应不同的显示设备。
2. **触发特定的行为**：当用户缩放浏览器窗口至某个特定的尺寸时，你可能希望根据窗口的大小改变来触发某些行为或动画。
3. **无限滚动**：在实现无限滚动的网页中，`window.innerHeight` 可以用来计算用户何时接近底部，从而在用户滚动到页面底部之前加载更多内容。
4. **全屏元素显示**：如果你想要某个元素（如视频、图片展示等）全屏显示，`window.innerHeight` 可以帮助你设置元素的高度，使之完全填充用户的视口。

下面是一个示例代码，展示如何使用 `window.innerHeight`来调整一个元素的高度使其总是占满整个视口：

```html
<style>
  .full-height {
    width: 100%;
    background-color: lightblue; /* 易于视觉识别的颜色 */
  }
</style>

<div class="full-height" id="fullHeightDiv">
  这个div高度将填满视口
</div>

<script>
  function adjustHeight() {
    const div = document.getElementById('fullHeightDiv');
    div.style.height = window.innerHeight + 'px';
  }

  window.onload = adjustHeight;
  window.onresize = adjustHeight; // 确保在窗口尺寸变化时更新高度
</script>
```

## tween的销毁

```
new TWEEN.Tween(yPosition)
          .to({ y: 100 }, 900)
          .easing(TWEEN.Easing.Quadratic.InOut)
          .start()
          .onUpdate(() => {
            loadingCover.style.setProperty(
              'transform',
              `translate(0,${yPosition.y}%)`
            )
          })
          .onComplete(() => {
            loadingCover.parentNode.removeChild(
              document.getElementById('loading-text-intro')
            ) // 写loadingCover会报空
            TWEEN.remove(this)
          })
```

### `tween.getId is not a function` 错误

这个错误提示表明，尝试调用的 `getId` 方法不存在于 `tween` 对象上。在你的代码中，错误来源于 `TWEEN.remove(this)` 这行代码。这里的问题是 `this` 在 `onComplete` 回调函数中不指向你期望的 tween 对象。在 ES6 的箭头函数中，`this` 关键字并不会绑定到外围的上下文，而是继承自它所在的上一层作用域。这意味着这里的 `this` 不是指向 Tween 实例，而可能是指向了全局对象或者 `undefined`（严格模式下）。

#### 解决方案

为了正确地移除 tween，你可以直接引用 tween 实例本身而不是使用 `this`。不过，从你的代码来看，并没有直接的引用。通常，如果你需要在回调中引用 tween，可以事先将其存储在一个变量中。但 `TWEEN.js` 库通常自我管理所有 tween 的生命周期，包括它们的添加和移除，所以手动调用 `remove` 方法可能并不是必须的。如果你确实需要移除 tween，考虑调整你的代码逻辑，避免在这种上下文中使用 `this`。

```
const tween1 = new TWEEN.Tween(yPosition)
          .to({ y: 100 }, 900)
          .easing(TWEEN.Easing.Quadratic.InOut)
          .start()
          .onUpdate(() => {
            loadingCover.style.setProperty(
              'transform',
              `translate(0,${yPosition.y}%)`
            )
          })
          .onComplete(() => {
            loadingCover.parentNode.removeChild(
              document.getElementById('loading-text-intro')
            ) // 写loadingCover会报空
            TWEEN.remove(tween1)
          })

        const tween2 = new TWEEN.Tween(camera1.position.set(0, 4, 2))
          .to({ x: 0, y: 2.4, z: 5.8 }, 3500)
          .easing(TWEEN.Easing.Quadratic.InOut)
          .start()
          .onComplete(() => {
            TWEEN.remove(tween2)
            document.querySelector('.header').classList.add('ended')
            document.querySelector('.description').classList.add('ended')
          })
```

## 查询元素

```
const loadingCover = document.getElementById('loading-text-intro')

        const tween1 = new TWEEN.Tween(yPosition)
          .to({ y: 100 }, 900)
          .easing(TWEEN.Easing.Quadratic.InOut)
          .start()
          .onUpdate(() => {
            loadingCover.style.setProperty(
              'transform',
              `translate(0,${yPosition.y}%)`
            )
          })
          .onComplete(() => {
            loadingCover.parentNode.removeChild(
              document.getElementById('loading-text-intro')
            ) // 写loadingCover会报空
            TWEEN.remove(tween1)
          })
```

`Cannot read properties of null (reading 'removeChild')` 错误

> 这个错误表明你尝试从一个 `null` 对象中调用 `removeChild` 方法。这通常发生在尝试访问或修改已经被移除或未被正确初始化的 DOM 元素时。在你的代码中，这个问题出现在尝试移除 `loadingCover` 元素的代码块中。

#### 解决方案

在尝试移除 `loadingCover` 之前，确保它存在。这个问题可能出现是因为 `loadingCover` 为 `null`，意味着 `document.querySelector('#loading-text-intro')` 没有找到匹配的元素。这可能是因为元素的 ID 错误，或者这段代码执行的时候，对应的 DOM 元素还没有加载到页面上。

你可以通过在尝试访问或修改元素之前检查它是否存在来解决这个问题：

```
if (loadingCover && loadingCover.parentNode) {
    loadingCover.parentNode.removeChild(loadingCover);
}
```

综合以上，你的问题可能是由于对 `this` 的误用和对 DOM 元素存在性的假设未经验证造成的。正确地引用变量和确保 DOM 元素存在，可以解决这些问题。

`document.querySelector` 和 `document.getElementById` 都是用来从 DOM 中获取元素的方法，但它们之间有一些关键的区别：

`document.querySelector`

- `querySelector` 方法可以接受任何 CSS 选择器作为参数，这意味着它非常灵活。你可以用它来获取类、ID、属性选择器等指定的元素。
- 如果有多个元素匹配给定的选择器，`querySelector` 只会返回第一个匹配的元素。
- 由于它接受任何 CSS 选择器，所以相对来说，执行速度会比 `getElementById` 慢一些。

```javascript
const element = document.querySelector('#a'); // 使用 # 来指定它是一个 ID
```

`document.getElementById`

- `getElementById` 方法仅接受一个 ID 作为参数，并返回具有该 ID 的元素。
- 它是直接根据 ID 来获取元素，不需要指定任何前缀，因为 ID 在 HTML 文档中应该是唯一的。
- `getElementById` 通常比 `querySelector` 更快，因为它专门用于通过 ID 查找元素，而 ID 在 DOM 中是唯一的。

```javascript
const element = document.getElementById('a');
```

如果你需要通过 ID 获取元素，并且对性能有一定要求，`getElementById` 是一个更好的选择。如果你需要更多的选择器灵活性（例如，通过类名、属性等获取元素），则应使用 `querySelector`。

- **性能**：对于通过 ID 获取单个元素的情况，`getElementById` 的性能通常更优。
- **灵活性**：`querySelector` 提供了更高的灵活性，可以使用任何有效的 CSS 选择器语法。

在大多数现代浏览器中，对于简单的 DOM 操作，这两种方法的性能差异几乎可以忽略不计。因此，你可以根据具体需求和个人偏好来选择使用哪种方法。

## 加载压缩过的glb

THREE .DRACOLoader: Unexpected geometrytype 错误解决

> https://blog.csdn.net/iefreer/article/details/131113390
>
> npm i draco3d
>
> npm i draco3dgltf
>
> https://segmentfault.com/q/1010000043706931
>
> 原因：`是draco版本与项目安装的three版本不对`
> 解决方法：以vite脚手架为例 `node_modules\three\examples\jsm\libs\` 下的darco文件夹拷贝到 `public\<span> </span>`下，之后报错就解决了。

### DRACO验证

检查一个模型是否包含了 Draco 压缩的几何数据，

- **使用 glTF Validator**：你可以使用 [glTF Validator](https://github.khronos.org/glTF-Validator/) 来检查你的 `.glb` 文件。这是一个由 Khronos Group 提供的工具，专门用于验证 glTF 文件的正确性。如果你的文件使用了 Draco 压缩，验证结果会显示相关信息。
- **查看文件内容**：高级用户可以直接查看 GLB 文件的内容（例如，使用十六进制编辑器），寻找 Draco 压缩的标记或结构。这种方法需要对 GLB 文件格式和 Draco 压缩有较深的了解。

```
{
    "uri": "statue.glb",
    "mimeType": "model/gltf-binary",
    "validatorVersion": "2.0.0-dev.3.8",
    "validatedAt": "2024-04-05T13:03:15.754Z",
    "issues": {
        "numErrors": 0,
        "numWarnings": 0,
        "numInfos": 3,
        "numHints": 0,
        "messages": [
            {
                "code": "UNSUPPORTED_EXTENSION",
                "message": "Cannot validate an extension as it is not supported by the validator: 'KHR_draco_mesh_compression'.",
                "severity": 2,
                "pointer": "/extensionsUsed/0"
            },
            {
                "code": "UNUSED_OBJECT",
                "message": "This object may be unused.",
                "severity": 2,
                "pointer": "/meshes/0/primitives/0/attributes/TEXCOORD_0"
            },
            {
                "code": "UNUSED_OBJECT",
                "message": "This object may be unused.",
                "severity": 2,
                "pointer": "/bufferViews/0"
            }
        ],
        "truncated": false
    },
    "info": {
        "version": "2.0",
        "generator": "Khronos glTF Blender I/O v1.8.19",
        "extensionsUsed": [
            "KHR_draco_mesh_compression"
        ],
        "extensionsRequired": [
            "KHR_draco_mesh_compression"
        ],
        "resources": [
            {
                "pointer": "/buffers/0",
                "mimeType": "application/gltf-buffer",
                "storage": "glb",
                "byteLength": 697820
            }
        ],
        "animationCount": 0,
        "materialCount": 1,
        "hasMorphTargets": false,
        "hasSkins": false,
        "hasTextures": false,
        "hasDefaultScene": true,
        "drawCallCount": 1,
        "totalVertexCount": 141741,
        "totalTriangleCount": 264708,
        "maxUVs": 1,
        "maxInfluences": 0,
        "maxAttributes": 3
    }
}
```

从你通过 glTF Validator 获取的信息中，我们可以了解到以下几点关于你的模型 `statue.glb`：

1. 基本信息

- **文件类型**：模型是一个二进制的 glTF 文件（GLB）。
- **验证器版本**：使用的 glTF Validator 版本为 2.0.0-dev.3.8。
- **验证时间**：文件在 2024-04-05T13:03:15.754Z 被验证。

2. 问题报告

- **错误数量**：0，表示文件没有违反 glTF 2.0 规范的错误。
- **警告数量**：0，没有发现可能影响性能或兼容性的问题。
- **信息数量**：3，提供了一些关于模型的额外信息但不影响模型的使用。
- **提示数量**：0，没有给出优化建议。

3. 具体消息

- **UNSUPPORTED_EXTENSION**：模型使用了 `KHR_draco_mesh_compression` 扩展进行 Draco 压缩，但 glTF Validator 不支持验证这个扩展的具体内容。这说明你的模型确实使用了 Draco 压缩。
- **UNUSED_OBJECT**：有两个未使用的对象，一个是纹理坐标 `TEXCOORD_0`，另一个是 `bufferViews[0]`。这可能意味着模型中包含了一些未被引用的数据，这通常不影响模型的使用，但可能稍微增加了文件的大小。

4. 模型信息

- **glTF 版本**：2.0。
- **创建工具**：使用了 Khronos glTF Blender I/O v1.8.19 创建。
- **扩展使用和要求**：模型使用并要求 `KHR_draco_mesh_compression` 扩展，证实了模型经过了 Draco 压缩。
- **资源信息**：包含一个 buffer，大小为 697820 字节。
- **动画、材质、顶点和三角形数量**：模型没有动画，有 1 个材质，共有 141741 个顶点和 264708 个三角形。

你的模型 `statue.glb` 成功使用了 Draco 压缩（通过 `KHR_draco_mesh_compression` 扩展），且没有发现任何违反 glTF 规范的错误。未使用的对象（如某些纹理坐标和 bufferViews）可能是导出过程中产生的，通常不会影响模型的渲染或使用。如果你关心文件大小，可以考虑优化这些未使用的数据。

这个报告也确认了你遇到的问题不是由文件损坏或不符合 glTF 规范造成的，可能需要在加载或渲染过程中进一步调查问题的原因。
