---
title: JavaScript基础知识Ⅰ
date: 2023-05-03 20:03:26
tags: 
  - JS
  - 基础知识
categories: 
  - 大前端
photos: https://wallroom.io/img/2560x1600/bg-6d49cc2.png
cover: https://wallroom.io/img/2560x1600/bg-6d49cc2.png
---
# JS基础

> 整理自黑马Pink前端的课程资料；
>
> 算是入门第一步；
>
> 后续有时间的话，再看看书，多了解了解

## emmet语法

Emmet的前身是Zen coding,它使用缩写,来提高html/css的编写速度。

1. 生成标签 直接输入标签名 按tab键即可   比如  div   然后tab 键， 就可以生成 `<div></div>`
2. 如果想要生成多个相同标签  加上 * 就可以了 比如   div*3  就可以快速生成3个div
3. 如果有父子级关系的标签，可以用 >  比如   ul > li就可以了
4. 如果有兄弟关系的标签，用  +  就可以了 比如 div+p
5. 如果生成带有类名或者id名字的，  直接写  .demo  或者  #two   tab 键就可以了
6. 如果生成的div 类名是有顺序的， 可以用 自增符号  $

   ```
   .demo$*3  
   <div class="demo1"></div>
   <div class="demo2"></div>
   <div class="demo3"></div>
   ```

   7. 在css中，可以简写：

   ```
   ti2
   text-indent:2em;
   ta
   text-aligh: ;
   lh30p
   line-height:30px;
   ```

## JS 的组成

![](https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/图片11.png)

1. #### **ECMAScript**

   ECMAScript 是由ECMA 国际（ 原欧洲计算机制造商协会）进行标准化的一门编程语言，这种语言在万维网上应用广泛，它往往被称为 JavaScript或 JScript，但实际上后两者是 ECMAScript 语言的实现和扩展。

   ![](https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/图片12.png)

   ECMAScript：规定了JS的编程语法和基础核心知识，是所有浏览器厂商共同遵守的一套JS语法工业标准。

   更多参看MDN: [MDN手册](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/JavaScript_technologies_overview)
2. #### **DOM——文档对象模型**

   **文档对象模型**（Document Object Model，简称DOM），是W3C组织推荐的处理可扩展标记语言的标准编程接口。通过 DOM 提供的接口可以对页面上的各种元素进行操作（大小、位置、颜色等）
3. #### **BOM——浏览器对象模型**

   **浏览器对象模型**(Browser Object Model，简称BOM) 是指浏览器对象模型，它提供了独立于内容的、可以与浏览器窗口进行互动的对象结构。通过BOM可以操作浏览器窗口，比如弹出框、控制浏览器跳转、获取分辨率等。

   JS 有3种书写位置，分别为行内、内嵌和外部。
4. 行内式

   ```html
   <input type="button" value="点我试试" onclick="alert('Hello World')" />
   ```

   - 可以将单行或少量 JS 代码写在HTML标签的事件属性中（以 on 开头的属性），如：onclick
   - 注意单双引号的使用：在HTML中我们推荐使用双引号, JS 中我们推荐使用单引号
   - 可读性差， 在html中编写JS大量代码时，不方便阅读；
   - 引号易错，引号多层嵌套匹配时，非常容易弄混；
   - 特殊情况下使用
5. 内嵌式

   ```html
   <script>
       alert('Hello  World~!');
   </script>
   ```

   - 可以将多行JS代码写到 script 标签中
   - 内嵌 JS 是学习时常用的方式
6. 外部JS文件

   ```html
   <script src="my.js"></script>
   ```

   - 利于HTML页面代码结构化，把大段 JS代码独立到 HTML 页面之外，既美观，也方便文件级别的复用
   - 引用外部 JS文件的 script 标签中间不可以写代码
   - 适合于JS 代码量比较大的情况

## 注释

+ flex子项目占的份数
+ align-self控制子项自己在侧轴的排列方式
+ order属性定义子项的排列顺序（前后顺序）

 // 用来注释单行文字（  快捷键   ctrl  +  /   ）

/* */  用来注释多行文字（ 默认快捷键  alt +  shift  + a ），快捷键修改为：   ctrl + shift  +  /

vscode → 首选项按钮 → 键盘快捷方式 → 查找 原来的快捷键 → 修改为新的快捷键 → 回车确认

## 输入输出语句

为了方便信息的输入输出，JS中提供了一些输入输出语句，其常用的语句如下：

| 方法             | 说明                           | 归属   |
| ---------------- | ------------------------------ | ------ |
| alert(msg)       | 浏览器弹出警示框               | 浏览器 |
| console.log(msg) | 浏览器控制台打印输出信息       | 浏览器 |
| prompt(info)     | 浏览器弹出输入框，用户可以输入 | 浏览器 |

