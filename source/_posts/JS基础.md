---
title: JavaScript基础知识Ⅰ
date: 2023-05-03 20:03:26
tags: 
  - JS
  - 基础知识
categories: 	
  - 大前端

---

# JS基础

> 整理自黑马Pink前端的课程资料；
>
> 算是入门第一步；
>
> 后续有时间的话，再看看书，多了解了解

## emmet语法

Emmet的前身是Zen coding,它使用缩写,来提高html/css的编写速度。

1. 生成标签 直接输入标签名 按tab键即可   比如  div   然后tab 键， 就可以生成 <div></div>

2. 如果想要生成多个相同标签  加上 * 就可以了 比如   div*3  就可以快速生成3个div

3. 如果有父子级关系的标签，可以用 >  比如   ul > li就可以了

4. 如果有兄弟关系的标签，用  +  就可以了 比如 div+p  

5. 如果生成带有类名或者id名字的，  直接写  .demo  或者  #two   tab 键就可以了

6. 如果生成的div 类名是有顺序的， 可以用 自增符号  $     

   ~~~
   .demo$*3        
   <div class="demo1"></div>
   <div class="demo2"></div>
   <div class="demo3"></div>
   ~~~

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

   ​		ECMAScript：规定了JS的编程语法和基础核心知识，是所有浏览器厂商共同遵守的一套JS语法工业标准。

   更多参看MDN: [MDN手册](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/JavaScript_technologies_overview)

2. #### **DOM——文档对象模型**

   	**文档对象模型**（Document Object Model，简称DOM），是W3C组织推荐的处理可扩展标记语言的标准编程接口。通过 DOM 提供的接口可以对页面上的各种元素进行操作（大小、位置、颜色等）

3. #### **BOM——浏览器对象模型**

   	**浏览器对象模型**(Browser Object Model，简称BOM) 是指浏览器对象模型，它提供了独立于内容的、可以与浏览器窗口进行互动的对象结构。通过BOM可以操作浏览器窗口，比如弹出框、控制浏览器跳转、获取分辨率等。



	JS 有3种书写位置，分别为行内、内嵌和外部。

1. 行内式

   ```html
   <input type="button" value="点我试试" onclick="alert('Hello World')" />
   ```

   - 可以将单行或少量 JS 代码写在HTML标签的事件属性中（以 on 开头的属性），如：onclick
   - 注意单双引号的使用：在HTML中我们推荐使用双引号, JS 中我们推荐使用单引号
   - 可读性差， 在html中编写JS大量代码时，不方便阅读；
   - 引号易错，引号多层嵌套匹配时，非常容易弄混；
   - 特殊情况下使用

2. 内嵌式

   ```html
   <script>
       alert('Hello  World~!');
   </script>
   ```

   - 可以将多行JS代码写到 script 标签中
   - 内嵌 JS 是学习时常用的方式

3. 外部JS文件

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

单行注释的注释方式如下：

```html
// 我是一行文字，不想被 JS引擎 执行，所以 注释起来	
```

	// 用来注释单行文字（  快捷键   ctrl  +  /   ）

多行注释的注释方式如下：

```html
/*
  获取用户年龄和姓名
  并通过提示框显示出来
*/
```

```
/* */  用来注释多行文字（ 默认快捷键  alt +  shift  + a ） 
```


快捷键修改为：   ctrl + shift  +  /

vscode → 首选项按钮 → 键盘快捷方式 → 查找 原来的快捷键 → 修改为新的快捷键 → 回车确认

##  输入输出语句

为了方便信息的输入输出，JS中提供了一些输入输出语句，其常用的语句如下：

| 方法             | 说明                           | 归属   |
| ---------------- | ------------------------------ | ------ |
| alert(msg)       | 浏览器弹出警示框               | 浏览器 |
| console.log(msg) | 浏览器控制台打印输出信息       | 浏览器 |
| prompt(info)     | 浏览器弹出输入框，用户可以输入 | 浏览器 |

