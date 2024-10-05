---
title: three案例实操|赛博风大屏Ⅱ
tags:
 - threejs
 - shader
categories:
 - WebGL
comments: true
date: 2024-09-11 10:02:05
photos: https://wallroom.io/img/3840x2160/bg-81f2d39.jpg
cover: https://wallroom.io/img/3840x2160/bg-81f2d39.jpg
---

# 前言

参考案例：https://dragonir.github.io/3d/#/earthDigital

# 调试工具

🔁 后话2-callback 🍵

* unforms统一存shader里uniform的初始值

🔁 后话1-callback 🍵

* 构造位于半径为5的球面上的冲击点、冲击最大半径、冲击比例、之前的点位置、飞线的比例和长度

安装dat.gui的库：`npm i dat.gui @types/dat.gui`

添加进入gui；

gui设置隐藏，通过键盘H键唤起；

# 飞线

🔁 后话1-callback 🍵

* 取消注释

![image-20240913170816909](C:\Users\Xiangting\AppData\Roaming\Typora\typora-user-images\image-20240913170816909.png)

### 制作飞线

初始化100个点，得到一条路径；添加index属性，形成起止正确的路径，加入trails

```jsx
  function makeTrail (idx) {
    let pts = new Array(100*3).fill(0); 
    let g = new Three.BufferGeometry();
    g.setAttribute("position", new Three.Float32BufferAttribute(pts,3));
    let m = new Three.LineDashedMaterial({
      color: params.colors.gradOuter,
      transparent: true,
      onBeforeCompile: shader => {
        shader.uniforms.actionRatio = impacts[idx].trailRatio;
        shader.uniforms.lineLength = impacts[idx].trailLength;
        shader.fragmentShader = lineFragmentShader;
      }
    })
    let l = new Three.Line(g,m);
    l.userData.idx = idx;
    setPath(l,impacts[idx].prevPosition,impacts[idx].impactPosition);
    trails.push(l);
  }
```

设置路径上点的位置和长度：传入当前路径、起点、终点、峰高、出现后经过几次弧度再进入后消失

```jsx
  const setPath = (l, startPoint, endPoint, peakHeight,cycles) => {
    let pos = l.geometry.attributes.position; //预存点的最新位置
    let division = pos - 1; //l上的分段数目
    let cycle = cycles || 1; // cycle=4:↷↷↷↷
    let peak = peakHeight || 1; //峰高
    let radius = startPoint.length(); // 对应球的半径
    let angle = startPoint.angleTo(endPoint); //起始点和终点的夹角
    let arcLength = radius * angle; //弧长

    let diameterMinor = arcLength / Math.PI; // 新圆的直径
    let radiusMinor = diameterMinor / 2 / cycle; // 考虑cycle下新圆的半径
    let peakRatio = peak / diameterMinor; // 峰高比例
    let radiusMajor = radius + radiusMinor; // 大圆的半径

    let basisMajor = new Three.Vector3().copy(startPoint).setLength(radiusMajor); // trail的点基准1
    let basisMinor = new Three.Vector3().copy(startPoint).negate().setLength(radiusMinor); // trail的点基准2

    let tri = new Three.Triangle(startPoint, endPoint, new Three.Vector3()); // 三角形
    let nrm = new Three.Vector3();
    tri.getNormal(nrm); // 拿到法线

    let v3Major = new Three.Vector3(); //里面的v3表示vec3 
    let v3Minor = new Three.Vector3();
    let v3Inter = new Three.Vector3();
    let vFinal = new Three.Vector3(); // 里面v也可理解为varying

    for(let i = 0;i<=division;i++) {
      let divisionRatio = i / division; // 分段比例
      let angleValue = divisionRatio * angle; // 分段角度
      v3Major.copy(basisMajor).applyAxisAngle(nrm, angleValue); // 在basisMajor的基础上绕着法线旋转
      v3Minor.copy(basisMinor).applyAxisAngle(nrm, angleValue+Math.PI*2*divisionRatio*cycle); // 在basisMinor的基础上绕着法线旋转
      v3Inter.addVectors(v3Major, v3Minor); // 两个向量相加
      let newLength = (v3Inter.length() - radius) * peakRatio + radius; // 新的长度
      vFinal.copy(v3Inter).setLength(newLength); // 设置新的长度
      pos.setXYZ(i,vFinal.x,vFinal.y,vFinal.z); // 设置新的位置
    }

    //更新完了点数据后需要加上这句
    pos.needsUpdate = true;

    l.computeLineDistances();  // 计算每个顶点到起点的累加距离
    l.geometry.attributes.lineDistance.needsUpdate = true;
    impacts[l.userData.idx].trailLength.value = l.geometry.attributes.lineDistance.array[99];
    l.material.dashSize = 3;
  }
```

