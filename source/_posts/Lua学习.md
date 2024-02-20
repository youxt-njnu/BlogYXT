---
title: Lua/XLua学习
tags:
 - 软件端AR
 - Lua
categories:
 - AR开发
comments: true
date: 2024-02-17 08:26:05
photos: https://4k.wpcoder.cn/wp-content/uploads/2023/09/1_1695198805-1600x780.png
cover: https://4k.wpcoder.cn/wp-content/uploads/2023/09/1_1695198805-1600x780.png
---
# Lua入门

## 语法

教程：[Lua 基本语法 - Lua中文手册 (dba.cn)](https://www.dba.cn/book/lua/LUAJiaoCheng/LUAJiBenYuFa.html)

```lua
print("hello world") 
-- note
--[[
 note
]]
```

执行

$ lua filename.lua

一般约定，以下划线开头连接一串大写字母的名字（比如 _VERSION）被保留用于 Lua 内部全局变量。

在默认情况下，变量总是认为是全局的。

全局变量不需要声明，给一个变量赋值后即创建了这个全局变量，访问一个没有初始化的全局变量也不会出错，只不过得到的结果是：nil。

如果你想删除一个全局变量，只需要将变量赋值为nil。

```lua
nil

boolean, true, false

number

string, "", [[]], str1 .. str2, #str, 

table, {}, table[key]=value, key从1开始, 

function, function end, fun2=fun1, 

type()
```

匿名函数

```lua
function anonymous(tab, fun)
    for k, v in pairs(tab) do
        print(fun(k, v))
    end
end
tab = { key1 = "val1", key2 = "val2" }
anonymous(tab, function(key, val)
    return key .. " = " .. val
end)
```

..是拼接的意思
#实现求字符串长度

```lua
a, b = 10, 2*x --多变量同时赋值
x, y = y, x -- swap 'x' for 'y'，变量交换
-- 多值赋值经常用来交换变量，或将函数调用返回给变量：
a, b = f()

t = {}
t["key"] = "www.dba.cn"
t["key"]
t.key  -- 当索引为字符串类型时的一种简化写法
```

Lua认为false和nil为假，true 和非nil为真。要注意的是Lua中 0 为 true。

```lua
[local] function fun1(x,y) 
    x=x+1
    y=y*2
    return x,y
end



if(a>b) then t=a
else t=b;
end
```

Lua 中我们可以将函数作为参数传递给函数，如下实例：

```lua
myprint = function(param)
 print("这是打印函数 - ##",param,"##")
end

function add(num1,num2,functionPrint)
 result = num1 + num2 -- 调用传递的函数参数
 functionPrint(result)
end
myprint(10)
-- myprint 函数作为参数传递
add(2,5,myprint)
```

```lua
s.upper()
s.lower()
s.gsub()
s.find()
s.reverse()
s.format()
s.char(), s.byte()
s.len()
s.rep()
```

数组

```lua
array={'a','b'}

for i= -2, 2 do --这里是直接表示i从-2到2的这个闭区间
   array[i] = i *2
end
```

0

迭代器

```lua
function square(iteratorMaxCount,currentNumber)
   if currentNumber<iteratorMaxCount
   then
      currentNumber = currentNumber+1
   return currentNumber, currentNumber*currentNumber
   end
end

for i,n in square,3,0
do
   print(i,n)
end
```

1    1
2    4
3    9

```lua
table.concat()

table.insert()

table.maxn()

table.remove()

table.sort()
```

模块

```lua
module={}
module.open=true
function module.fun1() end
return module


require("module")
module.open
module.fun1()

local m=require("module")
m.open
m.fun1()
```

> Lua 的模块是由变量、函数等已知元素组成的 table，因此创建一个模块很简单，就是创建一个 table，然后把需要导出的常量、函数放入其中，最后返回这个 table 就行。以下为创建自定义模块 module.lua
> Lua包的加载路径，需要注意默认路径和自行添加的路径：对于自定义的模块，模块文件不是放在哪个文件目录都行，函数 require 有它自己的文件路径加载策略，它会尝试从 Lua 文件或 C 程序库中加载模块。
> 也可以用C为Lua写包；

Lua元表Metatable

> setmetatable, getmetatable
> __index, __newindex
> rawset
> 加减乘除等操作, __sub
> __call, __tostring

Lua协程coroutine

> [Lua 协同程序(coroutine) - Lua中文手册 (dba.cn)](https://www.dba.cn/book/lua/LUAJiaoCheng/LUAXieTongChengXuCOROUTINE.html)

[(81条消息) 超级详细的Lua语言的基础教程_G东当的博客-CSDN博客](https://blog.csdn.net/qq_43594278/article/details/116018869)

[Lua脚本语言的使用笔记 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/126578836)

Lua中的冒号、点这些的含义：[理解lua 语言中的点、冒号与self_lua语言中两个点什么意思_hello_wangbin的博客-CSDN博客](https://blog.csdn.net/wangbin_jxust/article/details/12170233)

## 插件

[(81条消息) 新发的日常小技巧——Sublime插件安装教程（例：lua开发环境插件安装）_林新发的博客-CSDN博客](https://blog.csdn.net/linxinfa/article/details/109668372)

[sublime的lua插件 - yanzi_meng - 博客园 (cnblogs.com)](https://www.cnblogs.com/yanzi-meng/p/9557379.html)

[20 个强大的 Sublime Text 插件 - Sublime Text - 软件编程 - 深度开源 (open-open.com)](https://www.open-open.com/news/view/26d731)

[(81条消息) sublime Text3：安装相关Lua提示插件_断天涯zzz的博客-CSDN博客](https://blog.csdn.net/u013052238/article/details/79740112)

[(81条消息) 用sublime，优雅的写lua_sublime 写lua_菜鸟乎的博客-CSDN博客](https://blog.csdn.net/qq_38533963/article/details/120266107)

[华丽的使用sublime写lua~ sublime lua相关必装插件推荐~~ - MorePower - 博客园 (cnblogs.com)](https://www.cnblogs.com/cheerupforyou/p/6576861.html)

# XLua

[介绍 — XLua (tencent.github.io)](https://tencent.github.io/xLua/public/v1/guide/index.html)

[Tencent/xLua: xLua is a lua programming solution for C# ( Unity, .Net, Mono) , it supports android, ios, windows, linux, osx, etc. (github.com)](https://github.com/Tencent/xLua)

[【XLua】学习笔记 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/393839438)

[Xlua基础(三) Lua调用C# - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/159054613)

[Unity热更新框架之xLua - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/38322991)

[Xlua的热更新](https://www.cnblogs.com/BDFFZI/p/17152877.html)

实战（俄罗斯方块）：

[lua的俄罗斯方块](https://zhuanlan.zhihu.com/p/59358144)

[学习 2 天 lua 后开发俄罗斯方块的一些心得（耗时3天） - 掘金](https://juejin.cn/post/7069795082135666724)

[3.15 Lua调用CS · GitBook](https://shenjun-coder.github.io/LuaBook/%E7%AC%AC%E4%B8%89%E7%AB%A0%20XLua%E6%95%99%E7%A8%8B/3_15%20Lua%E8%B0%83%E7%94%A8CS.html)

[官方案例教程篇](https://zhuanlan.zhihu.com/p/408353700)

[Lua和C的集成](https://zhuanlan.zhihu.com/p/642523728)

## 教程

[链接](https://www.youtube.com/watch?v=Gz5W0Bo0fVc&list=PL0luF_aDUOooJvtTl7lwlR9xyAyWNZ41p&index=1)

> 相关的代码实验在LuaTutorial项目里；

只是Lua的一个插件，可以实现和c#的交互，也可以实现游戏的热更新。这样游戏出现新的功能更新到时候，用户只需要下载小部分内容，就能实现更新内容的体验。
lua不需要编译，且跨平台。c#编译成dll，然后打包成安装包。

导入xlua插件进入unity工程

> 直接把assets里的内容放到工程里就行；（把XLua的都放进去也行）
> Xlua和Plugins两个文件夹
> 学习路径：Xlua教程→Examples→
> 空间组织：Scenes文件夹，Xlua的两个文件夹(Plugins, XLua)

Lua输出的会在控制台前面有一个LUA的标识；
因为在c#里没办法识别lua的，只会识别文本类型的；所以文件命名需要在.lua后面加上.txt
此外，要确定lua文件是放在Resources的文件夹下面的，这样内置的Loader才会找到；

c#里自定义Loader加载指定目录下的lua脚本：

> 文件需要放到StreamingAssets里，而这个文件夹需要在根目录下，这样子c#里就可以找得到；

### c#里访问Lua的table

* 通过class访问的话，是值拷贝，修改不会影响lua里的数值，比较耗费性能；
* 映射到一个接口上，此时要注意添加[]，不然会报错为invalidCastException。此外，接口上对于table属性的修改，是引用修改，会同步到table上

  * 此外，如果报错说==LuaException: attemp to perform arithmetric on a table value(local 'a')== ➡️ 此时是因为在lua里第一个参数指向的是本身，所以在lua里第一个参数应该是arg/self这些表示第一个参数, 而c#调用里参数的就是实例化对象p
  * 也可以不设置第一个参数，但需要用冒号来进行定义
  * 需要重新generate code，如果还是不能运行，考虑兼容性问题，或者是需要在Xlua配置文档里学习并进行配置
* 轻量级映射：映射到C#的Dictionary或List内

  * dictionary只能映射有键值的；
  * list只能映射没有键值的数值，c#会根据定义的类型来决定获取到哪些映射值
  * 

如果文件出现了乱码问题，可以把代码文件，在记事本里另存为UTF8的格式。

如果报错说==InvalidOperationException: try to dispose a LuaEnv with C# callback!==，是因为c#有变量引用了lua的内容，但lua已经设置了释放，这就产生了冲突，无法释放。

在使用Action<int,int>的时候，报错说==InvalidCastException: This type must add to CSharpCallLua: System.Action<int, int>==，一种方式是进行配置，另一种是自己定义一个委托。因为在lua里面函数是function类型的，而c#里面函数可以说是delegate类型的。

* 但还是说InvalidCastException: This type must add to CSharpCallLua，也就说还是得添加[CSharpCallLua]，然后需要先clear generate code, 再generate code

### Lua访问C#里的内容

==new c#对象==

在C#这样new一个对象：
var newGameObj = new UnityEngine.GameObject("new gameobject");

对应到Lua是这样：
local newGameObj = CS.UnityEngine.GameObject("new gameobject")
直接CS.命名空间.类名，就可以

基本类似，除了：
**lua里头没有new关键字；**
**所有C#相关的都放到CS下，包括构造函数，静态成员属性、方法；**
**如果有多个构造函数呢？放心，xlua支持重载**，比如你要调用GameObject的带一个string参数的构造函数，这么写：

local newGameObj2 = CS.UnityEngine.GameObject('helloworld')

==访问C#静态属性，方法==

读静态属性

CS.UnityEngine.Time.deltaTime

写静态属性

CS.UnityEngine.Time.timeScale = 0.5

调用静态方法

CS.UnityEngine.GameObject.Find('helloworld')

小技巧：如果需要经常访问的类，可以先用局部变量引用后访问，除了减少敲代码的时间，还能提高性能：

local GameObject = CS.UnityEngine.GameObject

GameObject.Find('helloworld')

==访问C#成员属性，方法==

读成员属性
testobj.DMF

写成员属性
testobj.DMF = 1024

调用成员方法
注意：调用成员方法，**第一个参数需要传该对象，建议用冒号语法糖**，如下：testobj:DMFunc()

==访问父类属性，方法==

xlua支持（通过派生类）访问基类的静态属性，静态方法，（通过派生类实例）访问基类的成员属性，成员方法。

==参数的输入输出属性（out，ref）==

Lua调用测的参数处理规则：C#的普通参数算一个输入形参，ref修饰的算一个输入形参

out修饰的不算，然后从左往右对应lua 调用测的实参列表；

Lua调用测的返回值处理规则：C#函数的返回值（如果有的话）算一个返回值，out算一个返回值，ref算一个返回值，然后从左往右对应lua的多返回值。

==重载方法==

直接通过不同的参数类型进行重载函数的访问，例如：

testobj:TestFunc(100)

testobj:TestFunc('hello')

将分别访问整数参数的TestFunc和字符串参数的TestFunc。

注意：xlua只一定程度上支持重载函数的调用，因为lua的类型远远不如C#丰富，存在一对多的情况，比如C#的int，float，double都对应于lua的number，上面的例子中TestFunc如果有这些重载参数，第一行将无法区分开来，只能调用到其中一个（生成代码

中排前面的那个）。

==操作符==

支持的操作符有：+，-，*，/，==，一元 -，<，<=， %，[]

==参数带默认值的方法==

和C#调用有默认值参数的函数一样，如果所给的实参少于形参，则会用默认值补上。

==可变参数方法==

对于C#的如下方法：void VariableParamsFunc(int a, params string[] strs)

可以在lua里头这样调用：testobj:VariableParamsFunc(5, 'hello', 'john')

==使用Extension methods==

在C#里定义了，lua里就能直接使用。

==泛化（模版）方法==

**不直接支持，可以通过Extension methods功能进行封装后调用。**

==枚举类型==

枚举值就像枚举类型下的静态属性一样。

testobj:EnumTestFunc(CS.Tutorial.TestEnum.E1)

上面的EnumTestFunc函数参数是Tutorial.TestEnum类型的

另外，如果枚举类加入到生成代码**（就是在c#的枚举之前，添加[LuaCallCSharp]）**的话，枚举类将支持__CastFrom方法，可以实现从一个整数或者字符串到枚举值的转换，例如：

CS.Tutorial.TestEnum.__CastFrom(1)

CS.Tutorial.TestEnum.__CastFrom(‘E1')

==delegate使用（调用，+，-）==

C#的delegate调用：和调用普通lua函数一样

+操作符：对应C#的+操作符，把两个调用串成一个调用链，右操作数可以是同类型的C#delegate或者是lua函数。

-操作符：和+相反，把一个delegate从调用链中移除。

Ps：delegate属性可以用一个luafunction来赋值。

`<mark>`event`</mark>`

比如testobj里头有个事件定义是这样：public event Action TestEvent;

增加事件回调 testobj:TestEvent('+', lua_event_callback)

移除事件回调 testobj:TestEvent('-', lua_event_callback)

`<mark>`C#复杂类型和table的自动转换`</mark>`

对于一个有无参构造函数的C#复杂类型，在lua侧可以直接用一个table来代替，该table对应复杂类型的public字段有相应字段即可，支持函数参数传递，属性赋值等，例如：

C#下B结构体（class也支持）定义如下：

public struct A{ public int a;}

public struct B{ public A b; public double c;}

某个类有成员函数如下：void Foo(B b)

在lua可以这么调用：

obj:Foo({b = {a = 100}, c = 200})

获取类型（相当于C#的typeof）

比如要获取UnityEngine.ParticleSystem类的Type信息，可以这样 typeof(CS.UnityEngine.ParticleSystem)。

`<mark>`“强”转`</mark>`

lua没类型，所以不会有强类型语言的“强转”，但有个有点像的东西：告诉xlua要用指定的生成代码去调用一个对象，这在什么情况下能用到呢？有的时候第三方库对外暴露的是一个interface或者抽象类，实现类是隐藏的，这样我们无法对实现类进行代码生成。该实现类将会被xlua识别为未生成代码而用反射来访问，如果这个调用是很频繁的话还是很影响性能的，这时我们就可以把这个interface或者抽象类加到生成代码，然后指定用该生成代码来访问：

cast(calc, typeof(CS.Tutorial.Calc))

上面就是指定用CS.Tutorial.Calc的生成代码来访问calc对象。

==GetComponent()==
[xLua下调用GetComponent时返回值不是nil的坑_xlua trygetcomponet-CSDN博客](https://blog.csdn.net/gouki04/article/details/84784795)
[XLua 调用 unity 中获取组件的方法 - OldWu - 博客园 (cnblogs.com)](https://www.cnblogs.com/ProjectDeveloping/p/10765496.html)

### 热更新流程

写的lua代码是关联在AssetBundle上的，然后打包一起更新资源和逻辑到服务器上的项目中。

# 注意点

[VSCode 如何将 .lua.txt 后缀文件识别为 lua_lua txt后缀-CSDN博客](https://blog.csdn.net/ddd2213sss/article/details/126403982)

## 协程的调整

参考对ReloadMap.cs的改写

```lua
local _waitSecond = nil; 
_waitSecond = 0.3; 
coroutine.yield(UnityEngine.WaitForSeconds(_waitSecond));
```

相当于c#里的下面三句话：

```c#
WaitForSeconds _wait; 
_wait = new WaitForSeconds(.3f); 
yield return _wait;
```

## 数据类型的变换

ua脚本语言不像C语言一样，整型除以整型结果为整型。lua得出的结果是浮点型，有时会用到整型，[所以必须进行取整](https://blog.csdn.net/FightIngWork/article/details/89281084)。

> 直接用lua math库函数
> math.floor(s）输入参数为浮点型，整型都可

## 委托、事件

[【Unity知识点】通俗解释delegate，事件event，Action，Func和UnityAction，UnityEvent_unity delegate-CSDN博客](https://blog.csdn.net/boyZhenGui/article/details/120956981)

[Unity 事件番外篇：UnityEvent-CSDN博客](https://blog.csdn.net/qq_46044366/article/details/122806863)

XLua的API文档

https://zhuanlan.zhihu.com/p/408353700

[Lua实现事件触发机制 - 简书 (jianshu.com)](https://www.jianshu.com/p/118d6c421549)

https://blog.csdn.net/weixin_41995872/article/details/97806978

[使用Lua语言实现简单的事件系统_lua语言互斥实现-CSDN博客](https://blog.csdn.net/yiluxiangbei000/article/details/118723955)

委托delegate

> 可以被赋值，注册和注销, 多播委托+= -=. ?的使用表示非空则执行
> 《C#图解教程》
> 委托：持有一个或多个方法，以及一系列预定义操作的，用户定义的类型

事件event

> 对委托的一个限定，不需要被赋值，也不能被赋值，可以注册和注销方法。也可以注册和注销委托

Action
Action和Func可以理解为系统定义好的带泛型的delegate， Action无返回值

Func
Action和Func可以理解为系统定义好的带泛型的delegate, Func有无返回值

UnityAction
Unity对C#中Action的再次封装, 更适合在Unity中使用的一种泛型委托. UnityAction对象可以用于Unity内的.AddListener()

UnityEvent

可以在面板中添加监听事件，也可以在代码中添加监听事件或UnityAction, 且这两个模式不会互通

## 问题

```c#
public event Action OnUpdated = delegate { };
_map.OnUpdated += () => { _zoomSlider.value = _map.Zoom; };
```

这一句转换不过来，解决([参考链接](https://github.com/Tencent/xLua/blob/master/Assets/XLua/Doc/XLua_Tutorial_EN.md))：(注意是单引号）

```lua
_map:OnUpdated('+', function() _zoomSlider.value = _map.Zoom; end);
```

报错：
This type must add to CSharpCallLua：typeof(UnityEngine.Events.UnityAction`<float>`), stack:......（后面还提示了其他路径，但最后查找修改的文件路径主要还是 `ExampleConfig.cs, ExampleGenConfig.cs, MessageBox.cs, `
处理：

1. https://blog.csdn.net/X_King_Q/article/details/119376546，参考这个链接，用UnityAction关键词查找解决方案，添加一条 `typeof(UnityEngine.Events.UnityAction<float>),`;
2. https://github.com/Tencent/xLua/issues/24，参考这个链接，给项目清除并重新生成的代码（Clear generate code, generate code)
3. 经验：中文网站上查查，实现不了就转Google, 有些教程就是错误的，在那里误导我；

如果报错提示：

```
LuaException: c# exception:Non-static method requires a target.,stack:  at System.Reflection.MonoMethod.Invoke (System.Object obj, System.Reflection.BindingFlags invokeAttr, System.Reflection.Binder binder, System.Object[] parameters, System.Globalization.CultureInfo culture) [0x0004c] in <695d1cc93cca45069c528c15c9fdd749>:0 
  at System.Reflection.MethodBase.Invoke (System.Object obj, System.Object[] parameters) [0x00000] in <695d1cc93cca45069c528c15c9fdd749>:0 
  at XLua.OverloadMethodWrap.Call (System.IntPtr L) [0x001ab] in E:\Internship\mapboxText\Assets\Retinar\XLua\Src\MethodWarpsCache.cs:242 
  at XLua.MethodWrap.Call (System.IntPtr L) [0x00029] in E:\Internship\mapboxText\Assets\Retinar\XLua\Src\MethodWarpsCache.cs:297 
stack traceback:
	[C]: in field 'GetUrl'
	[string "MapOperation.lua"]:84: in function <[string "MapOperation.lua"]:76>
```

一个是：LuaException: c# exception:Non-static method requires a target.，然后仔细看到最后，提示了[C]: in field 'GetUrl'	[string "MapOperation.lua"]:84: in function <[string "MapOperation.lua"]:76>，最后发现是_resource.GetUrl()错了，应该是_resource:GetUrl()

### lua调用c#里的泛型

有一句话转不过来(==具体细节可以看问题梳理图示.pptx==)，
c#:

```c#
_resource = new ForwardGeocodeResource("");

MapboxAccess.Instance.Geocoder.Geocode(_resource, HandleGeocoderResponse);

public IAsyncRequest Geocode<T>(GeocodeResource<T> geocode, Action<ReverseGeocodeResponse> callback) { }

public IAsyncRequest Geocode(string geocodeUrl, Action<ReverseGeocodeResponse> callback)
```

Lua:

```lua
_resource = Geocoding.ForwardGeocodeResource("Nanjing");

local geocoder = MapboxAccess.Instance.Geocoder; 

geocoder:Geocode(_resource:GetUrl(), HandleGeocoderResponse);

```

应该是数据类型不对，但就是不知道怎么要整才算对的
我一级级往上，一直到Deserialize这个函数，我的那个就报错了

==解决方法：==
重写一遍C#里的泛型：

```c#
public IAsyncRequest Geocode<T>(GeocodeResource<T> geocode, Action<ForwardGeocodeResponse> callback)
		{
			return this.fileSource.Request(
				geocode.GetUrl(),
				(Response response) =>
				{
					var str = Encoding.UTF8.GetString(response.Data);

					var data = Deserialize<ForwardGeocodeResponse>(str);

					callback(data);
				});
		}

        public IAsyncRequest Geocode(GeocodeResource<string> geocode, Action<ForwardGeocodeResponse> callback)
        {
            return this.fileSource.Request(
                geocode.GetUrl(),
                (Response response) =>
                {
                    var str = Encoding.UTF8.GetString(response.Data);
                    var data = Deserialize<ForwardGeocodeResponse>(str);
                    callback(data);
                });
        }
```

[简单的重写](https://www.bilibili.com/read/cv14380732/)
[对c#泛型方法对应的类，添加扩展](https://zhuanlan.zhihu.com/p/130860627)

### 报错

[未将对象设置到对象实例 Object reference not set to an instance of an object-CSDN博客](https://blog.csdn.net/qq_42672770/article/details/109669424)

### 不懂的

c#里添加自定义loader，对于delegate变量的定义；

```c#
void Start()
{
    LuaEnv env = new LuaEnv();
    env.AddLoader(MyLoader);//参数为LuaEnv.customLoader loader
    //要求是LuaEnv.customLoader类型的，也就是下面这个类型的byte[]
    //public delegate byte[] CustomLoader(ref string filepath);
}

byte[] MyLoader(ref string filepath)
{
     return null;
}
```
