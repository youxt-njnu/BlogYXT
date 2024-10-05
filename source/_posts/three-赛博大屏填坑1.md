---
title: three案例实操|赛博风大屏坑①
tags:
 - threejs
 - shader
categories:
 - WebGL
comments: true
date: 2024-09-21 14:02:05
photos: https://wallroom.io/img/3840x2160/bg-81f2d39.jpg
cover: https://wallroom.io/img/3840x2160/bg-81f2d39.jpg
---

# 问题(第一批)

## 球体的陆地和海洋并没有区分出来

尝试1：看看texture是否正确加载

``` jsx
let earthTexture = new Three.TextureLoader().load(earthImg, function (texture) {
      console.log('texture loaded successfully');
    }, undefined, function (err) { console.log('texture load failed', err); });
```
问题是对UV的计算错了：
错处1：` let uv = [(sph + Math.PI) / (2 * Math.PI), 1 - (sph.theta / Math.PI)];`

需要使用new Three.Vector2(),而不是直接构建数组；如果是直接构建数组，那下面就访问不到.x和.y，需要使用[0]和[1]来访问对应的内容。

错处2：`let uv = new Three.Vector2(sph + Math.PI) / (2 * Math.PI), 1 - (sph.theta / Math.PI));`

这里对UV的映射计算错了

> 在 Three.js 中，`Spherical` 类表示的球坐标系统是基于三维右手笛卡尔坐标系，其中 `phi`（仰角）和 `theta`（方位角）的定义具体如下：
> 1. **`phi`（仰角）** - 这是从正Y轴向下至点P的线段与Y轴之间的角度。在 Three.js 中，`phi` 的范围通常是从0到π（即从0到180度），其中0对应于正Y轴（向上）的方向，而π对应于负Y轴（向下）的方向。
> 2. **`theta`（方位角）** - 这是在XZ平面上，从正Z轴向正X轴的角度。`theta` 的范围是从0到2π（即从0到360度），其中0开始于正Z轴，增加方向是逆时针（从屏幕向里到屏幕向外看为正Z方向）。
因此，`Spherical` 的 `phi` 和 `theta` 的直观理解如下：
> - `phi = 0`：点位于Y轴的正方向（向上）。
> - `phi = π/2`：点位于XZ平面。
> - `phi = π`：点位于Y轴的负方向（向下）。
> - `theta = 0`：点位于Z轴的正方向（向屏幕外）。
> - `theta = π/2`：点位于X轴的正方向（向右）。
> - `theta = π`：点位于Z轴的负方向（向屏幕里）。
> - `theta = 3π/2`：点位于X轴的负方向（向左）。
> 这种球坐标系统非常有用于处理与球面或者环绕运动相关的场景，如天体模拟、相机环绕目标物体的动作等。


正确写法：`let uv = new Three.Vector2((sph.theta + Math.PI) / (2 * Math.PI), 1 - (sph.phi / Math.PI));` 

为了正确理解这段代码 `let uv = [(sph.theta + Math.PI) / (2 * Math.PI), 1 - (sph.phi / Math.PI)];` 并将二维贴图UV映射到球面上，让我们逐一分析每个部分：

1. **`(sph.theta + Math.PI) / (2 * Math.PI)`**:
   - `sph.theta` 表示球坐标中的方位角，通常的取值范围是从 0 到 2π，表示从正Z轴顺时针到X轴再回到Z轴的全周角度。
   - `+ Math.PI` 的作用是将θ的起始点从正Z轴（前面）调整到负Z轴（后面）。通常，这种调整是为了使UV映射的起点对应于模型的后方，从而使得当模型前向朝向观察者时，贴图的“前面”能够正对观察者。
   - 除以 `(2 * Math.PI)` 将调整后的θ值归一化到 [0, 1] 的范围内，这样可以映射到贴图的水平坐标U。

2. **`1 - (sph.phi / Math.PI)`**:
   - `sph.phi` 是从正Y轴向球面的点的仰角，取值范围是从 0 到 π。
   - `(sph.phi / Math.PI)` 将φ值归一化到 [0, 1] 范围内，其中0代表北极，1代表南极。
   - `1 -` 的作用是反转V坐标，使得在UV贴图中，V = 0 对应于球体的北极，V = 1 对应于球体的南极。这样的反转是必要的，因为在大多数图形处理系统中，贴图的V坐标从下到上增加，而球坐标系统中的φ是从上到下增加的。


