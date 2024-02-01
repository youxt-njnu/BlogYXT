---
title: C#入门梳理Ⅰ|基本知识和方法
tags:
 - 知识框架
 - c#
categories:
 - AR开发
comments: true
date: 2024-01-25 10:15:05
photos: https://wallroom.io/img/2560x1600/bg-9f3ff33.jpg
cover: https://wallroom.io/img/2560x1600/bg-9f3ff33.jpg
---
# 基本知识

solution: 解决方案；针对需求的总方案；

project: 项目；针对具体问题；

**c#的各类应用程序：**

> console:
>
> WPF(windows presentation foundation):
>
> Windows Forms:
>
> ASP.NET Web Forms:
>
> ASP.NET MVC(Model-View-Controller):
>
> WCF(Windows Communication Foundation):
>
> Windows Store Application
>
> Windows Phone Application
>
> Cloud(Windows AZure)
>
> WF(Workflow Foundation)

类（方块加树形结构）和命名空间（花括号）
using 可以把命名空间引入到程序中来，然后程序就能使用命名空空间里的类。

> 可以通过 help 来搜索查看对应的类，会给出对应的 namespace 和 assembly。
> ctrl .可以快速弹出修复建议，然后可以根据这个来添加类。
> 多个命名空间下的类有冲突的话，需要加上些所属的类或命名空间
> 类和命名空间放在了类库里面，assembly。在项目管理器的 references 分支里可以查看。点击后就可以打开 object browser，查看对应的类库下面包括了那些类。
> 添加别人的类库
> 别人已经编译好的 dll，没有源码，但需要给说明文档，说明里面包括了哪些命名空间，哪些类和方法。右键 references 选择 add ，添加到项目中。此时也需要减少对于这个类库的依赖性，在工作中依赖性小的话，如果底层的类库出现了问题，可以用一个替代库来暂时代替。
> 此外，这个时候的引用没有源代码，一些底层的库没办法一起引进来，就会造成大型项目的未知错误。这个时候可以使用 nuget 技术，形成一个包来给我们用，这个时候右键选择 manage nuget...，选择 online 然后搜索需要的库，会安装包。
> 针对有源码的类库，需要把所在地的项目一起添加到我自己的解决方案中，然后我添加reference 里选择 solution 里的添加进来的人 project，就可以把类库的源码加进来了。
> 在 debug 的时候，需要找到 root cause，不要在表象的上面加补丁，不然后续就是补丁加补丁。
> 构建自己的类库，就 add project 的时候，选择 class project，这个时候编译出来的就是 dll ，命名空间名字默认就是添加类库的时候起的项目名称，
> 类（或对象）之间的耦合/依赖关系
> 高内聚，低耦合
> UML 类图
> 排除错误
> 阅读编辑器的错误
> 搜索

## 类

对现实世界的抽象，通过光标位于类名上，F1 即可打开帮助文档，会定位到对应的内容。先看一句话描述，然后看 remark，再看 examples，看不懂的话再去网上搜更好的例子。用到什么属性方法啥的对应着类的里面内容再看。

类 ➡️ 对象/实例（instance，object）
通过 new 来对类进行实例化。 new Form() 加上括号，是用来传入参数进行初始化。
通过引用变量来获取实例。

**类的三大成员**

> 属性 property（🔧） :lighting: ，存储数据，表示类或对象当前的状态（如模型类或对象，entity framework
> 方法 method（紫色多边形），表示能做什么，由 function 进化而来(如工具类或对象，Math
> 事件 event（⚡），类或对象通知其他类或对象的机制如通知类或对象，如 timer
> 静态成员（在文档里会标记红色 S)，实例成员，绑定 binding

**基本元素：（c# references）**

* keyword 关键字，
* operator 操作符，
* identifier 标识符，给...取得名字，尽量让比别人读懂
* 标点符号，
* 文本，
* 注释和空白，
  类型（type，data type），var, x.GetType()，强类型和弱类型语言，C#里 dynamic和 JS 里 var 是类似地位。
  变量
  方法/函数/算法

