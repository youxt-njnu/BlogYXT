---
title: JavaScript基础知识Ⅲ
date: 2024-03-17 09:33:26
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
> 算是入门第一步；
> 后续有时间的话，再看看书，多了解了解



## 11. 对象

* **利用字面量创建对象**

  ##### **使用对象字面量创建对象**：


  ```
  就是花括号 { } 里面包含了表达这个具体事物（对象）的属性和方法；{ } 里面采取键值对的形式表示 
  ```

  * 键：相当于属性名
  * 值：相当于属性值，可以是任意类型的值（数字类型、字符串类型、布尔类型，函数类型等）
    代码如下：

    ```
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
* 对象的使用

  * 对象的属性

    * 对象中存储具体数据的 "键值对"中的 "键"称为对象的属性，即对象中存储具体数据的项
  * 对象的方法

    * 对象中存储函数的 "键值对"中的 "键"称为对象的方法，即对象中存储函数的项
  * 访问对象的属性

    * 对象里面的属性调用 : 对象.属性名 ，这个小点 . 就理解为“ 的 ”
    * 对象里面属性的另一种调用方式 : 对象[‘属性名’]，注意方括号里面的属性必须加引号
      **示例代码如下：**
      ```
      console.log(star.name)     // 调用名字属性
      console.log(star['name'])  // 调用名字属性
      ```
  * **调用对象的方法**

    * 对象里面的方法调用：对象.方法名() ，注意这个方法名字后面一定加括号
      示例代码如下：
      ```
      star.sayHi();              // 调用 sayHi 方法,注意，一定不要忘记带后面的括号
      ```
  * **变量、属性、函数、方法总结**

    ```
    属性是对象的一部分，而变量不是对象的一部分，变量是单独存储数据的容器
    ```

    * 变量：单独声明赋值，单独存在
    * 属性：对象里面的变量称为属性，不需要声明，用来描述该对象的特征
      方法是对象的一部分，函数不是对象的一部分，函数是单独封装操作的容器
    * 函数：单独存在的，通过“函数名()”的方式就可以调用
    * 方法：对象里面的函数称为方法，方法不需要声明，使用“对象.方法名()”的方式就可以调用，方法用来描述该对象的行为和功能
* **利用 new Object 创建对象**

  * 创建空对象

    ```
    var andy = new Obect();
    ```

    通过内置构造函数Object创建对象，此时andy变量已经保存了创建出来的空对象
  * 给空对象添加属性和方法

    * 通过对象操作属性和方法的方式，来为对象增加属性和方法
      示例代码如下：

    ```
    andy.name = 'pink';
    andy.age = 18;
    andy.sex = '男';
    andy.sayHi = function(){
        alert('大家好啊~');
    }
    ```

    **注意：**

    * Object() ：第一个字母大写
    * new Object() ：需要 new 关键字
    * 使用的格式：对象.属性 =  值;
* **利用构造函数创建对象**

  * 构造函数
    * 构造函数：是一种特殊的函数，主要用来初始化对象，即为对象成员变量赋初始值，它总与 new 运算符一起使用。我们可以把对象中一些公共的属性和方法抽取出来，然后封装到这个函数里面。
    * 构造函数的封装格式：

      ```
      function 构造函数名(形参1,形参2,形参3) {
           this.属性名1 = 参数1;
           this.属性名2 = 参数2;
           this.属性名3 = 参数3;
           this.方法名 = 函数体;
      }
      ```
    * **构造函数的调用格式**

      ```
      var obj = new 构造函数名(实参1，实参2，实参3)
      ```

      以上代码中，obj即接收到构造函数创建出来的对象。
    * 注意事项

      1. 构造函数约定首字母大写。
      2. 函数内的属性和方法前面需要添加**this** ，表示当前对象的属性和方法。
      3. **构造函数中不需要 return 返回结果**。
      4. 当我们创建对象的时候，必须用 new 来调用构造函数。
    * **其他**
      构造函数，如 Stars()，抽象了对象的公共部分，封装到了函数里面，它泛指某一大类（class）
      创建对象，如 new Stars()，特指某一个，通过 new 关键字创建对象的过程我们也称为对象实例化
* **new关键字的作用**

  1. 在构造函数代码开始执行之前，创建一个空对象；
  2. 修改this的指向，把this指向创建出来的空对象；
  3. 执行函数的代码
  4. 在函数完成之后，返回this---即创建出来的对象

  ### 5.3 遍历对象


  ```
  for...in 语句用于对数组或者对象的属性进行循环操作。

  其语法如下：
  ```

  ```
  for (变量 in 对象名字) {
      // 在此执行代码
  }
  ```

  ```
  语法中的变量是自定义的，它需要符合命名规范，通常我们会将这个变量写为 k 或者 key。
  ```

  ```
  for (var k in obj) {
      console.log(k);      // 这里的 k 是属性名
      console.log(obj[k]); // 这里的 obj[k] 是属性值
  }
  ```

### 1.1 内置对象

JavaScript 中的对象分为3种：自定义对象 、内置对象、 浏览器对象
**前面两种对象是JS 基础内容，属于 ECMAScript；  第三个浏览器对象属于 JS 独有的， JS API 讲解内置对象就是指 JS 语言自带的一些对象，这些对象供开发者使用，并提供了一些常用的或是**最基本而必要的功能（属性和方法），内置对象最大的优点就是帮助我们快速开发

**JavaScript 提供了多个内置对象：Math、 Date 、Array、String等    **

### 1.2 查文档

查找文档：学习一个内置对象的使用，只要学会其常用成员的使用即可，我们可以通过查文档学习，可以通过MDN/W3C来查询。
Mozilla 开发者网络（MDN）提供了有关开放网络技术（Open Web）的信息，包括 HTML、CSS 和万维网及 HTML5 应用的 API。
MDN:[https://developer.mozilla.org/zh-CN/](https://developer.mozilla.org/zh-CN/)

### 1.3 Math对象

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

**注意：上面的方法使用时必须带括号**

获取指定范围内的随机整数：

```
function getRandom(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min; 
}
```

```
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

