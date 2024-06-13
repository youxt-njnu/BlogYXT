---
title: WebAPIs 知识点梳理
date: 2024-06-13 09:33:26
tags: 
  - JS
  - 基础知识
categories: 
  - 大前端
photos: https://static-ali.ihansen.org/app/bg1440/ichB3SM6v7Y.jpg!o
cover: https://static-ali.ihansen.org/app/bg1440/ichB3SM6v7Y.jpg!o
---
# JS基础

> 整理自黑马Pink前端的课程资料；
> 算是入门第一步；
> 后续有时间的话，再看看书，多了解了解

# Web APIs

1. **API 是为我们程序员提供的一个接口，帮助我们实现某种功能，我们会使用就可以了，不必纠结内部如何实现**
2. **Web API 主要是针对于浏览器提供的接口，主要针对于浏览器做交互效果。**
3. **Web API 一般都有输入和输出（ 函数的传参和返回值），Web API 很多都是方法（函数）**
4. **学习 Web API 可以结合前面学习内置对象方法的思路学习**

## 1.2. DOM 介绍

### 1.2.1 什么是DOM

**文档对象模型（Document Object Model，简称DOM），是 **[W3C](https://baike.baidu.com/item/W3C) 组织推荐的处理[可扩展标记语言](https://baike.baidu.com/item/%E5%8F%AF%E6%89%A9%E5%B1%95%E7%BD%AE%E6%A0%87%E8%AF%AD%E8%A8%80)（html或者xhtml）的标准[编程接口](https://baike.baidu.com/item/%E7%BC%96%E7%A8%8B%E6%8E%A5%E5%8F%A3)。

**W3C 已经定义了一系列的 DOM 接口，通过这些 DOM 接口可以改变网页的内容、结构和样式。**

> **DOM是W3C组织制定的一套处理 html和xml文档的规范，所有的浏览器都遵循了这套标准。**

### 1.2.2. DOM树

![](https://s2.loli.net/2024/06/13/bIQ3ek6qtW9CFBm.png)

**DOM树又称为文档树模型，把文档映射成树形结构，通过节点对象对其处理，处理的结果可以加入到当前的页面。**

* **文档：一个页面就是一个文档，DOM中使用document表示**
* **节点：网页中的所有内容，在文档树中都是节点（标签、属性、文本、注释等），使用node表示**
* **标签节点：网页中的所有标签，通常称为元素节点，又简称为“元素”，使用element表示**

**DOM把以上内容都看成对象；**

## 1.3. 获取元素

**为什么要获取页面元素？**

**例如：我们想要操作页面上的某部分(显示/隐藏，动画)，需要先获取到该部分对应的元素，再对其进行操作。**

### 1.3.1. 根据ID获取

```
语法：document.getElementById(id)
作用：根据ID获取元素对象
参数：id值，区分大小写的字符串
返回值：元素对象 或 null
```

**案例代码**

```
<body>
    <div id="time">2019-9-9</div>
    <script>
        // 因为我们文档页面从上往下加载，所以先得有标签 所以我们script写到标签的下面
        var timer = document.getElementById('time');
        console.log(timer);
        console.log(typeof timer);
        // console.dir 打印我们返回的元素对象 更好的查看里面的属性和方法
        console.dir(timer);
    </script>
</body>
```

![](https://s2.loli.net/2024/06/13/srPCptxVckEZMWQ.png)

### 1.3.2. 根据标签名获取元素

```
语法：document.getElementsByTagName('标签名') 或者 element.getElementsByTagName('标签名') 
作用：根据标签名获取元素对象
参数：标签名
返回值：元素对象集合（伪数组，数组元素是元素对象）
```

**案例代码**

```
<body>
    <ul>
        <li>知否知否，应是等你好久11</li>
        <li>知否知否，应是等你好久22</li>
        <li>知否知否，应是等你好久33</li>
        <li>知否知否，应是等你好久44</li>
        <li>知否知否，应是等你好久55</li>
    </ul>
    <ul id="nav">
        <li>生僻字</li>
        <li>生僻字</li>
        <li>生僻字</li>
        <li>生僻字</li>
        <li>生僻字</li>
    </ul>
    <script>
        // 1.返回的是 获取过来元素对象的集合 以伪数组的形式存储的
        var lis = document.getElementsByTagName('li');
        console.log(lis);
        console.log(lis[0]);
        // 2. 我们想要依次打印里面的元素对象我们可以采取遍历的方式
        for (var i = 0; i < lis.length; i++) {
            console.log(lis[i]);
        }
        // 3. element.getElementsByTagName()  可以得到这个元素里面的某些标签
        var nav = document.getElementById('nav'); // 这个获得nav 元素
        var navLis = nav.getElementsByTagName('li');
        console.log(navLis);
    </script>
</body>
```

**注意：**
**1.因为得到的是一个对象的集合，所以我们想要操作里面的元素就需要遍历**
**2.得到元素对象是动态的**

**注意：getElementsByTagName()获取到是动态集合，即：当页面增加了标签，这个集合中也就增加了元素。**

### 1.3.3. H5新增获取元素方式

**1.document.getElementsByclassName('类名');//根据类名返回元素对象集合**
**2.document.uerySelector('选择器)；//根据指定选择器返回第一个元素对象**
**3.document.querySelectorAll('选择器'); //根据指定选择器返回**
**注意：**
**querySelector和querySelectorAll里面的选择器需要加符号，比如：document.querySelector('#nav');**

**案例代码**

```
<body>
    <div class="box">盒子1</div>
    <div class="box">盒子2</div>
    <div id="nav">
        <ul>
            <li>首页</li>
            <li>产品</li>
        </ul>
    </div>
    <script>
        // 1. getElementsByClassName 根据类名获得某些元素集合
        var boxs = document.getElementsByClassName('box');
        console.log(boxs);
        // 2. querySelector 返回指定选择器的第一个元素对象  切记 里面的选择器需要加符号 .box  #nav
        var firstBox = document.querySelector('.box');
        console.log(firstBox);
        var nav = document.querySelector('#nav');
        console.log(nav);
        var li = document.querySelector('li');
        console.log(li);
        // 3. querySelectorAll()返回指定选择器的所有元素对象集合
        var allBox = document.querySelectorAll('.box');
        console.log(allBox);
        var lis = document.querySelectorAll('li');
        console.log(lis);
    </script>
</body>
```

### 1.3.4 获取特殊元素（body，html）

**获取body元素 1.doucumnet.body//返回body元素对象**
**获取html元素 1.document.documentElement//返回html元素对象**

## 1.4. 事件基础

### 1.4.1. 事件概述

**JavaScript 使我们有能力创建动态页面，而事件是可以被 JavaScript 侦测到的行为。**

**简单理解： ****触发 --- 响应机制**。

**网页中的每个元素都可以产生某些可以触发 JavaScript 的事件，例如，我们可以在用户点击某按钮时产生一个事件，然后去执行某些操作。**

### 1.4.2. 事件三要素

* **事件源（谁）：触发事件的元素**
* **事件类型（什么事件）： 例如 click 点击事件**
* **事件处理程序（做啥）：事件触发后要执行的代码(函数形式)，事件处理函数**

**案例代码**

```
<body>
    <button id="btn">唐伯虎</button>
    <script>
        // 点击一个按钮，弹出对话框
        // 1. 事件是有三部分组成  事件源  事件类型  事件处理程序   我们也称为事件三要素
        //(1) 事件源 事件被触发的对象   谁  按钮
        var btn = document.getElementById('btn');
        //(2) 事件类型  如何触发 什么事件 比如鼠标点击(onclick) 还是鼠标经过 还是键盘按下
        //(3) 事件处理程序  通过一个函数赋值的方式 完成
        btn.onclick = function() {
            alert('点秋香');
        }
    </script>
</body>
```

### 1.4.3. 执行事件的步骤

**1.获取事件源**
**2.注册事件（绑定事件）**
**3.添加事件处理程序（采取函数赋值形式）**

**案例代码**

```
<body>
    <div>123</div>
    <script>
        // 执行事件步骤
        // 点击div 控制台输出 我被选中了
        // 1. 获取事件源
        var div = document.querySelector('div');
        // 2.绑定事件 注册事件
        // div.onclick 
        // 3.添加事件处理程序 
        div.onclick = function() {
            console.log('我被选中了');
        }
    </script>
</body>
```

### 1.4.4. 常见的鼠标事件

![](https://s2.loli.net/2024/06/13/DPX2E1hGYSAmIjo.png)

### 1.4.5. 分析事件三要素（见实操案例）

* **下拉菜单三要素**
* **关闭广告三要素**

## 1.5. 操作元素

**    JavaScript的 DOM 操作可以改变网页内容、结构和样式，我们可以利用 DOM 操作元素来改变元素里面的内容、属性等。（注意：这些操作都是通过元素对象的属性实现的）**

### 1.5.1. 改变元素内容（获取或设置）

**element.innerText**
**从起始位置到终止位置的内容，但它去除html标签，同时空格和换行也会去掉**
**element.innerHTML**
**起始位置到终止位置的全部内容，包括html标签，同时保留空格和换行**

**innerText改变元素内容**

```
<body>
    <button>显示当前系统时间</button>
    <div>某个时间</div>
    <p>1123</p>
    <script>
        // 当我们点击了按钮，  div里面的文字会发生变化
        // 1. 获取元素 
        var btn = document.querySelector('button');
        var div = document.querySelector('div');
        // 2.注册事件
        btn.onclick = function() {
            // div.innerText = '2019-6-6';
            div.innerHTML = getDate();
        }
        function getDate() {
            var date = new Date();
            // 我们写一个 2019年 5月 1日 星期三
            var year = date.getFullYear();
            var month = date.getMonth() + 1;
            var dates = date.getDate();
            var arr = ['星期日', '星期一', '星期二', '星期三', '星期四', '星期五', '星期六'];
            var day = date.getDay();
            return '今天是：' + year + '年' + month + '月' + dates + '日 ' + arr[day];
        }
    </script>
</body>
```

**innerText和innerHTML的区别**

* **获取内容时的区别：**

**    innerText会去除空格和换行，而innerHTML会保留空格和换行    **

* **设置内容时的区别：**

**    innerText不会识别html，而innerHTML会识别**

**案例代码**

```
<body>
    <div></div>
    <p>
        我是文字
        <span>123</span>
    </p>
    <script>
        // innerText 和 innerHTML的区别 
        // 1. innerText 不识别html标签 非标准  去除空格和换行
        var div = document.querySelector('div');
        // div.innerText = '<strong>今天是：</strong> 2019';
        // 2. innerHTML 识别html标签 W3C标准 保留空格和换行的
        div.innerHTML = '<strong>今天是：</strong> 2019';
        // 这两个属性是可读写的  可以获取元素里面的内容
        var p = document.querySelector('p');
        console.log(p.innerText);
        console.log(p.innerHTML);
    </script>
</body>
```

### 1.5.2. 常用元素的属性操作

**1.innerText,innerHTML 改变元素内容**
**2.src、href**
**3.id、alt、title**

**获取属性的值**

> **元素对象.属性名**

**设置属性的值**

> **元素对象.属性名 = 值**

**案例代码**

```
<body>
    <button id="ldh">刘德华</button>
    <button id="zxy">张学友</button> <br>
    <img src="images/ldh.jpg" alt="" title="刘德华">
    <script>
        // 修改元素属性  src
        // 1. 获取元素
        var ldh = document.getElementById('ldh');
        var zxy = document.getElementById('zxy');
        var img = document.querySelector('img');
        // 2. 注册事件  处理程序
        zxy.onclick = function() {
            img.src = 'images/zxy.jpg';
            img.title = '张学友思密达';
        }
        ldh.onclick = function() {
            img.src = 'images/ldh.jpg';
            img.title = '刘德华';
        }
    </script>
</body>
```

### 1.5.3. 案例：分时问候

![](https://s2.loli.net/2024/06/13/ki3jF4NTnWQKRpL.png)

### 1.5.4. 表单元素的属性操作

**利用DOM可以操作如下表单元素的属性：**
**type、value、checked、selected、disabled**

**获取属性的值**

> **元素对象.属性名**

**设置属性的值**

> **元素对象.属性名 = 值**
>
> **表单元素中有一些属性如：disabled、checked、selected，元素对象的这些属性的值是布尔型。**

**案例代码**

```
<body>
    <button>按钮</button>
    <input type="text" value="输入内容">
    <script>
        // 1. 获取元素
        var btn = document.querySelector('button');
        var input = document.querySelector('input');
        // 2. 注册事件 处理程序
        btn.onclick = function() {
            // 表单里面的值 文字内容是通过 value 来修改的
            input.value = '被点击了';
            // 如果想要某个表单被禁用 不能再点击 disabled  我们想要这个按钮 button禁用
            // btn.disabled = true;
            this.disabled = true;
            // this 指向的是事件函数的调用者 btn
        }
    </script>
</body>
```

### 1.5.5. 案例：仿京东显示密码

![](https://s2.loli.net/2024/06/13/G9BXQyrReOTxI65.png)

### 1.5.6. 样式属性操作

**我们可以通过 JS 修改元素的大小、颜色、位置等样式。**

**常用方式**

**1.element.style  行内样式操作**
**2.element.className  类名样式操作**

#### 方式1：通过操作style属性

> **元素对象的style属性也是一个对象！**
>
> **元素对象.style.样式属性 = 值;**

**注意：**
**1.JS 里面的样式采取小驼峰命名法比如fontsize、backgroundColor**
**2.JS修改style样式操作，产生的是行内样式，css权重比较高**

**案例代码**

```
<body>
    <div></div>
    <script>
        // 1. 获取元素
        var div = document.querySelector('div');
        // 2. 注册事件 处理程序
        div.onclick = function() {
            // div.style里面的属性 采取驼峰命名法 
            this.style.backgroundColor = 'purple';
            this.style.width = '250px';
        }
    </script>
</body>
```

#### 案例：淘宝点击关闭二维码

![](https://s2.loli.net/2024/06/13/M17o26KwY4Tx9yR.png)

#### 案例：循环精灵图背景

![](https://s2.loli.net/2024/06/13/RHjG1g8Dzhw4kuB.png)

#### 案例：显示隐藏文本框内容

![](https://s2.loli.net/2024/06/13/yQxe7VT3YXpkrU5.png)

#### 方式2：通过操作className属性

> **元素对象.className = 值;**
>
> **因为class是关键字，所有使用className。**

**注意：**
**1,如果样式修改较多，可以采取操作类名方式更改元素样式。**
**2.class因为是个保留字，因此使用className来操作元素类名属性**
**3.className 会直接更改元素的类名，会覆盖原先的类名。**

**案例代码**

```
<body>
    <div class="first">文本</div>
    <script>
        // 1. 使用 element.style 获得修改元素样式  如果样式比较少 或者 功能简单的情况下使用
        var test = document.querySelector('div');
        test.onclick = function() {
            // this.style.backgroundColor = 'purple';
            // this.style.color = '#fff';
            // this.style.fontSize = '25px';
            // this.style.marginTop = '100px';

            // 2. 我们可以通过 修改元素的className更改元素的样式 适合于样式较多或者功能复杂的情况
            // 3. 如果想要保留原先的类名，我们可以这么做 多类名选择器
            // this.className = 'change';
            this.className = 'first change';
        }
    </script>
</body>
```

#### 案例：密码框格式提示错误信息

![](https://s2.loli.net/2024/06/13/6OkmXxy9sSY3bv7.png)

**总结：**

![](https://s2.loli.net/2024/06/13/5KReAgoXG1VIpdZ.png)

## 1.6. H5自定义属性

**自定义属性目的：是为了保存并使用数据。有些数据可以保存到页面中而不用保存到数据库中。**

**自定义属性获取是通过getAttribute(‘属性’) 获取。**

**但是有些自定义属性很容易引起歧义，不容易判断是元素的内置属性还是自定义属性。**

**H5给我们新增了自定义属性：**

**1.设置H5自定义属性**
**H5规定自定义属性data-开头做为属性名并目赋值。**
**比如** `<div data-index="1">`**`</div>`**
**或者使用JS设置**
**element.setAttribute('data-index',2)**

**2.获取H5自定义属性**
**1.兼容性获取element.getAttribute('data-index'):**
**2.H5新增element.dataset.index或者element.dataset['index']   ie11才开始支持**

```
    <div getTime="20" data-index="2" data-list-name="andy"></div>
    <script>
        var div = document.querySelector('div');
        // console.log(div.getTime);
        console.log(div.getAttribute('getTime'));
        div.setAttribute('data-time', 20);
        console.log(div.getAttribute('data-index'));
        console.log(div.getAttribute('data-list-name'));
        // h5新增的获取自定义属性的方法 它只能获取data-开头的
        // dataset 是一个集合里面存放了所有以data开头的自定义属性
        console.log(div.dataset);
        console.log(div.dataset.index);
        console.log(div.dataset['index']);
        // 如果自定义属性里面有多个-链接的单词，我们获取的时候采取 驼峰命名法
        console.log(div.dataset.listName);
        console.log(div.dataset['listName']);//data-list-name
    </script>
```

## 1.7. 节点操作

**一般地，节点至少拥有nodeType（节点类型）、nodeName（节点名称）和nodeValue（节点值）这三个基本属性。**

* **元素节点nodeType为1**
* **属性节点nodeType为2**
* **文本节点nodeType为3(文本节点包含文字、空格、换行等)**

**我们在实际开发中，节点操作主要操作的是元素节点**

### 1.7.1. 节点层级

**    利用 DOM 树可以把节点划分为不同的层级关系，常见的是****父子兄层级关系**。

![](https://s2.loli.net/2024/06/13/Ogtv9nHuEMZqyhA.png)

```
node.parentNode
parentNode.childNodes
parentNode.children
parentNode.firstChild
parentNode.lastChild
parentNode.firstElementChild
parentNode.lastElementChild
parentNode.children[0]
parentNode.children[parentNode.children.length-1]
```

```
node.nextSibling
node.previousSibling
node.nextElementSibling
node.previousElementSibling
```

```
   function getNextElementSibling(element) {
      var el = element;
      while (el = el.nextSibling) {
        if (el.nodeType === 1) {
            return el;
        }
      }//是元素节点，就返回
      return null;//不是元素节点，返回null
    }  
```

### 1.7.2. 节点操作

```
document.createElement('tagName')
node.appendChild(child)
node.insertBefore(child,指定元素)
node.removeChild(child)
node.cloneNode()
```
