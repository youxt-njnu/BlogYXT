
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

## react-vertical-timeline-component

`react-vertical-timeline-component` 是一个React库，用于创建和显示垂直时间线。这个库提供了一种简单而有效的方式来展示按时间顺序排列的事件或步骤，非常适合用于展示项目里程碑、历史事件、工作经历等。它基于React开发，因此可以轻松地集成到现有的React应用中。

### 核心特性

- **易于使用**：通过提供的组件和属性，开发者可以快速构建出美观、响应式的时间线。
- **高度可定制**：支持自定义颜色、图标和内容，使时间线能够匹配应用的风格和主题。
- **响应式设计**：时间线会自动适应不同屏幕尺寸，保证在移动设备和桌面设备上都能良好展示。
- **动画效果**：内置动画效果，为时间线的展示增添视觉吸引力。

### 基本使用

要开始使用 `react-vertical-timeline-component`，首先需要将它添加到你的React项目中：

```bash
npm install react-vertical-timeline-component
# 或
yarn add react-vertical-timeline-component
```

接下来，你可以在你的组件中引入并使用时间线组件及其相关的子组件。以下是一个简单的示例：

```jsx
import React from 'react';
import {
  VerticalTimeline,
  VerticalTimelineElement,
} from 'react-vertical-timeline-component';
import 'react-vertical-timeline-component/style.min.css'; // 引入样式文件

const MyTimeline = () => (
  <VerticalTimeline>
    <VerticalTimelineElement
      className="vertical-timeline-element--work"
      date="2011 - present"
      iconStyle={{ background: 'rgb(33, 150, 243)', color: '#fff' }}
      // icon={<WorkIcon />}
    >
      <h3 className="vertical-timeline-element-title">Creative Director</h3>
      <h4 className="vertical-timeline-element-subtitle">Miami, FL</h4>
      <p>
        Creative Direction, User Experience, Visual Design, Project Management, Team Leading
      </p>
    </VerticalTimelineElement>
    {/* 可以添加更多的 VerticalTimelineElement 组件来展示其他事件 */}
  </VerticalTimeline>
);

export default MyTimeline;
```

在这个例子中，`VerticalTimeline`是容纳所有时间线元素的容器，而 `VerticalTimelineElement`代表时间线上的单个事件或里程碑。你可以通过传递不同的props来定制每个时间线元素的外观和内容。

### 自定义和样式

`react-vertical-timeline-component`提供了多种方式来定制时间线的样式和行为：

- **颜色和图标**：通过 `iconStyle`属性自定义图标样式，以及通过 `icon`属性添加自定义图标。
- **内容布局**：每个 `VerticalTimelineElement`可以包含标题、副标题和任意的HTML内容，允许灵活地展示信息。
- **自定义类名**：可以为时间线元素添加自定义CSS类名，进一步通过CSS来定制样式。

### 总结

`react-vertical-timeline-component`是一个功能丰富且易于使用的React库，它能够帮助开发者在应用中快速创建出美观、响应式的垂直时间线。通过广泛的定制选项，它能够满足多种展示需求，使得时间线既能传达必要的信息，又具有吸引人的视觉效果。

## @emailjs/browser

`@emailjs/browser` 是一个JavaScript库，用于在客户端直接从前端应用中发送电子邮件，无需后端服务器。通过使用 EmailJS 服务，开发者可以在Web页面中集成电子邮件发送功能，而不需要编写服务器端代码或存储用户的电子邮件凭据。这使得在联系表单、通知系统或任何需要发送电子邮件的场景中，实现邮件发送变得异常简单和安全。

### 核心特性

- **无服务器**：不需要自己的服务器来发送电子邮件，减少了开发和维护成本。
- **简单易用**：提供了简洁的API，可以快速在前端代码中集成和使用。
- **安全**：不需要在客户端暴露敏感信息，如SMTP服务器登录凭据等。
- **灵活性**：支持自定义电子邮件模板，可以根据需要发送不同内容的邮件。

### 安装

通过npm或yarn安装 `@emailjs/browser`：

```bash
npm install @emailjs/browser
# 或者
yarn add @emailjs/browser
```

### 基本使用

要使用 `@emailjs/browser`发送电子邮件，首先需要在 EmailJS 网站上注册账户，创建邮件模板，并获取必要的服务ID、模板ID和用户ID。

然后，可以在前端代码中使用这些ID来发送邮件：

```javascript
import emailjs from '@emailjs/browser';

// 在组件加载时初始化 EmailJS 服务（例如，在React组件的useEffect中）
emailjs.init("your-user-id"); // 使用你的User ID替换"your-user-id"

// 创建发送邮件的函数
const sendEmail = () => {
  const templateParams = {
    to_name: '收件人名字',
    from_name: '发件人名字',
    message: '邮件内容',
    // ...其他模板参数
  };

  emailjs.send('your-service-id', 'your-template-id', templateParams)
    .then((response) => {
       console.log('SUCCESS!', response.status, response.text);
    }, (error) => {
       console.log('FAILED...', error);
    });
};
```

### 使用场景

`@emailjs/browser`适用于多种场景，如：

- **联系表单**：在静态网站或SPA中收集用户反馈或查询，直接从前端发送邮件。
- **注册确认**：在用户注册流程中发送欢迎邮件或确认邮件。
- **通知系统**：向用户发送通知邮件，如订单状态更新、活动提醒等。

### 总结

`@emailjs/browser`提供了一种简单而强大的方式，使得在没有后端的情况下直接从浏览器发送电子邮件成为可能。通过减少服务器端的需求和复杂性，开发者可以更专注于用户体验和前端功能的实现，同时保持应用的安全性和可维护性。

## tailwindcss

[官方文档](https://tailwindcss.com/docs/installation)

[15分钟入门](https://segmentfault.com/a/1190000022622923)

[入门教程](http://www.xcj.com/front-end-life/CSS/TailwindCSS.html)

延申插件：

> Typography
>
> Forms
>
> Aspect Ratio
>
> Container Queries
