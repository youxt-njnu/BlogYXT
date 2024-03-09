---
title: three+react|个人主页实战Ⅱ
tags:
 - WebGL
 - 3DGIS
categories:
 - 3DGIS
comments: true
date: 2024-02-25 11:28:05
photos: https://wallroom.io/img/3840x2160/bg-81f2d39.jpg
cover: https://wallroom.io/img/3840x2160/bg-81f2d39.jpg
---
# 说明

[模仿项目地址](https://github.com/youxt-njnu/3d_portfolio_primary)，[参考的视频教程出处](https://www.youtube.com/watch?v=FkowOdMjvYo)，[react入门](https://juejin.cn/post/6960262593265025031)

[react three fiber](https://docs.pmnd.rs/react-three-fiber/getting-started/your-first-scene#adding-lights)

ChatGPT辅助生成❗自辨❗❗❗

# 相关库

## React Three

`@react-three/fiber`表明这个包是React Three（一个专注于在React中使用three.js的项目）下的一个模块或子项目。这有助于开发者快速识别包的来源，同时也方便了对项目包的管理。

作用域包的结构让开发者更容易地发现和维护相关的包。例如，React Three项目可能包含多个相关包（如 `@react-three/fiber`、`@react-three/drei`等），这些都被归类在 `@react-three`这个作用域下。这种组织方式使得维护者和使用者能够轻松地识别和更新这些相互关联的包。

### 1. `@react-three/fiber`

- **作用**：这是React Three Fiber项目的核心库，提供了将three.js集成到React的基础架构。它是一个React渲染器，允许你以声明式的方式在React应用中创建和控制3D场景。
- **区别**：作为基础库，它不提供额外的three.js对象或抽象，而是专注于提供与React兼容的three.js渲染环境。

### 2. `@react-three/drei`

- **作用**：`drei`是一个工具包，提供了许多有用的辅助组件和钩子（hooks），以简化在React Three Fiber中开发3D场景的过程。这些组件包括环境灯光、效果、加载器、抽象的3D对象等。
- **区别**：与 `@react-three/fiber`相比，`drei`更多地提供了构建3D场景时的"快捷方式"，帮助开发者减少样板代码和加快开发流程。

### 3. `@react-three/cannon`

- **作用**：这个包提供了物理引擎的集成，基于 `cannon.js`物理引擎。它允许开发者在React Three Fiber创建的3D场景中添加物理效果，如碰撞检测、重力和弹性。
- **区别**：专注于为3D对象添加物理特性，而不是3D渲染或组件的直接创建。

### 4. `@react-three/postprocessing`

- **作用**：该包提供了对后处理效果的支持，允许开发者在React Three Fiber场景中添加和配置后处理效果，如模糊、光晕、色彩校正等。
- **区别**：专注于渲染流程中的后期处理效果，改善或增加场景的视觉效果。

### 5. `@react-three/gui`

- **作用**：提供了一个简易的图形用户界面（GUI），用于在开发过程中调试和修改three.js场景的参数。
- **区别**：主要用于开发和调试阶段，通过图形界面快速调整场景参数，而不直接影响3D内容的渲染或逻辑。

### 6. `@react-three/xr`

- **作用**：支持构建虚拟现实（VR）和增强现实（AR）体验。它提供了创建和管理XR会话的工具，允许开发者在兼容的设备上提供沉浸式的3D体验。
- **区别**：专门针对XR应用的开发，提供了与VR和AR技术集成的工具和组件


# React Three Fiber

[官网教程](https://docs.pmnd.rs/react-three-fiber/getting-started/your-first-scene#adding-lights)

## [Canvas](https://docs.pmnd.rs/react-three-fiber/api/canvas)

**Scene+Camera+raycaster+(shadow)**

**Canvas里面的内容，可以用驼峰法写，不需要导入，直接是原生的JSX组件。如 **`<mesh></mesh>`, `<boxGeometry />`, `<meshStandardMaterial />`。但从v8以后，不会自动化导入这些，可以导入react-three-fiber，然后可以直接写这些；或者是通过import 'three'

**ThreeJS里的类的构造器的参数传入，**`<boxGeometry args={[w,h,d]}/>`

## 与threejs的转换

[对象，属性和构造器参数设置](https://docs.pmnd.rs/react-three-fiber/api/objects)（说明了从ThreeJS过渡到react-three-fiber的一些注意点）

**有set方法的对象**

> **可以直接用=，如果有多个参数，可以放入array**
>
> `color="hotpink"` not `color={new THREE.Color('hotpink')}`
>
> `position={[100,0,0]}` not set

**有setScalar这种类似的方法，可以直接用属性scale**

> `<mesh scale={1} />` or `<mesh scale={[1,1,1]} />`

**有类似mesh.rotation.x这种串接的方法，使用-来连接**

> `<mesh rotation-x ={1} />`

## attach

**在 **`react-three-fiber`（R3F）中，`attach`属性是一个非常重要的概念，用于将React组件的某些属性或对象“附加”到Three.js的父对象上。这样做的目的是为了在R3F的React元素树中保持Three.js场景图的结构和属性同步。

`attach`属性通常用于 `<primitive>`组件或任何自定义组件内部，来指定如何将当前组件的Three.js对象（如材质、几何体、相机等）附加到其父Three.js对象上。例如，你可以将一个材质附加到一个网格上，或者将一个相机附加到场景中。

```
<mesh>
  <boxGeometry attach="geometry" args={[1, 1, 1]} />
  <meshStandardMaterial attach="material" color="orange" />
</mesh>
```

**在这个例子中：**

* `<boxGeometry>`的 `geometry`对象被附加到父 `<mesh>`的 `geometry`属性上。
* `<meshStandardMaterial>`的 `material`对象被附加到相同 `<mesh>`的 `material`属性上。

**在传统的Three.js应用中，你需要手动管理场景图的所有方面，包括创建对象、设置属性、添加到场景中等。而在R3F中，**`attach`属性简化了这一过程，允许你直接在JSX中以声明式的方式组织和连接Three.js的对象。

**例如，传统Three.js中创建和添加一个带材质的立方体可能需要这样：**

```
const geometry = new THREE.BoxGeometry(1, 1, 1);
const material = new THREE.MeshStandardMaterial({color: 'orange'});
const mesh = new THREE.Mesh(geometry, material);
scene.add(mesh);
```

**而在R3F中，相同的操作可以通过使用 **`attach`属性在JSX中直接声明，如前面的例子所示，这使得代码更加简洁并且保持了React的声明式风格。

**总之，**`attach`属性是 `react-three-fiber`中连接React组件属性和Three.js对象的强大工具，它简化了在React中构建和管理复杂3D场景的过程。

## Hooks

**只能使用在Canvas元素里，因为Hooks依赖于context.**

### useThree

**const state = useThree()**

**state.gl, .scene, .camera, .raycaster, .pointer(.mouse), .clock, .linear, .legacy, .frameloop, .performace, .size, .viewport, .set(), .get(), .invalidate(), .setSize(), .setDpr(), .setFrameloop(), .setEvents(), .onPointerMissed(), .events**

### useFrame

**useFrame((state, clock_delta, xrFrame) =>{})**

### useLoader

### useGraph

[官方案例](https://docs.pmnd.rs/react-three-fiber/getting-started/examples)

## @react-spring

`@react-spring`是一个基于Spring物理原理的现代React动画库，它允许开发者以声明式的方式在React应用中创建流畅、自然的动画效果。`@react-spring`的设计旨在简化动画的创建和管理，提供了一种简单而强大的方法来实现复杂的动画效果，无论是简单的值变化、列表的动态排序，还是复杂的交互动画。

### 核心特性

- **简洁的API**：通过简单的API，开发者可以轻松创建和控制动画。
- **物理原理驱动**：动画效果基于真实的物理原理，使得动画看起来更自然。
- **高性能**：优化的性能确保即使在复杂动画中也能保持流畅。
- **适用范围广**：支持Web（React）、React Native和其他平台，通过相同的API在不同平台上创建动画。

### 主要子项目

`@react-spring`项目包含了几个子项目，分别针对不同的平台或提供特定的功能：

1. **`@react-spring/web`**：专为Web平台设计，用于在Web应用中创建动画。它提供了与DOM元素交互的能力，是构建Web界面动画的理想选择。
2. **`@react-spring/native`**：专为React Native设计，允许在React Native应用中创建流畅的原生动画效果。它利用React Native的动画库来实现高性能的动画。
3. **`@react-spring/core`**：是 `@react-spring`库的核心，提供了动画功能的基本实现。其他子项目如 `@react-spring/web`和 `@react-spring/native`都是在这个核心基础上扩展而来，以支持特定平台的动画效果。
4. **`@react-spring/three`**：为three.js提供动画支持，允许在使用React Three Fiber开发的3D场景中创建和管理动画。这使得开发者可以在3D应用中实现复杂的动画效果。
5. **`@react-spring/konva`**：用于在React Konva（一个用于在React中绘制2D canvas图形的库）项目中创建动画。这使得开发者可以为2D图形和图像添加动画效果。

### 区别

- **平台支持**：主要区别在于各个子项目针对的平台或库不同。`@react-spring/web`专注于Web平台，`@react-spring/native`专注于React Native，而 `@react-spring/three`和 `@react-spring/konva`则分别针对three.js和Konva。
- **使用场景**：虽然这些子项目在API设计上保持一致性，但它们提供的动画能力和使用场景各有侧重，例如 `@react-spring/three`专门处理3D动画，而 `@react-spring/konva`处理2D canvas动画。

### 总结

`@react-spring`提供了一个跨平台的动画解决方案，通过不同的子项目满足了在Web、React Native、3D场景和2D canvas图形中创建动画的需求。

## React router dom

**在React应用中使用 **`react-router-dom`库时，`Route`、`BrowserRouter`（在这里使用别名 `Router`），和 `Routes`组件是构建单页面应用（SPA）路由系统的核心。下面是每个组件的作用以及为什么它们通常会一起被导入和使用：

**BrowserRouter (别名 Router)**

* **作用**：`BrowserRouter`是一个使用HTML5历史API（`pushState`、`replaceState`和 `popstate`事件）来保持UI与URL同步的路由器。它为React应用提供了一个路由的上下文环境。使用 `BrowserRouter`时，你的网址看起来很“干净”，不会有 `#`符号。

**Routes**

* **作用**：`Routes`组件在 `react-router-dom` v6中引入，用于替代v5中的 `Switch`组件。它负责根据当前的URL决定哪一个子 `Route`组件应该被渲染。`Routes`组件会选择与当前URL匹配的最佳 `Route`来渲染，并提供了嵌套路由的支持。

**Route**

* **作用**：`Route`组件用于在路由系统中定义单个路由规则。它接受一个 `path`属性，用于指定路由的匹配路径，以及一个 `element`属性，用于指定当该路由匹配时应该渲染的组件。

**这三个组件通常一起使用来构建React应用的路由系统。**`BrowserRouter`提供了一个高阶的路由容器，`Routes`用于管理路由的选择逻辑，而 `Route`用于定义具体的路由规则和对应渲染的组件。没有 `BrowserRouter`，路由系统将无法工作，因为它提供了路由的上下文；没有 `Routes`和 `Route`，你就无法定义和管理你的路由规则。这种模式允许React开发者以一种声明式的方式来组织和管理用户在应用中的导航路径，同时确保应用的UI与URL保持同步，从而提高用户体验和应用的可维护性。

```
<Router>
  <Routes>
    {/* 路由配置 */}
    <Route path="/" element={<HomePage />} />
    <Route path="/about" element={<AboutPage />} />
  </Routes>
</Router>
```