- 注意：alert() 主要用来显示消息给用户，console.log() 用来给程序员自己看运行时的消息。

##  变量的使用

- 变量的声明   

- 变量的赋值

- 同时声明多个变量

  ​		同时声明多个变量时，只需要写一个 var， 多个变量名之间使用英文逗号隔开。

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

  ​		JavaScript 数字类型既可以保存整数，也可以保存小数(浮点数）。  

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

  ​		字符串型可以是引号中的任意文本，其语法为 双引号 "" 和 单引号''

  ```js
  var strMsg = "我爱北京天安门~";  // 使用双引号表示字符串
  var strMsg2 = '我爱吃猪蹄~';    // 使用单引号表示字符串
  // 常见错误
  var strMsg3 = 我爱大肘子;       // 报错，没使用引号，会被认为是js代码，但js没有这些语法
  ```

  ​		因为 HTML 标签里面的属性使用的是双引号，JS 这里我们更推荐使用单引号。

  1. 字符串引号嵌套

     ​		JS 可以用单引号嵌套双引号 ，或者用双引号嵌套单引号 (外双内单，外单内双)

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
     | \"     | ”双引号                           |
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

  ​		布尔类型有两个值：true 和 false ，其中 true 表示真（对），而 false 表示假（错）。

  ​		布尔型和数字型相加的时候， true 的值为 1 ，false 的值为 0。

  ```js
  console.log(true + 1);  // 2
  console.log(false + 1); // 1
  ```

- Undefined和 Null

  ​		一个声明后没有被赋值的变量会有一个默认值undefined ( 如果进行相连或者相加时，注意结果）

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

###  获取变量数据类型

- 获取检测变量的数据类型

  ​		typeof 可用来获取检测变量的数据类型

  ```js
  var num = 18;
  console.log(typeof num) // 结果 number      
  ```

  ​		不同类型的返回值

  ![](https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/图片18.png)

- 字面量

  ​		字面量是在源代码中一个固定值的表示法，通俗来说，就是字面量表示如何表达这个值。

  - 数字字面量：8, 9, 10
  - 字符串字面量：'黑马程序员', "大前端"
  - 布尔字面量：true，false

### 数据类型转换

- 转换为字符串

  ![image-20230416204735959](F:\20-求职\11-前端\images\js\image-20230416204735959.png)

  - toString() 和 String()  使用方式不一样。
  - 三种转换方式，更多第三种加号拼接字符串转换方式， 这一种方式也称之为隐式转换。

- 转换为数字型（重点）

  ![image-20230416204801500](F:\20-求职\11-前端\images\js\image-20230416204801500.png)

  - 注意 parseInt 和 parseFloat 单词的大小写，这2个是重点
  - 隐式转换是我们在进行算数运算的时候，JS 自动转换了数据类型

- 转换为布尔型

  ![image-20230416204811756](F:\20-求职\11-前端\images\js\image-20230416204811756.png)

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

-  翻译器翻译的方式有两种：一个是编译，另外一个是解释。两种方式之间的区别在于翻译的时间点不同
-  编译器是在代码执行之前进行编译，生成中间代码文件
-  解释器是在运行时进行及时解释，并立即执行(当编译器以解释方式运行的时候，也称之为解释器)

执行过程：

![](https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/图片23.png)

## 运算符（操作符）

浮点数的精度问题

浮点数值的最高精度是 17 位小数，但在进行算术计算时其精确度远远不如整数。

```js
var result = 0.1 + 0.2;    // 结果不是 0.3，而是：0.30000000000000004
console.log(0.07 * 100);   // 结果不是 7，  而是：7.000000000000001
```

所以：不要直接判断两个浮点数是否相等 ! 

## 断点调试

```
断点调试是指自己在程序的某一行设置一个断点，调试时，程序运行到这一行就会停住，然后你可以一步一步往下调试，调试过程中可以看各个变量当前的值，出错的话，调试到出错的代码行即显示错误，停下。断点调试可以帮助观察程序的运行过程
```

```html
断点调试的流程：
1、浏览器中按 F12--> sources -->找到需要调试的文件-->在程序的某一行设置断点
2、Watch: 监视，通过watch可以监视变量的值的变化，非常的常用。
3、摁下F11，程序单步执行，让程序一行一行的执行，这个时候，观察watch中变量的值的变化。
```

==标识符命名规范==

-  变量、函数的命名必须要有意义
-  **变量的名称一般用名词**  
-  **函数的名称一般用动词**

## 数组

JS 中创建数组有两种方式：

- 利用  new 创建数组  

  ```js
  var 数组名 = new Array() ；
  var arr = new Array();   // 创建一个新的空数组
  ```

  注意 Array () ，A 要大写    

- 利用数组字面量创建数组

  ```js
  //1. 使用数组字面量方式创建空的数组
  var  数组名 = []；
  //2. 使用数组字面量方式创建带初始值的数组
  var  数组名 = ['小白','小黑','大黄','瑞奇'];
  ```

  - 数组的字面量是方括号 [ ] 
  - 声明数组并赋值称为数组的初始化
  - 这种字面量方式也是我们以后最多使用的方式

- 数组元素的类型

  数组中可以存放任意类型的数据，例如字符串，数字，布尔值等。

  ```js
  var arrStus = ['小白',12,true,28.9];
  ```

```js
// 定义数组
var arrStus = [1,2,3];
// 获取数组中的第2个元素
alert(arrStus[1]);    
```

注意：如果访问时数组没有和索引值对应的元素，则得到的值是undefined

- 数组的长度

  数组的长度：默认情况下表示数组中元素的个数

  使用“数组名.length”可以访问数组元素的数量（数组长度）。

  ```js
  var arrStus = [1,2,3];
  alert(arrStus.length);  // 3
  ```

    **注意**：

- 此处数组的长度是数组元素的个数 ，不要和数组的索引号混淆。

- 当我们数组里面的元素个数发生了变化，这个 length 属性跟着一起变化

- 数组的length属性可以被修改：

- 如果设置的length属性值大于数组的元素个数，则会在数组末尾出现空白元素；

- 如果设置的length属性值小于数组的元素个数，则会把超过该值的数组元素删除

  

## 函数

  ```js
// 声明函数
function 函数名() {
    //函数体代码
}
  ```

  - function 是声明函数的关键字,必须小写

  - 由于函数一般是为了实现某个功能才定义的， 所以通常我们将函数名命名为动词，比如 getSum

  ### 调用函数

  ```js
// 调用函数
函数名();  // 通过调用函数名来执行函数体代码
  ```

  ### arguments的使用

  ​		当不确定有多少个参数传递的时候，可以用 arguments 来获取。JavaScript 中，arguments实际上它是当前函数的一个内置对象。所有函数都内置了一个 arguments 对象，arguments 对象中存储了传递的所有实参。arguments展示形式是一个伪数组，因此可以进行遍历。伪数组具有以下特点：

  - 具有 length 属性

  - 按索引方式储存数据

  - 不具有数组的 push , pop 等方法

    注意：在函数内部使用该对象，用此对象获取函数调用时传的实参。

### 函数案例

  	函数内部可以调用另一个函数，在同一作用域代码中，函数名即代表封装的操作，使用函数名加括号即可以将封装的操作执行。

### 函数的两种声明方式

  - 自定义函数方式(命名函数)

    利用函数关键字 function 自定义函数方式

    ```js
    // 声明定义方式
    function fn() {...}
    // 调用  
    fn();  
    ```

    - 因为有名字，所以也被称为命名函数
    - **调用函数的代码既可以放到声明函数的前面，也可以放在声明函数的后面**

  - **函数表达式方式(匿名函数）**

    利用函数表达式方式的写法如下： 

    ```js
    // 这是函数表达式写法，匿名函数后面跟分号结束
    var fn = function(){...}；
    // 调用的方式，函数调用必须写到函数体下面
    fn();
    ```

    - 因为函数没有名字，所以也被称为匿名函数
    - 这个fn 里面存储的是一个函数  
    - 函数表达式方式原理跟声明变量方式是一致的
    - **函数调用的代码必须写到函数体后面**

## 作用域

### 作用域概述

​		通常来说，一段程序代码中所用到的名字并不总是有效和可用的，而限定这个名字的可用性的代码范围就是这个名字的作用域。作用域的使用提高了程序逻辑的局部性，增强了程序的可靠性，减少了名字冲突。

JavaScript（es6前）中的作用域有两种：

- 全局作用域
- 局部作用域（函数作用域）	

### 全局作用域

	作用于所有代码执行的环境(整个script标签内部)或独立的js文件。

### 局部作用域

	作用于函数内的代码环境，就是局部作用域。 
	因为跟函数有关系，所以也称为函数作用域。

### jS没有块级作用域

- 块作用域由 { } 包括。

- 在其他编程语言中（如 java、c#等），在 if 语句、循环语句中创建的变量，仅仅只能在本 if 语句、本循环语句中使用；

  js中没有块级作用域（在ES6之前）

  ```js
  if(true){
    var num = 123;
    //console.log(num); //123
  }
  console.log(num);   //123
  ```

### 变量的作用域

	在JavaScript中，根据作用域的不同，变量可以分为两种：

- 全局变量
- 局部变量

全局变量

	在全局作用域下声明的变量叫做全局变量（在函数外部定义的变量）。

- 全局变量在代码的任何位置都可以使用
- 在全局作用域下 var 声明的变量 是全局变量
- **特殊情况下，在函数内不使用 var 声明的变量也是全局变量（不建议使用）**

局部变量

	在局部作用域下声明的变量叫做局部变量（在函数内部定义的变量）

- 局部变量只能在该函数内部使用
- **在函数内部 var 声明的变量是局部变量**
- **函数的形参实际上就是局部变量**

全局变量和局部变量的区别

- 全局变量：在任何一个地方都可以使用，只有在浏览器关闭时才会被销毁，因此比较占内存
- 局部变量：只在函数内部使用，当其所在的代码块被执行时，会被初始化；当代码块运行结束后，就会被销毁，因此更节省内存空间

### 作用域链

只要是代码都一个作用域中，写在函数内部的局部作用域，未写在任何函数内部即在全局作用域中；如果函数中还有函数，那么在这个作用域中就又可以诞生一个作用域；根据在**[内部函数可以访问外部函数变量]**的这种机制，用链式查找决定哪些数据能被内部函数访问，就称作作用域链。

```js
案例分析1：
function f1() {
    var num = 123;
    function f2() {
        console.log( num );//123
    }
    f2();
}
var num = 456;
f1();
```

```js
作用域链：采取就近原则的方式来查找变量最终的值
var a = 1;
function fn1() {
    var a = 2;
    var b = '22';
    fn2();
    function fn2() {
        var a = 3;
        fn3();
        function fn3() {
            var a = 4;
            console.log(a); //a的值 4
            console.log(b); //b的值 22
        }
    }
}
fn1();
```

![](https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/图片2-16779152458061.png)

### 预解析

JavaScript 代码是由浏览器中的 JavaScript 解析器来执行的。

JavaScript 解析器在运行 JavaScript 代码的时候分为两步：

​	预解析和代码执行。

- **预解析：在当前作用域下, JS 代码执行之前，浏览器会默认把带有 var 和 function 声明的变量在内存中进行提前声明或者定义**，预解析也叫做变量、函数提升。

- 代码执行： 从上到下执行JS语句。

  注意：**预解析会把变量和函数的声明在代码执行之前执行完成。**

变量预解析：

​	变量的声明会被提升到当前作用域的最上面，变量的赋值不会提升。

```js
console.log(num);  // undefined
var num = 10;
//注意：变量提升只提升声明，不提升赋值
```

函数预解析：

函数的声明会被提升到当前作用域的最上面，但是不会调用函数。

```js
fn();
function fn() {
    console.log('打印');
}
```

结果：控制台打印字符串 --- ”打印“ 

注意：函数声明代表函数整体，所以函数提升后，函数名代表整个函数，但是函数并没有被调用！	

函数表达式声明函数问题：

函数表达式创建函数，会执行变量提升

```js
fn();
var  fn = function() {
    console.log('想不到吧');
}
```

结果：报错提示 ”fn is not a function"

> 解释：该段代码执行之前，会做变量声明提升，fn在提升之后的值是undefined；而fn调用是在fn被赋值为函数体之前，此时fn的值是undefined，所以无法正确调用

## 对象

- 利用字面量创建对象 

  #####     **使用对象字面量创建对象**：

  	就是花括号 { } 里面包含了表达这个具体事物（对象）的属性和方法；{ } 里面采取键值对的形式表示 

  - 键：相当于属性名

  - 值：相当于属性值，可以是任意类型的值（数字类型、字符串类型、布尔类型，函数类型等）

    代码如下：

    ```js
    var star = {
        name : 'pink',
        age : 18,
        sex : '男',
        sayHi : function(){
            alert('大家好啊~');
        }
    };
    ```

    上述代码中 star即是创建的对象。

- 对象的使用

  - 对象的属性

    - 对象中存储**具体数据**的 "键值对"中的 "键"称为对象的属性，即对象中存储具体数据的项

  - 对象的方法

    - 对象中存储**函数**的 "键值对"中的 "键"称为对象的方法，即对象中存储函数的项

  - 访问对象的属性

    - 对象里面的属性调用 : 对象.属性名 ，这个小点 . 就理解为“ 的 ”  

    - 对象里面属性的另一种调用方式 : 对象[‘属性名’]，注意方括号里面的属性必须加引号      

      示例代码如下：

      ```js
      console.log(star.name)     // 调用名字属性
      console.log(star['name'])  // 调用名字属性
      ```

  - 调用对象的方法

    - 对象里面的方法调用：对象.方法名() ，注意这个方法名字后面一定加括号 

      示例代码如下：

      ```js
      star.sayHi();              // 调用 sayHi 方法,注意，一定不要忘记带后面的括号
      ```

  - 变量、属性、函数、方法总结

    	属性是对象的一部分，而变量不是对象的一部分，变量是单独存储数据的容器

    - 变量：单独声明赋值，单独存在

    - 属性：对象里面的变量称为属性，不需要声明，用来描述该对象的特征

      方法是对象的一部分，函数不是对象的一部分，函数是单独封装操作的容器

    - 函数：单独存在的，通过“函数名()”的方式就可以调用

    - 方法：对象里面的函数称为方法，方法不需要声明，使用“对象.方法名()”的方式就可以调用，方法用来描述该对象的行为和功能。 

- 利用 new Object 创建对象 

  - 创建空对象

    ```js
    var andy = new Obect();
    ```

    通过内置构造函数Object创建对象，此时andy变量已经保存了创建出来的空对象

  - 给空对象添加属性和方法

    - 通过对象操作属性和方法的方式，来为对象增加属性和方法

      示例代码如下：

    ```js
    andy.name = 'pink';
    andy.age = 18;
    andy.sex = '男';
    andy.sayHi = function(){
        alert('大家好啊~');
    }
    ```

    注意：

    - Object() ：第一个字母大写   
    - new Object() ：需要 new 关键字
    - 使用的格式：对象.属性 =  值;     

- 利用构造函数创建对象

  - 构造函数

    - 构造函数：是一种特殊的函数，主要用来初始化对象，即为对象成员变量赋初始值，它总与 new 运算符一起使用。我们可以把对象中一些公共的属性和方法抽取出来，然后封装到这个函数里面。

    - 构造函数的封装格式：

      ```js
      function 构造函数名(形参1,形参2,形参3) {
           this.属性名1 = 参数1;
           this.属性名2 = 参数2;
           this.属性名3 = 参数3;
           this.方法名 = 函数体;
      }
      ```

    - 构造函数的调用格式

      ```
      var obj = new 构造函数名(实参1，实参2，实参3)
      ```

      以上代码中，obj即接收到构造函数创建出来的对象。

    - 注意事项

      1.   构造函数约定**首字母大写**。
      2.   函数内的属性和方法前面需要添加 **this** ，表示当前对象的属性和方法。
      3.   构造函数中**不需要 return 返回结果**。
      4.   当我们创建对象的时候，**必须用 new 来调用构造函数**。

    - 其他

      构造函数，如 Stars()，抽象了对象的公共部分，封装到了函数里面，它泛指某一大类（class）  
      创建对象，如 new Stars()，特指某一个，通过 new 关键字创建对象的过程我们也称为对象实例化

- new关键字的作用

  1. 在构造函数代码开始执行之前，创建一个空对象；
  2. 修改this的指向，把this指向创建出来的空对象；
  3. 执行函数的代码
  4. 在函数完成之后，返回this---即创建出来的对象

  ### 遍历对象

  	for...in 语句用于对数组或者对象的属性进行循环操作。
  	
  	其语法如下：

  ```js
  for (变量 in 对象名字) {
      // 在此执行代码
  }
  ```

  	语法中的变量是自定义的，它需要符合命名规范，通常我们会将这个变量写为 k 或者 key。

  ```js
  for (var k in obj) {
      console.log(k);      // 这里的 k 是属性名
      console.log(obj[k]); // 这里的 obj[k] 是属性值
  }
  ```

### 内置对象

JavaScript 中的对象分为3种：**自定义对象 、内置对象、 浏览器对象**
前面两种对象是JS 基础内容，属于 ECMAScript；  第三个浏览器对象属于 JS 独有的， JS API 讲解内置对象就是指 JS 语言自带的一些对象，这些对象供开发者使用，并提供了一些常用的或是**最基本而必要的功能**（属性和方法），内置对象最大的优点就是帮助我们快速开发

JavaScript 提供了多个内置对象：Math、 Date 、Array、String等	

### 查文档

查找文档：学习一个内置对象的使用，只要学会其常用成员的使用即可，我们可以通过查文档学习，可以通过MDN/W3C来查询。
Mozilla 开发者网络（MDN）提供了有关开放网络技术（Open Web）的信息，包括 HTML、CSS 和万维网及 HTML5 应用的 API。
MDN:https://developer.mozilla.org/zh-CN/

### Math对象

Math 对象不是构造函数，它具有数学常数和函数的属性和方法。跟数学相关的运算（求绝对值，取整、最大值等）可以使用 Math 中的成员。

| 属性、方法名          | 功能                                         |
| --------------------- | -------------------------------------------- |
| Math.PI               | 圆周率                                       |
| Math.floor()          | 向下取整                                     |
| Math.ceil()           | 向上取整                                     |
| Math.round()          | 四舍五入版 就近取整   注意 -3.5   结果是  -3 |
| Math.abs()            | 绝对值                                       |
| Math.max()/Math.min() | 求最大和最小值                               |
| Math.random()         | 获取范围在[0,1)内的随机值                    |

​	注意：上面的方法使用时必须带括号

​	**获取指定范围内的随机整数**：

```js
function getRandom(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min; 
}
```

```js
// 利用对象封装自己的数学对象  里面有 PI 最大值和最小值
        var myMath = {
            PI: 3.141592653,
            max: function() {
                var max = arguments[0];
                for (var i = 1; i < arguments.length; i++) {
                    if (arguments[i] > max) {
                        max = arguments[i];
                    }
                }
                return max;
            },
            min: function() {
                var min = arguments[0];
                for (var i = 1; i < arguments.length; i++) {
                    if (arguments[i] < min) {
                        min = arguments[i];
                    }
                }
                return min;
            }
        }
        console.log(myMath.PI);
        console.log(myMath.max(1, 5, 9));
        console.log(myMath.min(1, 5, 9));
```



### 日期对象

Date 对象和 Math 对象不一样，Date是一个构造函数，所以使用时需要实例化后才能使用其中具体方法和属性。Date 实例用来处理日期和时间

- 使用Date实例化日期对象

  - 获取当前时间必须实例化：

  ```js
  var now = new Date();
  ```

  - 获取指定时间的日期对象

  ```js
  var future = new Date('2019/5/1');
  ```

  注意：如果创建实例时并未传入参数，则得到的日期对象是当前时间对应的日期对象

- 使用Date实例的方法和属性

  ![](https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/图片1-16816526761481.png)

- 通过Date实例获取总毫米数

  - 总毫秒数的含义

    ​	基于1970年1月1日（世界标准时间）起的毫秒数

  - 获取总毫秒数

    ```js
    // 实例化Date对象
    var now = new Date();
    // 1. 用于获取对象的原始值
    console.log(date.valueOf())	
    console.log(date.getTime())	
    // 2. 简单写可以这么做
    var now = + new Date();			
    // 3. HTML5中提供的方法，有兼容性问题
    var now = Date.now();
    ```

### 数组对象

#### 创建数组的两种方式

- 字面量方式

  - 示例代码如下：

    ```js
    var arr = [1,"test",true];
    ```

- new Array()

  - 示例代码如下：

    ```
    var arr = new Array();
    ```

    ​	注意：上面代码中arr创建出的是一个空数组，如果需要使用构造函数Array创建非空数组，可以在创建数组时传入参数

    ​	参数传递规则如下：

    - 如果只传入一个参数，则参数规定了数组的长度

    - 如果传入了多个参数，则参数称为数组的元素

#### 检测是否为数组

- instanceof 运算符

  - instanceof 可以判断一个对象是否是某个构造函数的实例

    ```js
    var arr = [1, 23];
    var obj = {};
    console.log(arr instanceof Array); // true
    console.log(obj instanceof Array); // false
    ```

- Array.isArray()

  - Array.isArray()用于判断一个对象是否为数组，isArray() 是 HTML5 中提供的方法

    ```js
    var arr = [1, 23];
    var obj = {};
    console.log(Array.isArray(arr));   // true
    console.log(Array.isArray(obj));   // false
    ```

#### 添加删除数组元素的方法

- 数组中有进行增加、删除元素的方法，部分方法如下表

  ![img](https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/pushpop.png)

  注意：push、unshift为增加元素方法；pop、shift为删除元素的方法

#### 数组排序

- 数组中有对数组本身排序的方法，部分方法如下表

  ![img](https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/sort.png)

  注意：sort方法需要传入参数来设置升序、降序排序

  - 如果传入“function(a,b){ return a-b;}”，则为升序
  - 如果传入“function(a,b){ return b-a;}”，则为降序

#### 数组索引方法

- 数组中有获取数组指定元素索引值的方法，部分方法如下表

  ![img](https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/index.png)

#### 数组转换为字符串

- 数组中有把数组转化为字符串的方法，部分方法如下表

  ![](https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/tostring.png)

  注意：join方法如果不传入参数，则按照 “ , ”拼接元素

#### 其他方法

- 数组中还有其他操作方法，同学们可以在课下自行查阅学习

  ![img](https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/other.png)

### 字符串对象

#### 基本包装类型

为了方便操作基本数据类型，JavaScript 还提供了三个特殊的引用类型：String、Number和 Boolean。

基本包装类型就是把简单数据类型包装成为复杂数据类型，这样基本数据类型就有了属性和方法。

```js
// 下面代码有什么问题？
var str = 'andy';
console.log(str.length);
```

按道理基本数据类型是没有属性和方法的，而对象才有属性和方法，但上面代码却可以执行，这是因为js会把基本数据类型包装为复杂数据类型，其执行过程如下 ：

```js
// 1. 生成临时变量，把简单类型包装为复杂数据类型
var temp = new String('andy');
// 2. 赋值给我们声明的字符变量
str = temp;
// 3. 销毁临时变量
temp = null;
```

#### 字符串的不可变

指的是里面的值不可变，虽然看上去可以改变内容，但其实是地址变了，内存中新开辟了一个内存空间。

当重新给字符串变量赋值的时候，变量之前保存的字符串不会被修改，依然在内存中重新给字符串赋值，会重新在内存中开辟空间，这个特点就是字符串的不可变。
由于字符串的不可变，在**大量拼接字符串**的时候会有效率问题

#### 根据字符返回位置

​		字符串通过基本包装类型可以调用部分方法来操作字符串，以下是返回指定字符的位置的方法：

![img](https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/location.png)

案例：查找字符串"abcoefoxyozzopp"中所有o出现的位置以及次数

```js
var str = "oabcoefoxyozzopp";
var index = str.indexOf('o');
var num = 0;
// console.log(index);
while (index !== -1) {
      console.log(index);
      num++;
      index = str.indexOf('o', index + 1);
}
console.log('o出现的次数是: ' + num);
```

#### 根据位置返回字符

字符串通过基本包装类型可以调用部分方法来操作字符串，以下是根据位置返回指定位置上的字符：

![](https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/place.png)

案例：判断一个字符串 'abcoefoxyozzopp' 中出现次数最多的字符，并统计其次数

```js
      var o = {};
      var str = "abcoefoxyozzopp";
      //   for (var k in str) {
      //     if (o[str[k]]) {
      //       o[str[k]]++;
      //     } else {
      //       o[str[k]] = 1;
      //     }
      //   }
      for (var i = 0; i < str.length; i++) {
        var chars = str.charAt(i);
        if (o[chars]) {
          o[chars]++;
        } else {
          o[chars] = 1;
        }
      }
      console.log(o);
      var max = 0;
      var ch = "";
      for (var k in o) {
        if (o[k] > max) {
          max = o[k];
          ch = k;
        }
      }
      console.log(max);
      console.log(ch);
```

#### 字符串操作方法

字符串通过基本包装类型可以调用部分方法来操作字符串，以下是部分操作方法：

![](https://raw.githubusercontent.com/youxt-njnu/blog-img/master/%E5%9B%BE%E7%89%8710.png)

#### replace()方法

replace() 方法用于在字符串中用一些字符替换另一些字符，其使用格式如下：  

```
字符串.replace(被替换的字符串， 要替换为的字符串)；
```

#### split()方法

split()方法用于切分字符串，它可以将字符串切分为数组。在切分完毕之后，返回的是一个新数组。

```
字符串.split("分割字符")
```

###  复杂数据类型传参

函数的形参也可以看做是一个变量，当我们把引用类型变量传给形参时，其实是把变量在栈空间里保存的堆地址复制给了形参，形参和实参其实保存的是同一个堆地址，所以操作的是同一个对象。

```JavaScript
function Person(name) {//构造函数
    this.name = name;
}
function f1(x) { // x = p
    console.log(x.name); // 2.刘德华   
    x.name = "张学友";
    console.log(x.name); // 3. 张学友  
}
var p = new Person("刘德华"); //实例
console.log(p.name);    // 1.刘德华
f1(p);
console.log(p.name);    // 4. 张学友
```