- 注意：alert() 主要用来显示消息给用户，console.log() 用来给程序员自己看运行时的消息。

## 变量的使用

- 变量的声明
- 变量的赋值
- 同时声明多个变量

  同时声明多个变量时，只需要写一个 var， 多个变量名之间使用英文逗号隔开。

  ```js
  var age = 10,  name = 'zs', sex = 2;   
  ```
- 声明变量特殊情况

  | 情况                           | 说明                    | 结果      |
  | ------------------------------ | ----------------------- | --------- |
  | var  age ; console.log (age);  | 只声明 不赋值           | undefined |
  | console.log(age)               | 不声明 不赋值  直接使用 | 报错      |
  | age   = 10; console.log (age); | 不声明   只赋值         | 10        |

变量命名规范规则：

- 由字母(A-Za-z)、数字(0-9)、下划线(_)、美元符号( $ )组成，如：usrAge, num01, _name
- 严格区分大小写。var app; 和 var App; 是两个变量
- 不能 以数字开头。  18age   是错误的
- 不能 是关键字、保留字。例如：var、for、while
- 变量名必须有意义。 MMD   BBD        nl   →     age
- **遵守驼峰命名法。首字母小写，后面单词的首字母需要大写。myFirstName**

## 数据类型

数据类型的分类

    JS 把数据类型分为两类：

