---
title: three+react|个人主页实战Ⅳ
tags:
 - WebGL
 - 3DGIS
categories:
 - 3DGIS
comments: true
date: 2024-03-09 11:26:05
photos: https://wallroom.io/img/3840x2160/bg-81f2d39.jpg
cover: https://wallroom.io/img/3840x2160/bg-81f2d39.jpg
---
# 说明

[模仿项目地址](https://github.com/youxt-njnu/3d_portfolio_primary)，[参考的视频教程出处](https://www.youtube.com/watch?v=FkowOdMjvYo)，[react入门](https://juejin.cn/post/6960262593265025031)

[react three fiber](https://docs.pmnd.rs/react-three-fiber/getting-started/your-first-scene#adding-lights)

本文是对Island.jsx及其相关文件的学习解读，内容中含ChatGPT辅助生成❗自辨❗❗❗

# 相关案例补充

## Bird案例

### const = useGLTF(birdScene)

`<primitive />` is a component from React Three Fiber that allows you to directly include three.js objects into the React component tree. By setting the `object` prop to `scene`, you're telling React Three Fiber to render the entire GLTF scene, which `useGLTF` hook returns, without having to manually construct the scene with React components. This is a straightforward way to include complex 3D models and their associated hierarchies into your 3D scene.

```
//这种形式
<mesh
   geometry={nodes.pCube11_rocks1_0.geometry}
   material={materials.PaletteMaterial001}
/>
//和这种形式
<mesh
   ref={birdRef}
   position={[-5, 2, 1]}
   scale={[0.003, 0.003, 0.003]}
>
   <primitive object={scene} />
</mesh>
```

**第一种形式直接使用了GLTF模型中的具体节点（如 **`pCube11_rocks1_0`）和材质（如 `PaletteMaterial001`）来创建一个 `<mesh>`。这种方式允许你精细控制模型的每个部分，例如指定使用模型中的哪个节点和哪种材质。

**第二种形式使用了 **`<primitive object={scene} />`来直接渲染整个场景（`scene`），这里的 `scene`是 `useGLTF`钩子返回的整个GLTF场景对象。通过这种方式，你可以简单快捷地在React Three Fiber中渲染整个3D模型，而不需要逐个指定模型的每个部分。这种方法适用于当你想要原封不动地渲染整个模型，而不需要对模型的单独部分进行精细控制。

### Unity中导入glb/gltf

**在Unity中导入GLB模型，可以使用Unity的GLTFUtility插件，支持导入GLB和GLTF格式的3D模型。首先需要从**[GitHub获取URL](https://github.com/Siccity/GLTFUtility)，然后在Unity中使用package manager来进行安装。导入插件后，你可以直接将GLB模型拖拽到Unity的Assets文件夹中，插件会自动处理模型的导入过程。这个过程简单快速，不需要复杂的配置。

### useFrame()

**在 **`useFrame`回调函数中，`{ clock, camera }`是从React Three Fiber的渲染循环中解构出来的对象。`clock`是一个 `THREE.Clock`实例，用于追踪时间；`camera`是当前场景中的相机对象。这种写法允许你直接访问这些对象而不需要从外部传入，因为 `useFrame`已经为你提供了对它们的引用，这是React Three Fiber框架的一部分，旨在简化动画和渲染逻辑的实现。

```
useFrame(({ clock, camera }) => { // 这里需要给clock, camera外面加上{}，不然会报错
    // Update the Y position simulate the flight moving in a sin wave 
    birdRef.current.position.y = Math.sin(clock.elapsedTime) * 0.2 + 2;

    // 控制鸟在island的范围内（相机位置的前后10个单位）
    if (birdRef.current.position.x > camera.position.x + 10) {
      birdRef.current.rotation.y = Math.PI;
    } else if (birdRef.current.position.x < camera.position.x - 10) {
      birdRef.current.rotation.y = 0;
    }


```

**在 **`useFrame`中，除了 `clock`和 `camera`，你还可以接收到一个 `state`对象作为参数，它包含了React Three Fiber渲染循环中的当前状态和一些实用的属性和方法。这些包括场景（`scene`）、渲染器（`gl`）、大小（`size`）、鼠标位置（`mouse`）等。`useFrame`函数提供了一个强大的接口，让你可以在每一帧中访问和修改这些对象，从而创建动画和交互。

```
  useFrame((_, delta) => {
    if (isRotating) {
      skyRef.current.rotation.y += 0.15 * delta;
    }
  });
```

**In the **`useFrame` hook, the first parameter (`_` in this case) is the state object provided by React Three Fiber, which contains information about the current render state, such as the scene, camera, etc. The underscore `_` is often used to indicate that the parameter is not being used in the function. The second parameter, `delta`, represents the time in seconds since the last frame was rendered. This is useful for creating time-dependent animations or simulations, ensuring smooth and consistent motion regardless of the frame rate.

## Home

### Audio的使用

```
const audioRef = useRef(new Audio(sakura));
```

**The **`Audio` in refers to the HTML Audio element, which is a standard part of the Web APIs provided by browsers for playing sound. It does not need to be imported from a module because it's globally available in the browser environment, similar to `document` or `window`. This API allows you to programmatically control audio playback, such as play, pause, and volume control directly from your React components.

**To use the **`Audio` interface in JavaScript, you simply create a new `Audio` object, optionally passing the URL of the audio file you wish to play as a parameter. You can then use methods like `play()`, `pause()`, and properties such as `volume` to control playback.

**Example:**

```
const audio = new Audio('path/to/your/audio/file.mp3');
audio.play(); // To start playing the audio
audio.pause(); // To pause the audio
audio.volume = 0.5; // To set the volume to 50%
```

**In a React component, you might use it with **`useRef` to keep a reference to the audio object.

```
const [isPlayingMusic, setIsPlayingMusic] = useState(false);

useEffect(() => {
    if (isPlayingMusic) { audioRef.current.play(); }

    return () => {
      audioRef.current.pause();
    }
}, [isPlayingMusic]);

// return里面：
<div className='absolute bottom-2 left-2'>
        <img
          src={!isPlayingMusic ? soundoff : soundon}
          alt="sound"
          className='w-10 h-10 cursor-pointer object-contain'
          onClick={() => setIsPlayingMusic(!isPlayingMusic)}
        />
</div>
```

**This component plays the audio when it mounts and pauses when it unmounts, with the **`src` prop specifying the audio file's path.

### currentStage的切换

```
<div className="absolute top-28 left-0 right-0 z-10 flex items-center justify-center">
        {currentStage && <HomeInfo currentStage={currentStage} />} </div>}
```

**HomeInfo中**

```
const HomeInfo = ({ currentStage }) => {
  return renderContent[currentStage] || null;
};
```

**Island中**

```
switch (true) {
        case normalizeRotation >= 5.45 && normalizeRotation <= 5.85:
          setCurrentStage(4);
          break;
        case normalizeRotation >= 0.85 && normalizeRotation <= 1.3:
          setCurrentStage(3);
          break;
        case normalizeRotation >= 2.4 && normalizeRotation <= 2.6:
          setCurrentStage(2);
          break;
        case normalizeRotation >= 4.25 && normalizeRotation <= 4.75:
          setCurrentStage(1);
          break;
        default:
          setCurrentStage(null); // 当不在特定角度下，currentStage为null，所以Home.jsx里要对currentStage进行判断
      }
```

**所以需要先判断currentStage，不是null之后再渲染HomeInfo**

## Components

### Loader

```
<div className='flex justify-center items-center'>
        <div className="w-20 h-20 border-4 border-opacity-20 border-purple-500 border-t-purple-800 rounded-full animate-spin"></div>
      </div>
```

**这段代码是一个使用Tailwind CSS实现的简单旋转动画示例。它创建了一个居中的圆形div，该div具有旋转动画。下面是详细解析：**

* `className='flex justify-center items-center'`：这三个类一起使用，创建了一个flex容器，其中的内容水平（`justify-center`）和垂直（`items-center`）居中。这确保了内部的div（旋转元素）在父容器中居中。
* **`w-20 h-20`**：设置div的宽度和高度为5rem（根据Tailwind CSS的默认配置，`20`单位等于5rem，如果没有自定义配置的话）。
* **`border-4`**：设置边框宽度为4像素。
* **`border-opacity-20`**：设置边框透明度为20%，使边框颜色较浅。
* **`border-purple-500`**：设置边框的默认颜色为紫色，透明度由 `border-opacity-20`控制。
* **`border-t-purple-800`**：特别为上边框设置了较深的紫色（`purple-800`），这在旋转时会产生视觉效果，使得看起来像是在旋转。
* **`rounded-full`**：使div变为圆形。
* **`animate-spin`**：应用Tailwind CSS内置的旋转动画，使div无限期旋转。这个动画是通过CSS的 `@keyframes`实现的，定义了从0%到100%的转动，实现了连续旋转的效果。Tailwind CSS内部定义了这个动画关键帧，实现了元素的360度旋转。动画是循环播放的，因此旋转动画会一直持续，直到被另外的CSS规则覆盖或者从DOM中移除。

### 匿名函数的()和{}

* **使用 **`()=>()`时，箭头函数会直接返回圆括号 `()`中的表达式结果。这适用于返回单个表达式或JSX元素的简短函数。
* **使用 **`()=>{}`时，你可以在花括号 `{}`中执行多个语句，但如果想要返回值，需要显式使用 `return`语句。这适用于更复杂的函数，其中包含了多条语句。

## Contact

### useAnimations

```
const group = useRef();
  const { nodes, materials, animations } = useGLTF(scene);
  const { actions } = useAnimations(animations, group);

  useEffect(() => {
    console.log(actions);// 可以查看模型有哪些动画
    Object.values(actions).forEach((action) => { action.stop() });
    if (actions[currentAnimation]) actions[currentAnimation].play();

  }, [actions, currentAnimation]);
```

`Object.values()` 是一个JavaScript方法，它从一个对象中提取出所有可枚举的属性值，并以数组的形式返回这些值。这个方法让你能够更方便地遍历对象的值。

**使用方法:**

```
const object = { a: 1, b: 'text', c: true };
const values = Object.values(object);
console.log(values); // 输出: [1, 'text', true]
```

**案例:**
**假设你有一个对象，存储了不同动画的状态，你想要停止所有正在播放的动画，可以这样做:**

```
const animations = {
  walk: { /* 动画对象 */ },
  run: { /* 动画对象 */ }
};

// 停止所有动画
Object.values(animations).forEach(animation => {
  animation.stop(); // 假设每个动画对象都有一个stop方法
});
```

**这样，**`Object.values()`帮助你获取了 `animations`对象中所有动画的集合，然后你可以使用 `forEach`遍历这个集合，并对每个动画调用 `stop`方法。

### useAlert

**新建的一个自定义Hook;**

```
import React from 'react'

const useAlert = () => {
  const [alert, setAlert] = React.useState({
    show: false,
    text: '',
    type: 'danger',
  })
  const showAlert = ({ text, type = 'danger' }) =>
    setAlert({ show: true, text, type })
  const hideAlert = () => setAlert({ show: false, text: '', type: 'danger' })
  return { alert, showAlert, hideAlert }
}

export default useAlert
```

**在useState的基础上，新建了两个函数，这两个函数都是传入参数+setAlert进行组合；最后返回alert，升级的setAlert(也就是这两个函数)**
