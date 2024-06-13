---
title: three+react|个人主页实战Ⅰ
tags:
 - threejs
 - react
categories:
 - 3DGIS
 - WebGL
comments: true
date: 2024-02-25 11:23:05
photos: https://wallroom.io/img/3840x2160/bg-81f2d39.jpg
cover: https://wallroom.io/img/3840x2160/bg-81f2d39.jpg
---
# 说明

[模仿项目地址](https://github.com/youxt-njnu/3d_portfolio_primary)，[参考的视频教程出处](https://www.youtube.com/watch?v=FkowOdMjvYo)，[react入门](https://juejin.cn/post/6960262593265025031)

[react three fiber](https://docs.pmnd.rs/react-three-fiber/getting-started/your-first-scene#adding-lights)

# 教程

`npm create vite@latest ./` 在当前目录安装react

`npm install` 安装需要的包

`npm run dev` 运行开发环境

delete src folder, and create a new one called 'src'.

new file 'main.jsx' and code, change 'tsx' to 'jsx' in 'main.html'

new file 'App.jsx' and code ➡️ rafce shortcut(ES7+React... plugin embeded)

open a new TERMINAL then `npm install -D tailwindcss postcss autoprefixer`, `npx tailwindcss init -p`, configure path,

> [documentation](https://tailwindcss.com/docs/installation)
>
> install 'npx' in advance: `npm i -g npx`

new file 'index.css', and import it in 'main.jsx'

copy the downloaded file 'index.css' for your 'index.css'

copy the downloaded file 'tailwind.cofig.js' for your 'tailwind.config.js'

install the plugin 'Tailwind CSS IntelliSense' , then vscode can recognize the css code in html

then restart the server, the result is shown!

install: `npm i react-router-dom`

create folder 'components', and create file 'Navbar.jsx', and code

create folder 'pages' and file 'About.jsx', 'Contact.jsx', 'Home.jsx', 'Projects.jsx'

create index.js to import all the following, then export for use

`npm i @react-three/fiber`

`npm i @react-three/drei`

download foxs_islands.glb from sketchfab, use [this website](https://gltf.pmnd.rs/) to simplize and transfer it.

> use model offered by author directly
>
> 可以找一些教程，来使用这个网站对模型进行简化。

create folder 'models' and file 'Island.jsx', then copy and paste the model info

> `npm i @react-spring/three`
>
> 修改成能导出的模型：
>
> ```
> import { useRef, useEffect } from "react";
> import { useGLTF } from "@react-three/drei";
> import { useFrame, useThree } from "@react-three/fiber";
> import isLandScene from '../assets/3d/island.glb'
> import { a } from '@react-spring/three'
>
> const { nodes, materials } = useGLTF(isLandScene);
> const Island = (props) => { ..return(<a.group{}..)..}
>
>
> export default Island;
> ```

删除：

> dispose={null}
>
> castShadow
>
> receiveShadow
>
> useGLTF.preload("/island.glb");

修改：

> <a.group ref={islandRef} {...props}>

in vite.config.js, modify:

```
export default defineConfig({
  plugins: [react()],
  assetsInclude: ['**/*.glb']
})
```

给light添加属性的时候，会提示“unknown property 'position' found“，这个时候需要在.eslintrc.cjs里添加：

`ignorePatterns: ['dist', '.eslintrc.cjs', 'src'],`

加载上各类模型，然后尝试实现拖动旋转和飞行 ➡️ 这个效果就是这个的[初级版](https://www.joshuas.world/)

GitHub Copilot的使用，免费1个月的订阅，也认证了[学生身份](https://education.github.com/discount_requests/application)（可以免费使用）

报错提示Island.jsx:109  Uncaught ReferenceError: setCurrentStage is not defined

在jsx文件内部定义的节点，需要注意返回return，外面的才能使用节点定义的结构；

```
npm i @emailjs/browser
```

在[emailjs官网](https://dashboard.emailjs.com/admin/templates/0hhd7hu/settings)上，creat free account

add new service → Service ID → connect account → create service

Email Template → Save → Settings, Template ID

把这些参数放到新的文件.env.local里面

错误：message栏总是输入无响应，

```js
<textarea
              name="message" 
              rows='4'
              className='textarea'
              placeholder='Let me know how I can help you!'
              required
              value={form.message}
              onChange={handleChange}
              onFocus={handleFocus}
              onBlur={handleBlur}
            />
// 是name="message",而不是type="message"
// row='4' 而不是row={4}
```

对于fox的模型，可以去gltf.pmnd.rs网站，拖入后，把代码复制到Fox.jsx里进行调整（这个网站实现了GLTF -> React Three Fiber）

之后记得修改，不然会报错 `Contact.jsx:5 Uncaught SyntaxError: The requested module '/src/models/Fox.jsx' does not provide an export named 'default' (at Contact.jsx:5:8)`

导入images文件夹，以及构建constants->index.js，复制粘贴进去内容；可以针对自己情况进行修改；

为了实现时间线的功能，需要 `npm i react-vertical-timeline-component`

参考案例进行模仿使用；

最后作者完成了部署deployment，通过hostinger网站