程序的静态阶段：编辑和编译，这个时候观察它的类和结构
程序的动态阶段：运行阶段，这个时候动态地进行调试

## type

类型的作用（Type）：
估算变量存储所需要的空间大小，得知该变量可存储的最大最小范围，获取该类型包含的成员如方法、属性和事件等。也可以知道该类型由什么基类获取而来，该类型允许的运算。此外，也可以知道该类型变量在分配在内存的位置，栈或堆。

```csharp
Type myType = typeof(Form);
PropertyInfo[] pintos = myType.GetProperties();
MethodInfo[] mInfos = myType.GetMethods[];
```

可以用上面的来做反射，也就是 type 的作用。

栈 stack 用来函数调用方法，会有栈溢出；
堆 heap 用来存储对象，会有内存泄漏。可以在运行的时候，win+r 之后输入 perfmon ，可以打开性能监视器，选择 private bytes 来监视运行的程序的内存耗量。

**C#的数据类型：**

> 类 class，结构体 struct，枚举 enum, 接口 interface，委托 delegate
> ![](https://s2.loli.net/2023/12/24/Yfb13EJRDSexmF9.jpg)

## 变量

存储数据，其实是存储位置。
**变量类型：**

> 静态变量（静态字段，直接类.变量，就能调用），实例变量（成员变量，字段，要实例化之后调用），数组元素，值参数，引用参数 ref，输出形参 out，**局部变量**
>
> 值变量和引用变量：
>
>> 值类型的变量和实例合二为一
>> 引用类型默认栈里面四个字节，指向堆里面的实际的值存储空间；默认值为 0.
>>

装箱和拆箱（用得少，但面试会考）

> 装箱：把数值隐式转换为 object------》把值类型的数值拷贝到堆上，记录对应的内存地址，在 obj 的四字节栈上里存储这个地址。
> 拆箱：从堆上把 obj内容搬过来，存储到 目标数据类型的栈上
> 会损失程序的性能。

# 方法

method，c 里面的 function
方法永远是类或结构体的成员；
方法的作用：隐藏复杂的逻辑、大算法分解为小算法、算法复用、
c#里方法的声明和定义不分家；
方法的命名规范：大小写规范 pascal，以动词或动词短语为名字；
调用方法：实参 argument，形参 parameter

**构造器 constructor：**

* 狭义：实例构造器，instance constructor
* 无返回值类型，用于构造好自己的内存块， public class_name(parameter) { body }，快捷键 ctor 加两次 tab 键。
* 调用带构造参数的函数的时候，构造参数的类型如果上引用类型，那么这一块对应的堆内存存储的仍然是这个引用类型值所在的堆堆地址。
* 构造器也可以进行重载。

方法的构造函数，不包括对于返回值的差异。

**对方法进行debug**

> 设置断点 breakpoint。观察方法调用的 call stack。 step in, step over, step out.。观察局部变量的变化。

方法调用中调用栈和内存栈里的实际分配，最后结果是存储在 CPU 的寄存器里，如果存不下会存在栈里。

## 操作符/运算符，操作数

操作符的优先级

![](https://s2.loli.net/2023/12/24/3JPxSh6EvfwOCn1.png)

操作符是一种简记法，与数据类型关联，且也可以进行自定义（把方法名换成类似：operator+）。

**. 成员访问操作符**: 访问外层空间的子集命名空间，访问命名空间里的类，访问类的静态成员，访问对象中的实例成员。

**f() 方法调用操作符**，就是方法后跟的圆括号。但对于委托 delegate（小提包符号，如 action），

```
Action myAction= new Action(传入方法名即可，无需这个方法执行，不需要圆括号);
myAction(); // 执行传入的所有方法
```

**[] 元素访问操作符**

> 泛型类，<>, 需要和其他类型结合。
>
> 里面存放的是索引，不一定就是整型

**{} 初始化器，初始化元素**

**后置和前置的++，--**

**typeof,** 获取到类型所属的类型，处于哪个命名空间，

```
Type t=typeof(int);
t.Namespace, tFullName,t.Name, t.GetMethods(),
```

**default,** 获取类型的默认值

> struct: 0
>
> enum: 声明的第一个，如果有显式赋值，则返回显式赋值为0的数据，如果没有显式赋值为0的，则返回0
>
> 引用类型：null

**new操作符**

> var关键词，用于声明隐式类型变量； x.GetType().Name
>
> new: 创建一个类型的实例，然后立即调用这个实例的构造器（用圆括号）；
>
> 后面还可以进一步调用实例的初始化器（大括号，再大括号里初始化成员变量这些）
>
> 并不是所有的类类型在初始化的时候都要new，针对一些常用的如string,c#有语法糖衣，可以直接进行初始化 `string name="tim";`
>
> 此外，还有int[] arr=new int[10]; int[] arr={1,2,3,4};  都是语法糖衣
>
> **new也可以为匿名类型创建实例,也就是后面不一定要跟着类型名。可以直接进行初始化。**
>
> ```csharp
> var person = new {Name="time", Age=34};
> Console.WriteLine(person.Name, person.GetType().Name);
>
> ```
>
> new操作符使用的时候要小心，因为里面存在着紧耦合。此时，有一种设计模式“依赖注入depency injection”可以用来让这种紧耦合变得略微松一些
>
> new关键字的多用性：
>
>> 作为修饰符，在子类对父类方法的修改中；
>>

**checked，unchecked，用于检查值是否溢出。**

> 操作符用法：check(变量)
>
> 上下文用法：checked { }

**delegate操作符：**

> 使用较少，已被Lambert表达式取代，用于声明匿名方法。
>
> delegate() { }
>
> 已过时
>
> lambert表达式：(形参，且不需要类型) => {}

**sizeof():**

> 获取系统已有的数据类型；
>
> 更精确的数据类型decimal，相对于double, int, 这些，是16位的；
>
> 在获取自定义数据类型的大小的时候，需要在外面套上unsafe{ int x = sizeof(Student)} 还要设置项目属性里的build里的allow unsafe code,才能运行。

****->, *, & c#里面的指针的使用****

**+,-,~ ！**

> if(!string.IsNullOrEmpty(initName)) 如果字符串initName不是null也不是空

**强制类型转换运算符(T)x**

> 类型转换
> 隐式转换

* 不丢失精度的转换
* 子类向父类的转换
* 装箱
* 隐式类型转换操作符    -类里面，public static implicit operator 转换类名() {} 除了修饰符，其他都类似一个构造器
  显示转换
* 可能丢失精度的转换，cast ，y= (unit)x, （针对一些差距太大的如 string 到数值类型，不能用 cast 来转换）
* 拆箱
* Convert 类，.ToInt32, 类似，也有 ToString
* 实例的ToString 方法, 目标数据类型的Parse,和TryParse
* 显示类型转换操作符    -类里面，public static explicit operator 转换类名() {} 除了修饰符，其他都类似一个构造器

**左移和右移，乘二和除以二，要注意正负数的补位，以及溢出的问题，可以使用 check。**
字符串只能进行相等不相等比较，比较大小需要使用 string.Compare 方法，或者自己写。
**类型检验操作符**

* is，检验一个对象是不是某个类型的变量
* as，
  逻辑与、异或、或：& ^ |
  条件与、或： && ||
  可空类型，null 合并：

> 针对一些无法复制为 null 的数据，c#设置了可空类型，简化为？
>
> ```csharp
> int? X = null; //直接 int x = null;  编译器会报错
> //不然得写成： nullable<int> x = null; 
> int y = x ?? 1; //如果 X 是 null，那就输出 1，否则输出 X
> ```

**条件操作符：?:**