![202408281104602.png](https://s2.loli.net/2024/08/28/GWszxuwCZIaV1EB.png)

#### 飞线shader

下面是如何在你的 Vite 配置中添加对 `.glsl` 文件的支持的步骤：

1. 安装 `vite-plugin-glsl`：
   ```
   npm install vite-plugin-glsl
   ```
2. 在你的 Vite 配置文件中（通常是 `vite.config.js` 或 `vite.config.ts`）添加插件：
   ```
   import { defineConfig } from 'vite'
   import vue from '@vitejs/plugin-vue'
   import glsl from 'vite-plugin-glsl'
   
   export default defineConfig({
     plugins: [
       vue(),
       glsl()
     ]
   })
   ```

这样配置后，当你导入 `.glsl` 文件时，`vite-plugin-glsl` 将自动处理它们，确保 GLSL 代码被作为字符串正确导入，避免 JavaScript 解析错误。

确保在你的 Vue 组件或 JavaScript 文件中按照下面的方式导入 `.glsl` 文件：

```
import textFragmentShader from './Shader/text.frag.glsl'
import textVertexShader from './Shader/text.vert.glsl'
```

这些文件现在应该会被正确地作为字符串导入，可以直接使用在 Three.js 的 `ShaderMaterial` 中。这应该解决你遇到的关于 GLSL 代码导入的问题。


页面中导入：

```
import lineFragmentShader from './line.frag.glsl';
```

shader内容：

actionRatio 代码中是0 _ 动画中修改 

vLineDistance // 在使用 Three.js 的 LineDashedMaterial 时，确保顶点着色器正确地计算并传递 vLineDistance 变量到片元着色器是非常关键的，因为这个变量决定了线段的虚线效果。

totalSize 一整个没用到

lineLength 到起点的累积距离


这段代码是一个在 Three.js 中使用的 GLSL shader，具体是用来处理具有虚线效果的线条材料（`LineDashedMaterial`）。这个 shader 控制着如何根据距离和虚线模式显示线条的片段。

- **uniforms** 是从 Three.js 的 JavaScript 代码传递到 shader 的变量。
  - `mediump float`: 精度指示符，表明浮点运算使用中等精度。
  - `actionRatio`: 控制线条的动画或进度的参数。
  - `lineLength`: 线条的总长度。
  - `diffuse`: 线条的颜色。
  - `opacity`: 线条的不透明度。
  - `dashSize`: 单个虚线的长度。
  - `totalSize`: 虚线和间隔的总和长度。
- **varying**
  - `vLineDistance`: 表示当前片段在整个线条中的位置。

**虚线计算**:
  - 首先计算出当前应显示的虚线位置。
  - 根据 `vLineDistance` 和虚线参数计算出当前片段是否在虚线中还是在空白间隔中。如果在间隔中，使用 `discard` 抛弃这个片段，不进行渲染。
  - 使用渐变 (`grad`) 来处理虚线的边缘，使得虚线边缘平滑过渡。

1. 顶点着色器中的计算:

在顶点着色器中，每个顶点的位置被用来计算它在整条线中的相对位置或距离，这个距离累计到 vLineDistance。例如，如果线条由多个段组成，每个段的长度会被累加到起点距离，直到当前顶点的位置。

2. 传递到片段着色器:

这个计算出的距离 (vLineDistance) 作为一个 varying 变量，被传递到片段着色器。在图形管线中，顶点着色器处理后的结果（如位置、颜色、距离等）会通过插值传递到片段着色器。

3. 在片段着色器中，vLineDistance 用来确定每个片段是否应该被渲染为虚线的一部分：

> float currPos = (lineLength + dashSize) * actionRatio;
这里 currPos 表示虚线开始的位置，是通过线条总长度、虚线大小和动画比例 (actionRatio) 计算的。actionRatio 可能用于动态调整虚线的显示，比如滚动效果。
确定片段位置:

> float d = (vLineDistance + halfDash) - currPos;
这个计算检查当前片段的位置（vLineDistance + halfDash）相对于当前虚线开始位置的偏移量。halfDash 用于调整计算到虚线中心的距离。
判断是否在虚线内部:

> if (abs(d) > halfDash ) discard;如果偏移量大于 halfDash，则表示当前片段不在虚线内部，应该被丢弃（不渲染）。这样，只有在虚线范围内的片段会被渲染，形成断续的线条效果。

4. 渐变边缘的处理:

float grad = ((vLineDistance + halfDash) - (currPos - halfDash)) / halfDash; 这里 grad 用于计算当前片段在虚线边缘的位置，用于实现边缘的渐变效果。这可以让虚线的开始和结束更加平滑，不会突然截断。

![ac03418e3341f086f3b780056121b39.jpg](https://s2.loli.net/2024/09/11/yfOMo2NWJZVekbq.jpg)

#### 补充 | `negate`

`negate()` 是一个 Three.js 中的向量方法，它用来将向量中的每个分量取反，也就是每个分量乘以 -1。这样做的目的是将向量的方向反转。具体到这段代码：

```javascript
let basisMinor = new THREE.Vector3().copy(startPoint).negate().setLength(radiusMinor);
```

- `new THREE.Vector3()` 创建一个新的三维向量，默认为 (0, 0, 0)。
- `.copy(startPoint)` 将 `startPoint` 的值复制到这个新的向量中。
- `.negate()` 将复制后的向量中的每个分量乘以 -1，实现向量方向的反转。
- `.setLength(radiusMinor)` 将反转后的向量的长度设置为 `radiusMinor`。

这样，`basisMinor` 就变成了一个方向与 `startPoint` 相反、长度为 `radiusMinor` 的向量。

#### 补充 | `applyAxisAngle`

在 Three.js 中，`applyAxisAngle` 方法用于将一个旋转应用到一个向量上。这个方法接受两个参数：一个轴向量和一个角度。它的作用是围绕给定的轴向量旋转原向量指定的角度。

1. **轴向量** (`axis`): 这是一个标准化的三维向量，指定了旋转的轴。例如，若轴向量为 `(0, 1, 0)`，则表示围绕 y 轴进行旋转。
2. **角度** (`angle`): 这是旋转的角度，单位是弧度。正值表示逆时针旋转，负值表示顺时针旋转（根据右手规则）。

当你调用 `applyAxisAngle` 方法时，它会改变调用它的向量的方向，但保持向量的长度不变。这种旋转是通过右手规则来定义的，即如果你的右手拇指指向轴向量的方向，那么四指的卷曲方向定义了正旋转方向。

在 Three.js 中使用 `applyAxisAngle` 方法进行旋转时，向量是绕通过原点 (0, 0, 0) 的轴进行旋转的。轴向量只定义了旋转的方向和轴线，而不是旋转的位置。因此，这种旋转总是认为轴向量通过三维空间的原点。

旋转的中心点是坐标系统的原点 (0, 0, 0)。向量从它当前的位置开始，绕通过原点的轴旋转。这意味着：

* 如果向量的一个端点位于原点，旋转将直接改变向量的方向，而长度保持不变。
* 如果向量的一个端点不在原点，向量会在想象中被拉直至原点，然后绕轴旋转，再放回原位置。

如果你需要绕一个不在原点的点旋转向量，你需要先将系统平移到那个点变成新的原点，执行旋转，然后再平移回去。这可以通过以下步骤完成：

1. **平移** ：将向量和旋转中心点一同平移到原点附近。
2. **旋转** ：在新的位置应用旋转。
3. **逆平移** ：将旋转后的向量移回原始位置。

#### 补充 | needsUpdate

在 Three.js 中，`needsUpdate` 属性用于告诉引擎某个对象的数据已经改变，需要重新计算或重新上传到 GPU。这个属性常见于与几何体（`Geometry` 或 `BufferGeometry`）、材料（`Material`）、纹理（`Texture`）等相关的对象。

当你修改了几何体的顶点数据、纹理的内容、或者材料的参数等，这些改变不会自动反映在渲染的对象上，除非你明确地告诉 Three.js 这些数据已经更新。`needsUpdate` 属性就是用于这种通知。

示例 - 几何体顶点数据更新

假设你更改了一个几何体的顶点位置数据，你需要设置对应属性的 `needsUpdate` 为 `true`，以确保这些改变被应用到下一次渲染：

```javascript
// 修改几何体顶点位置
geometry.attributes.position.setXYZ(index, newX, newY, newZ);

// 标记顶点位置数据为需要更新
geometry.attributes.position.needsUpdate = true;
```

示例 - 纹理内容更新

如果你修改了纹理的图像数据，你同样需要设置 `needsUpdate`：

```javascript
// 加载一个新的图像到纹理
texture.image = newImage;

// 告诉Three.js纹理已更新
texture.needsUpdate = true;
```

示例 - 材料属性更新

当改变材料的一些属性（如颜色、透明度等）后，如果要立即反映这些变化，同样需要更新 `needsUpdate`：

```javascript
// 修改材料的颜色
material.color.setHex(0xff0000);

// 标记材料需要更新
material.needsUpdate = true;
```

#### 补充 | `computeLineDistances`

在 Three.js 中，`Mesh` 对象本身并没有 `computeLineDistances()` 方法；这个方法是属于 `Line` 类的。`Line` 类用于创建和处理线段对象，在 3D 场景中表示由多个点连接而成的直线或折线。

该方法用于计算线段对象中每个顶点到线起点的累计距离，并将这些距离存储在线段的 `lineDistances` 属性中。这个功能通常与线性材料（`LineDashedMaterial`）一起使用，用来创建虚线效果。`LineDashedMaterial` 需要这些距离来正确地渲染每段虚线。

在 Three.js 中，当你使用 `computeLineDistances()` 方法计算线段（由 `THREE.Line` 或 `THREE.LineSegments` 类创建的对象）的每个顶点到起点的距离时，这些距离会存储在 `geometry.attributes.lineDistance` 的 `array` 属性中。这个数组中的每个元素代表从起点到对应顶点的累积距离。

数组索引是从 0 开始的。因此，数组中的第一个元素（索引 0）对应第一个顶点（通常是线段的起点），第二个元素（索引 1）对应第二个顶点，依此类推。索引 99 的元素就是数组中的第 100 个元素，对应于你的线段的第 100 个顶点。在 `array[99]` 中获取的值表示从线段的起点到第 100 个顶点的累积距离。

注意事项：

- 如果你更改了线段的顶点位置，需要重新调用 `computeLineDistances()` 方法来更新距离数据。
- 这个方法仅对 `THREE.Line` 或 `THREE.LineSegments` 对象有效，对 `Mesh` 对象不适用。

#### 补充 | 预制shader

在Three.js的shader编程中，`#include` 语句用于插入共用的代码块，这些代码块通常封装了一些常用的函数和变量定义，使得shader的编写更加模块化和可复用。下面是你提到的几个常见的include文件的用途：

1. **`<common>`**:
   - 这个文件包括了一些常用的数学函数和宏定义，比如计算线性插值、饱和度计算等，还有一些常用的常量定义，例如PI的值等。

2. **`<color_pars_fragment>`**:
   - 这个文件定义了与颜色处理相关的参数和函数，比如处理顶点颜色、漫反射等。

3. **`<fog_pars_fragment>`**:
   - 用于定义和计算雾效果(fog)的参数，使得物体在雾中逐渐消失的效果可以在shader中实现。

4. **`<logdepthbuf_pars_fragment>`**:
   - 如果启用了logarithmic depth buffer，这个文件包含了相关的实现，用来改善远距离渲染时的深度精度问题。

5. **`<clipping_planes_pars_fragment>`**:
   - 这个文件提供了剪裁平面的支持，使得可以在shader中处理剪裁逻辑，仅渲染剪裁平面允许的部分。

要查找Three.js中可用的所有shader chunks，最好的方法是直接查看Three.js的源代码。在GitHub上的Three.js库中，这些shader chunk文件位于`src/renderers/shaders/ShaderChunk/`目录下。


### 飞线动画

内容结构：

``` jsx
const setTrailAnimation = () => {
    let tweens = [];
    for(let i =0;i<maxImpactAmount;i++) {
      tweens.push({
        runTween: ()=>{}
      })
    }
    tweens.forEach(t=>t.runTween());
    createPoints(); // 因为runTween会影响到points的效果
  }
```

for循环里的设置

更新了impacts，同时也影响到了球体的效果；

``` jsx
    for (let i = 0; i < maxImpactAmount; i++) {
      tweens.push({
        runTween: () => {
          let path = trails[i]; // 当前路径
          let speed = 3;
          let len = path.geometry.attributes.lineDistance.array[99];
          let dur = len / speed; // 持续时间

          let tweenTrail = new TWEEN.Tween({ value: 0 })
            .to({ value: 1 }, dur * 1000)
            .onUpdate(val => {
              impacts[i].trailRatio.value = val.value; // 通过Tween来控制trailRatio/actionRatio
            });
          var tweenImpact = new TWEEN.Tween({ value: 0 })
            .to({ value: 1 }, Three.MathUtils.randInt(2500, 5000))
            .onUpdate(val => {
              uniformsSettings.impacts.value[i].impactRatio = val.value; // 通过Tween来控制impactRatio
            })
            .onComplete(val => {
              impacts[i].prevPosition.copy(impacts[i].impactPosition);
              impacts[i].impactPosition.random().subScalar(0.5).setLength(5);
              setPath(path, impacts[i].prevPosition, impacts[i].impactPosition, 1);
              uniformsSettings.impacts.value[i].impactMaxRadius = 5 * Three.MathUtils.randFloat(0.5, 0.75);
              tweens[i].runTween();
            });
          tweenTrail.chain(tweenImpact);
          tweenTrail.start();
        }
      })
    }
```