### 1.4 日期对象

Date 对象和 Math 对象不一样，Date是一个构造函数，所以使用时需要实例化后才能使用其中具体方法和属性。Date 实例用来处理日期和时间

* 使用Date实例化日期对象

  * 获取当前时间必须实例化：

  ```
  var now = new Date();
  ```

  * 获取指定时间的日期对象

  ```
  var future = new Date('2019/5/1');
  ```

  注意：如果创建实例时并未传入参数，则得到的日期对象是当前时间对应的日期对象
* **使用Date实例的方法和属性**
  ![图片1](https://s2.loli.net/2024/03/17/Bt7q6c4fYMbLpgr.png)
* 通过Date实例获取总毫米数

  * 总毫秒数的含义
    基于1970年1月1日（世界标准时间）起的毫秒数
  * 获取总毫秒数
    ```
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

### 1.5 数组对象

#### 创建数组的两种方式

* 字面量方式
  * 示例代码如下：
    ```
    var arr = [1,"test",true];
    ```
* new Array()
  * 示例代码如下：

    ```
    var arr = new Array();
    ```

    注意：上面代码中arr创建出的是一个空数组，如果需要使用构造函数Array创建非空数组，可以在创建数组时传入参数

    参数传递规则如下：

    * 如果只传入一个参数，则参数规定了数组的长度
    * 如果传入了多个参数，则参数称为数组的元素

#### 检测是否为数组

* instanceof 运算符
  * instanceof 可以判断一个对象是否是某个构造函数的实例
    ```
    var arr = [1, 23];
    var obj = {};
    console.log(arr instanceof Array); // true
    console.log(obj instanceof Array); // false
    ```
* Array.isArray()
  * Array.isArray()用于判断一个对象是否为数组，isArray() 是 HTML5 中提供的方法
    ```
    var arr = [1, 23];
    var obj = {};
    console.log(Array.isArray(arr));   // true
    console.log(Array.isArray(obj));   // false
    ```

#### 添加删除数组元素的方法

* 数组中有进行增加、删除元素的方法，部分方法如下表
  ![pushpop](https://s2.loli.net/2024/03/17/uebmUwin5S3j91r.png)
  注意：push、unshift为增加元素方法；pop、shift为删除元素的方法

#### 数组排序

* **数组中有对数组本身排序的方法，部分方法如下表**
  ![](https://s2.loli.net/2024/03/17/DOPv2ZCqMBnpmsN.png)
  注意：sort方法需要传入参数来设置升序、降序排序
  * 如果传入“function(a,b){ return a-b;}”，则为升序
  * 如果传入“function(a,b){ return b-a;}”，则为降序

#### 数组索引方法

* 数组中有获取数组指定元素索引值的方法，部分方法如下表
  ![](https://s2.loli.net/2024/03/17/IFPG26TQdrbw1NK.png)

#### 数组转换为字符串

* **数组中有把数组转化为字符串的方法，部分方法如下表**
  ![](https://s2.loli.net/2024/03/17/fGBnsd1I65ZK7tz.png)
  **注意：join方法如果不传入参数，则按照 “ , ”拼接元素**

#### 其他方法

* 数组中还有其他操作方法，同学们可以在课下自行查阅学习
  ![](https://s2.loli.net/2024/03/17/3iDC7uAtBHdKRGr.png)

### 1.6 字符串对象

#### 基本包装类型

为了方便操作基本数据类型，JavaScript 还提供了三个特殊的引用类型：String、Number和 Boolean。

基本包装类型就是把简单数据类型包装成为复杂数据类型，这样基本数据类型就有了属性和方法。

```
// 下面代码有什么问题？
var str = 'andy';
console.log(str.length);
```

按道理基本数据类型是没有属性和方法的，而对象才有属性和方法，但上面代码却可以执行，这是因为js会把基本数据类型包装为复杂数据类型，其执行过程如下 ：

```
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
由于字符串的不可变，在****大量拼接字符串的时候会有效率问题

#### 根据字符返回位置

字符串通过基本包装类型可以调用部分方法来操作字符串，以下是返回指定字符的位置的方法：

![](https://s2.loli.net/2024/03/17/7QdYZoglVsuOrxw.png)

**案例：查找字符串"abcoefoxyozzopp"中所有o出现的位置以及次数**

```
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

![](https://s2.loli.net/2024/03/17/6S5Z3Ezf4BYJH9T.png)

案例：判断一个字符串 'abcoefoxyozzopp' 中出现次数最多的字符，并统计其次数

```
      var o = {};
      var str = "abcoefoxyozzopp";
      //   for (var k in str) {
      //     if (o[str[k]]) {
      //       o[str[k]]++;
      //     } else {
      //       o[str[k]] = 1;
      //     }
      //   }
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

**字符串通过基本包装类型可以调用部分方法来操作字符串，以下是部分操作方法：**

![](https://s2.loli.net/2024/03/17/zO5PavVILoNGh9l.png)

#### replace()方法

**replace() 方法用于在字符串中用一些字符替换另一些字符，其使用格式如下：**

```
字符串.replace(被替换的字符串， 要替换为的字符串)；
```

#### split()方法

**split()方法用于切分字符串，它可以将字符串切分为数组。在切分完毕之后，返回的是一个新数组。**

```
字符串.split("分割字符")
```

### 复杂数据类型传参

**函数的形参也可以看做是一个变量，当我们把引用类型变量传给形参时，其实是把变量在栈空间里保存的堆地址复制给了形参，形参和实参其实保存的是同一个堆地址，所以操作的是同一个对象。**

```
function Person(name) {//构造函数
    this.name = name;
}
function f1(x) { // x = p
    console.log(x.name); // 2.刘德华   
    x.name = "张学友";
    console.log(x.name); // 3. 张学友  
}
var p = new Person("刘德华"); //实例
console.log(p.name);    // 1.刘德华
f1(p);
console.log(p.name);    // 4. 张学友
```