- 简单数据类型 （Number,String,Boolean,Undefined,Null）
- 复杂数据类型 （object)
- 数字型 Number

  JavaScript 数字类型既可以保存整数，也可以保存小数(浮点数）。

  ```js
  var age = 21;       // 整数
  var Age = 21.3747;  // 小数   
  ```

  1. 数字型进制

     最常见的进制有二进制、八进制、十进制、十六进制。

     ```js
       // 1.八进制数字序列范围：0~7
      var num1 = 07;   // 对应十进制的7
      var num2 = 019;  // 对应十进制的19
      var num3 = 08;   // 对应十进制的8
       // 2.十六进制数字序列范围：0~9以及A~F
      var num = 0xA;   
     ```

     现阶段我们只需要记住，在JS中八进制前面加0，十六进制前面加 0x
  2. 数字型范围

     JavaScript中数值的最大和最小值

     - 最大值：Number.MAX_VALUE，这个值为： 1.7976931348623157e+308
     - 最小值：Number.MIN_VALUE，这个值为：5e-32

3. 数字型三个特殊值

   - Infinity ，代表无穷大，大于任何数值
   - -Infinity ，代表无穷小，小于任何数值
   - NaN ，Not a number，代表一个非数值
4. isNaN

   用来判断一个变量是否为非数字的类型，返回 true 或者 false

 ![](https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/图片17.png)

```js
var usrAge = 21;
var isOk = isNaN(userAge);
console.log(isNum);          // false ，21 不是一个非数字
var usrName = "andy";
console.log(isNaN(userName));// true ，"andy"是一个非数字
```

- 字符串型 String

  字符串型可以是引号中的任意文本，其语法为 双引号 "" 和 单引号''

  ```js
  var strMsg = "我爱北京天安门~";  // 使用双引号表示字符串
  var strMsg2 = '我爱吃猪蹄~';    // 使用单引号表示字符串
  // 常见错误
  var strMsg3 = 我爱大肘子;       // 报错，没使用引号，会被认为是js代码，但js没有这些语法
  ```

  因为 HTML 标签里面的属性使用的是双引号，JS 这里我们更推荐使用单引号。

  1. 字符串引号嵌套

     JS 可以用单引号嵌套双引号 ，或者用双引号嵌套单引号 (外双内单，外单内双)

     ```js
     var strMsg = '我是"高帅富"程序猿';   // 可以用''包含""
     var strMsg2 = "我是'高帅富'程序猿";  // 也可以用"" 包含''
     //  常见错误
     var badQuotes = 'What on earth?"; // 报错，不能 单双引号搭配
     ```
  2. 字符串转义符

     类似HTML里面的特殊字符，字符串中也有特殊字符，我们称之为转义符。

     转义符都是 \ 开头的，常用的转义符及其说明如下：

     | 转义符 | 解释说明                          |
     | ------ | --------------------------------- |
     | \n     | 换行符，n   是   newline   的意思 |
     | \ \    | 斜杠   \                          |
     | \'     | '   单引号                        |
     | \"     | ”双引号                          |
     | \t     | tab  缩进                         |
     | \b     | 空格 ，b   是   blank  的意思     |
  3. 字符串长度

     字符串是由若干字符组成的，这些字符的数量就是字符串的长度。通过字符串的 length 属性可以获取整个字符串的长度。

     ```js
     var strMsg = "我是帅气多金的程序猿！";
     alert(strMsg.length); // 显示 11
     ```
  4. 字符串拼接

     - 多个字符串之间可以使用 + 进行拼接，其拼接方式为 字符串 + 任何类型 = 拼接之后的新字符串
     - 拼接前会把与字符串相加的任何类型转成字符串，再拼接成一个新的字符串

       ```js
       //1.1 字符串 "相加"
       alert('hello' + ' ' + 'world'); // hello world
       //1.2 数值字符串 "相加"
       alert('100' + '100'); // 100100
       //1.3 数值字符串 + 数值
       alert('11' + 12);     // 1112 
       ```

       - ***+ 号总结口诀：数值相加 ，字符相连***
  5. 字符串拼接加强

     ```js
     console.log('pink老师' + 18);        // 只要有字符就会相连 
     var age = 18;
     console.log('pink老师age岁啦');      // 这样不行哦
     console.log('pink老师' + age);         // pink老师18
     console.log('pink老师' + age + '岁啦'); // pink老师18岁啦
     ```

     - 经常会将字符串和变量来拼接，变量可以很方便地修改里面的值
     - 变量是不能添加引号的，因为加引号的变量会变成字符串
     - 如果变量两侧都有字符串拼接，口诀“引引加加 ”，删掉数字，变量写加中间
- 布尔型Boolean

  布尔类型有两个值：true 和 false ，其中 true 表示真（对），而 false 表示假（错）。

  布尔型和数字型相加的时候， true 的值为 1 ，false 的值为 0。

  ```js
  console.log(true + 1);  // 2
  console.log(false + 1); // 1
  ```
- Undefined和 Null

  一个声明后没有被赋值的变量会有一个默认值undefined ( 如果进行相连或者相加时，注意结果）

  ```js
  var variable;
  console.log(variable);           // undefined
  console.log('你好' + variable);  // 你好undefined
  console.log(11 + variable);     // NaN
  console.log(true + variable);   //  NaN
  ```

  一个声明变量给 null 值，里面存的值为空（学习对象时，我们继续研究null)

  ```js
  var vari = null;
  console.log('你好' + vari);  // 你好null
  console.log(11 + vari);     // 11
  console.log(true + vari);   //  1
  ```

### 获取变量数据类型

- 获取检测变量的数据类型

  typeof 可用来获取检测变量的数据类型

  ```js
  var num = 18;
  console.log(typeof num) // 结果 number  
  ```

  不同类型的返回值

  ![](https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/图片18.png)
- 字面量

  字面量是在源代码中一个固定值的表示法，通俗来说，就是字面量表示如何表达这个值。

  - 数字字面量：8, 9, 10
  - 字符串字面量：'黑马程序员', "大前端"
  - 布尔字面量：true，false

### 数据类型转换

- 转换为字符串

  ![img](https://s2.loli.net/2024/03/07/aDbFRZ7JynMwHSY.png)

  - toString() 和 String()  使用方式不一样。
  - 三种转换方式，更多第三种加号拼接字符串转换方式， 这一种方式也称之为隐式转换。
- 转换为数字型（重点）

  ![img](https://s2.loli.net/2024/03/07/wdV98ZQUyLRS7xE.png)![]()

  - 注意 parseInt 和 parseFloat 单词的大小写，这2个是重点
  - 隐式转换是我们在进行算数运算的时候，JS 自动转换了数据类型
- 转换为布尔型

  ![](https://s2.loli.net/2024/03/07/cLOEZlA1pHbNxDU.png)

  - 代表空、否定的值会被转换为 false  ，如 ''、0、NaN、null、undefined
  - 其余值都会被转换为 true

    ```js
    console.log(Boolean('')); // false
    console.log(Boolean(0)); // false
    console.log(Boolean(NaN)); // false
    console.log(Boolean(null)); // false
    console.log(Boolean(undefined)); // false
    console.log(Boolean('小白')); // true
    console.log(Boolean(12)); // true
    ```

## 解释型语言和编译型语言

    计算机不能直接理解任何除机器语言以外的语言，所以必须要把程序员所写的程序语言翻译成机器语言才能执行程序。程序语言翻译成机器语言的工具，被称为翻译器。

![](https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/图片22.png)

- 翻译器翻译的方式有两种：一个是编译，另外一个是解释。两种方式之间的区别在于翻译的时间点不同
- 编译器是在代码执行之前进行编译，生成中间代码文件
- 解释器是在运行时进行及时解释，并立即执行(当编译器以解释方式运行的时候，也称之为解释器)

执行过程：

![](https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/图片23.png)