(Three.js地理坐标和三维空间坐标的转换)[https://blog.csdn.net/qihoo_tech/article/details/101443066]

![image-20240908162225403.png](https://s2.loli.net/2024/09/09/pbtr2HfNW7xTSkC.png)

## trail没显示出来

vLineDistance // 在使用 Three.js 的 LineDashedMaterial 时，确保顶点着色器正确地计算并传递 vLineDistance 变量到片元着色器是非常关键的，因为这个变量决定了线段的虚线效果。

原先是再setPath里修改点的位置，然后计算lineDistance，由于lineDistance计算有问题，所以在fragment shader里，vLineDistance就都是0

在setPath的代码里，求division需要用pos.count来-1，之前是pos直接-1，导致了错误，使得trail的pos都没有正确更新；

## 按钮切换效果不出来

如果在 `useEffect` 中的依赖数组（第二个参数）设置为空数组 `[]`，这意味着 `useEffect` 只会在组件首次挂载时运行一次，而不会在组件的状态或属性更新时再次运行。这常用于执行那些只需一次的初始化操作，如 API 调用或设置初始配置。


在您的情况中，如果 `animate` 函数在 `useEffect` 内定义，并且依赖数组为空，这会导致 `animate` 函数捕获到初始渲染时的状态值，例如 `renderGlithPass` 的初始值（`false`）。即使后续 `renderGlithPass` 状态更新，`animate` 函数中的值也不会更新，因为 `useEffect` 不会重新执行来更新闭包中的状态值。

解决方法

要让 `animate` 函数能够访问最新的 `renderGlithPass` 状态，您有两个选择：

1. **更新 `useEffect` 的依赖数组**：在依赖数组中包含 `renderGlithPass`，这样每次 `renderGlithPass` 改变时，`useEffect` 都会重新执行，`animate` 函数也将重新定义，从而捕获到最新的状态值。

```javascript
useEffect(() => {
  const animate = () => {
    console.log(renderGlithPass);
  };

  const animationId = requestAnimationFrame(animate);

  return () => cancelAnimationFrame(animationId);
}, [renderGlithPass]); // 现在包含 renderGlithPass 作为依赖
```

2. **使用 `useRef` 追踪最新的状态**：如之前所述，您可以使用 `useRef` 来追踪 `renderGlithPass` 的最新值，这样 `animate` 函数可以通过 ref 访问当前的状态，而无需重新定义 `animate` 函数。

```javascript
const renderGlithPassRef = useRef(renderGlithPass);
renderGlithPassRef.current = renderGlithPass;

useEffect(() => {
  const animate = () => {
    console.log(renderGlithPassRef.current); // 使用 ref 访问最新状态
  };

  const animationId = requestAnimationFrame(animate);

  return () => cancelAnimationFrame(animationId);
}, []); // 依赖数组仍然为空
```

这两种方法各有优势，选择哪一种取决于您的具体需求和组件的其他逻辑。如果 `animate` 函数对性能要求较高并且状态更新频繁，使用 `useRef` 可能是一个更好的选择。如果状态更新对 `animate` 的影响比较大，并且确保每次都是最新的状态很重要，那么更新 `useEffect` 的依赖数组可能更适合。

## animate里的renderGlithPass总是false，未更新

在你的React代码中，你遇到的问题是由于闭包（closure）引起的。在JavaScript和React中，闭包会捕获它们创建时的环境状态。当你在 `useEffect` 钩子里调用 `animate()` 函数时，这个函数被“固定”在了那一刻的状态，包括 `renderGlithPass` 的值。

因为 `useEffect` 仅在组件挂载时执行一次（因为它的依赖列表是空的 `[]`），所以 `animate` 函数只会捕获 `renderGlithPass` 最初的值，即 `false`。之后即使状态更新了，`animate` 函数中捕获的 `renderGlithPass` 的值仍然是最初的 `false`。

解决这个问题的一个方法是使用 `useRef` 钩子来持久化 `renderGlithPass` 的值，这样 `animate` 函数总是能获取到最新的状态。这里是如何修改你的代码：

```javascript

  const [renderGlitchPass, setRenderGlitchPass] = useState(false);
  const renderGlitchPassRef = useRef(renderGlitchPass);  // 使用 useRef 来持久化状态

  useEffect(() => {
    renderGlitchPassRef.current = renderGlitchPass;  // 更新 ref 的值
  }, [renderGlitchPass]);  // 每当 renderGlitchPass 更新时，更新 ref

  useEffect(() => {
    const animate = () => {
      TWEEN.update();
      earth.rotation.y += 0.001;
      renderer && scene && camera && renderer.render(scene, camera);
      requestAnimationFrame(animate);

      // 使用 ref 的 current 值来获取最新状态
      renderGlitchPassRef.current && composer.render();
    };

    animate();  // 启动动画循环
  }, []);  // 空依赖列表，仅在组件挂载时执行

  const handleStartButtonClick = () => {
    setRenderGlitchPass(!renderGlitchPass);  // 切换状态
  };
```