---
title: three+react|个人主页实战Ⅱ(补)
tags:
 - WebGL
 - 3DGIS
categories:
 - 3DGIS
comments: true
date: 2024-03-09 11:34:05
photos: https://wallroom.io/img/3840x2160/bg-81f2d39.jpg
cover: https://wallroom.io/img/3840x2160/bg-81f2d39.jpg
---
# 说明

[模仿项目地址](https://github.com/youxt-njnu/3d_portfolio_primary)，[参考的视频教程出处](https://www.youtube.com/watch?v=FkowOdMjvYo)，[react入门](https://juejin.cn/post/6960262593265025031)

[react three fiber](https://docs.pmnd.rs/react-three-fiber/getting-started/your-first-scene#adding-lights)

ChatGPT辅助生成❗自辨❗❗❗


# 相关库补充

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

* **无服务器**：不需要自己的服务器来发送电子邮件，减少了开发和维护成本。
* **简单易用**：提供了简洁的API，可以快速在前端代码中集成和使用。
* **安全**：不需要在客户端暴露敏感信息，如SMTP服务器登录凭据等。
* **灵活性**：支持自定义电子邮件模板，可以根据需要发送不同内容的邮件。

### 安装

**通过npm或yarn安装 **`@emailjs/browser`：

```
npm install @emailjs/browser
# 或者
yarn add @emailjs/browser
```

### 基本使用

**要使用 **`@emailjs/browser`发送电子邮件，首先需要在 EmailJS 网站上注册账户，创建邮件模板，并获取必要的服务ID、模板ID和用户ID。

**在React项目中使用 **`emailjs`或任何其他依赖于环境变量的服务时，创建一个 `.env.local`文件的主要目的是为了安全和配置的灵活性。这个文件被用来存储敏感信息和项目配置，如API密钥、服务ID、用户ID等，这些信息对于应用的运行至关重要，但你不希望它们被直接硬编码到源代码中或被提交到版本控制系统（如Git）中。这样做主要有以下几个原因：

1. **安全性**：保护你的敏感信息不被公开，如API密钥和密码。如果这些信息被硬编码在应用程序的源代码中，并且源代码被上传到了公开的代码仓库，那么这些敏感信息就有泄露的风险。
2. **配置的灵活性**：允许你在不同的环境（开发、测试、生产等）中使用不同的配置，而不需要改变代码。比如，在开发环境和生产环境中使用不同的API密钥。
3. **便于维护**：将配置从代码中分离，使得配置的更改更加容易，而不需要每次都触摸代码本身。

`.env.local`是一种特殊的 `.env`文件，它被设计用来在你的本地开发环境中覆盖 `.env`文件中的默认设置。React和许多其他现代前端框架都支持使用 `.env`文件来定义环境变量。这些变量在构建过程中会被嵌入到最终的JavaScript包中，因此在运行时可以访问它们。

**要在React项目中使用 **`.env.local`中定义的环境变量，你需要遵循以下步骤：

1. **在项目根目录下创建一个 **`.env.local`文件。
2. **在 **`.env.local`文件中添加环境变量，遵循 `REACT_APP_`前缀的规则。例如：
   ```
   REACT_APP_EMAILJS_USER_ID=你的用户ID
   REACT_APP_EMAILJS_SERVICE_ID=你的服务ID
   REACT_APP_EMAILJS_TEMPLATE_ID=你的模板ID
   ```
3. **在你的React应用中，你可以通过 **`process.env.REACT_APP_EMAILJS_USER_ID`这样的方式访问这些变量。

**使用 **`.env.local`文件是一种实践，旨在提高项目的安全性和配置的灵活性，特别是当涉及到处理敏感信息时。这种方法有助于避免将敏感数据暴露给公众，同时还提供了在不同环境下轻松切换配置的能力。

```
VITE_APP_EMAILJS_SERVICE_ID=service_5bezaam
VITE_APP_EMAILJS_TEMPLATE_ID=template_dafnzfi
VITE_APP_EMAILJS_PUBLIC_KEY=JpbkxBEdlS8faxCrq
```

**在使用Vite作为构建工具的项目中，环境变量的命名遵循特定的前缀规则，这是为了确保这些变量在构建时被正确地加载和注入到项目中。**

1. **`VITE_`前缀**：Vite要求所有应该暴露给项目前端代码的环境变量都必须以 `VITE_`为前缀。这是一个约定，用于保护你的环境变量，确保只有带有这个前缀的变量才会被包含在最终的前端构建中。这样可以防止无意间将敏感的服务器端环境变量暴露给客户端。
2. **`APP`**：这个部分不是必需的，但它是一个常见的做法，用于指示这些环境变量是应用级别的。加上 `APP`（或者其他类似的标识符）有助于在环境变量中创建一个逻辑上的分组，使得它们更易于管理和识别。
3. **`EMAILJS_SERVICE_ID`、`EMAILJS_TEMPLATE_ID`、`EMAILJS_PUBLIC_KEY`**：这些部分具体指明了变量的用途。在这个例子中，它们分别用于标识EmailJS服务的ID、模板的ID和公共密钥。这样的命名方法有助于清晰地表达每个环境变量的作用和关联的服务。

**你可以修改环境变量的名称，但需要保持 **`VITE_`前缀不变，以确保Vite能够正确地处理这些变量。如果你决定更改变量名称（比如，将 `VITE_APP_EMAILJS_PUBLIC_KEY`更改为 `VITE_APP_EMAILJS_KEY`），你需要在项目中引用这些环境变量的地方也做相应的更改，以确保一致性和正确的变量访问。

**然后，可以在前端代码中使用这些ID来发送邮件：**

```
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

* **联系表单**：在静态网站或SPA中收集用户反馈或查询，直接从前端发送邮件。
* **注册确认**：在用户注册流程中发送欢迎邮件或确认邮件。
* **通知系统**：向用户发送通知邮件，如订单状态更新、活动提醒等。

### 总结

`@emailjs/browser`提供了一种简单而强大的方式，使得在没有后端的情况下直接从浏览器发送电子邮件成为可能。通过减少服务器端的需求和复杂性，开发者可以更专注于用户体验和前端功能的实现，同时保持应用的安全性和可维护性。

## tailwindcss

[官方文档](https://tailwindcss.com/docs/installation)

[15分钟入门](https://segmentfault.com/a/1190000022622923)

[入门教程](http://www.xcj.com/front-end-life/CSS/TailwindCSS.html)

**延申插件：**

> **Typography**
>
> **Forms**
>
> **Aspect Ratio**
>
> **Container Queries**

### 安装

**安装和配置Tailwind CSS主要涉及两个步骤，这两个步骤通过上述命令完成。下面是对这两条命令的解释：**

1. `npm install -D tailwindcss postcss autoprefixer`

**这条命令使用npm（Node.js包管理器）来安装三个开发依赖：**

* **`tailwindcss`**：Tailwind CSS库本身，它是一个功能类优先的CSS框架，允许你通过在HTML中添加类来直接应用样式，从而加快开发速度。
* **`postcss`**：PostCSS是一个用JavaScript工具和插件转换CSS代码的工具。Tailwind CSS依赖于PostCSS，因为它实际上是一组PostCSS插件，用于生成和处理Tailwind的工具类。
* **`autoprefixer`**：Autoprefixer是一个PostCSS插件，用于自动添加浏览器厂商前缀到CSS规则中，确保CSS属性在不同的浏览器中能够正常工作。这对于兼容性是非常有用的。

`-D`标志表示这些包被安装为开发依赖项，这意味着它们只在开发过程中需要，在生产环境的构建过程中不会被使用。

2. `npx tailwindcss init -p`

**这条命令用于初始化Tailwind CSS的配置文件，并自动生成PostCSS配置文件。**

* `npx`是一个npm包运行器，它允许你执行安装在本地node_modules目录中的包而不需要全局安装这些包。
* `tailwindcss init`是Tailwind CLI的一部分，用于生成 `tailwind.config.js`文件。这个文件是Tailwind CSS的配置文件，你可以在其中自定义你的设计系统，如颜色、字体大小、间距等。
* `-p`标志表示同时生成 `postcss.config.js`文件，这是PostCSS的配置文件，Tailwind CSS作为PostCSS插件运行时需要这个文件。这个标志确保了 `autoprefixer`也被包含在PostCSS配置中，因为它通常与Tailwind CSS一起使用以确保最佳的浏览器兼容性。

**总的来说，这两个命令共同完成了Tailwind CSS和其依赖的安装，以及为项目生成必要的配置文件，让你可以开始使用Tailwind CSS来构建项目。**

### 配置

**在 **`tailwind.config.js`文件中，`content`、`theme`、和 `plugins`是Tailwind CSS配置的主要部分，它们各自承担着不同的角色，以便定制和控制Tailwind CSS的行为。下面是它们各自的作用：

1. `content`

`content`属性用于指定Tailwind CSS应该扫描哪些文件来寻找类名。这是Tailwind CSS的一个重要特性，称为PurgeCSS（在Tailwind CSS v3.x中，这一特性已经内置且默认启用），它用于移除最终CSS文件中未使用的样式，以减小文件大小和提高加载速度。

**在 **`tailwind.config.js`文件中，你可以这样配置 `content`属性：

```
module.exports = {
  content: ['./src/**/*.{html,js,jsx,ts,tsx}'],
  // 其他配置...
};
```

**这告诉Tailwind CSS扫描你项目中的 **`src`目录下所有的HTML和JavaScript文件，包括React（`.jsx`）、TypeScript（`.ts`、`.tsx`）等文件，来确定哪些样式是实际被使用的。

2. `theme`

`theme`属性用于定制Tailwind CSS提供的默认设计系统。你可以在这里调整颜色、字体、间距等默认设置，或者添加自己的设计令牌（tokens）。这使得Tailwind CSS极其灵活，能够适应几乎任何设计需求。

**例如，你可以定制主题颜色和字体大小：**

```
module.exports = {
  // 其他配置...
  theme: {
    extend: {
      colors: {
        'custom-blue': '#5b6d5b',
      },
      fontSize: {
        'big': '2rem',
      },
    },
  },
  // 其他配置...
};
```

`extend`属性用于扩展而不是覆盖默认的主题设置。

**在 **`tailwind.config.js`文件的 `extend`对象中对 `colors`的设置允许你自定义或扩展默认的颜色系统。这种方式让你可以添加新的颜色或覆盖现有颜色的某些阶级（如100-900这样的权重级别通常用于表示颜色的浅到深）。这里是对给出的设置的详细解释：

```
colors: {
  black: {
    DEFAULT: '#000',
    500: '#1D2235',
  }
},
```

**`black`**：

* `DEFAULT`：这里指定了 `black`颜色的默认值为 `#000`。在Tailwind CSS中，如果你直接使用 `text-black`或 `bg-black`这样的类，将会应用这个默认颜色值。
* `500`：除了默认值之外，还定义了一个 `500`阶级的黑色 `black-500`为 `#1D2235`。这意味着你可以通过 `text-black-500`或 `bg-black-500`来使用这个特定的深度值。

3. `plugins`

`plugins`属性允许你添加第三方插件或自定义的插件到Tailwind CSS中，以扩展其功能。这些插件可以添加新的工具类、组件，或是在构建过程中使用的功能性钩子。

**你可以这样添加插件：**

```
module.exports = {
  // 其他配置...
  plugins: [
    require('@tailwindcss/forms'),
    // 其他插件...
  ],
};
```

**这个例子中，我们添加了 **`@tailwindcss/forms`插件，它是一个官方插件，用于重置和定制表单元素的样式。

**总的来说，**`tailwind.config.js`中的 `content`、`theme`、和 `plugins`配置项提供了一个强大的机制来定制和优化你的Tailwind CSS集成，使其更适合你的项目需求。

### 导入

**在Tailwind CSS中，使用 **`@tailwind`指令在 `index.css`（或任何主CSS文件）中导入Tailwind的层是初始化和使用Tailwind CSS框架的关键步骤。这些指令告诉Tailwind CSS在构建过程中将其功能类和样式注入到CSS文件中。下面是这三个指令的作用：

1. `@tailwind base;`

**这个指令导入Tailwind的基础样式，这些样式包括浏览器样式的重置和归一化以及一些基本的HTML元素样式。这为你的项目提供了一个一致的基线样式，确保在不同浏览器中具有一致的外观和感觉。基础样式还包括对标准元素（如 **`<h1>`到 `<h6>`、`<p>`等）的默认样式设置，这样你可以在不需要额外类的情况下立即开始工作。

2. `@tailwind components;`

**这个指令导入由你或其他人创建的组件类。在Tailwind中，组件不是指UI组件库中的组件，而是一组可以重用的工具类组合，用于构建界面元素。通过在 **`tailwind.config.js`文件中自定义或扩展，你可以创建自定义的组件样式，这些自定义样式将通过这个指令被引入。这允许你定义一些常用的样式模式，如按钮、卡片等，以便在整个项目中重用。

3. `@tailwind utilities;`

**这个指令导入Tailwind的工具类，这是Tailwind CSS最强大的特性之一。工具类包括了边距、填充、文字大小、颜色等几乎所有CSS属性的类。这些类设计为原子类，意味着每个类都具有单一的责任，你可以组合它们来创建复杂的设计。这种方法提供了极高的灵活性和定制性，使得你能够快速构建和调整界面。**

**通过在CSS文件中包含这些 **`@tailwind`指令，你实际上是在激活Tailwind CSS的强大功能，使其成为构建和设计你的Web项目的基础。这种方法允许你利用Tailwind的所有预定义样式和工具，同时保持了扩展和自定义的能力。

### Tailwind CSS IntelliSense

**Tailwind CSS IntelliSense 是一个 Visual Studio Code (VSCode) 插件，它极大地增强了在使用VSCode开发时使用Tailwind CSS的体验。这个插件提供了一系列功能，旨在提高开发效率和减少编码错误。主要功能包括：**

* **类名提示**：在编写HTML或JSX等文件时，**插件会提供Tailwind CSS类名的自动完成建议**。这意味着你开始键入时，它会显示可用的Tailwind 类名列表，让你快速选择而不需要记住每个具体的类名。
* **悬停后可以样式预览**：当你将鼠标悬停在某个Tailwind CSS类名上时，IntelliSense会显示一个小弹窗，其中展示了这个类名对应的CSS样式。这使得理解和检查类名的作用变得直接且方便。
* **错误检查**：插件能够识别并高亮显示不存在的Tailwind CSS类名，帮助你快速定位并修正错误或拼写问题。
* **颜色预览**：对于颜色相关的类名（如背景、文字颜色等），IntelliSense会在类名旁边显示一个小色块，直观地展示这个类名对应的颜色，增强了颜色选择的直观性。
* **@apply指令支持**：当使用 `@apply`指令在CSS或SCSS文件中应用Tailwind CSS实用程序类时，IntelliSense也会提供自动完成和验证功能。
* **自定义配置支持**：IntelliSense插件能够读取并根据你的 `tailwind.config.js`文件提供自定义配置的自动完成建议，这包括你添加或修改的颜色、间距等自定义主题设置。

**总之，Tailwind CSS IntelliSense 插件通过提供强大的代码完成、验证和预览功能，极大地提高了使用Tailwind CSS进行开发的效率和舒适度。它帮助开发者更快地编写代码，减少错误，并在编码时获得即时的样式反馈。**
