---
title: C#转xlua| event?Actinon?如何转换`_map.OnUpdated += () => { _zoomSlider.value = _map.Zoom; };`
tags:
 - delegate
 - event
 - unity
categories:
 - c#
 - xlua
comments: true
date: 2023-12-10 20:03:05
photos: https://4k.wpcoder.cn/wp-content/uploads/2022/11/1_1668700001-1600x900.png
cover: https://4k.wpcoder.cn/wp-content/uploads/2022/11/1_1668700001-1600x900.png
---
# 背景

需要在unity里调用Mapbox SDK for Unity，并将Mapbox里的官方C#脚本案例修改为Xlua形式，在平台上才能实现交互。

在c#转xlua过程中，有一个event和Actiond的结构的事件变量，但不会转；此外之前对event delegate Action UnityAction这些都不了解；

1. 向GPT求助，给了我一堆名词，实在不懂问不下去了；
2. 开始想着略微系统学习下委托、事件这些，看了些教程似懂非懂；
3. 准备死马当活马医，直接网上找教程套，中文网上找了半天，有些教程就是错误的，误导我好久；
4. 转Google，看了几个教程，终于把bug除了，哭死/(ㄒoㄒ)/~~

# 解决方案

需要转换的代码结构：

```c#
public event Action OnUpdated = delegate { };
_map.OnUpdated += () => { _zoomSlider.value = _map.Zoom; };
```

解决([参考链接](https://github.com/Tencent/xLua/blob/master/Assets/XLua/Doc/XLua_Tutorial_EN.md))：(注意是单引号）

```lua
_map:OnUpdated('+', function() _zoomSlider.value = _map.Zoom; end);
```

报错：
This type must add to CSharpCallLua：typeof(UnityEngine.Events.UnityAction `<float>`), stack:......（后面还提示了其他路径，但最后查找修改的文件路径主要还是 `ExampleConfig.cs, ExampleGenConfig.cs, MessageBox.cs, `

处理：

1. [参考这个链接](https://blog.csdn.net/X_King_Q/article/details/119376546)，用UnityAction关键词查找解决方案，添加一条 `typeof(UnityEngine.Events.UnityAction<float>),`;
2. [参考这个链接](https://github.com/Tencent/xLua/issues/24)，给项目清除并重新生成的代码（Clear generate code, generate code)

感谢小白中的小白的自己，还是耐心解决了这个问题，/(ㄒoㄒ)/~~
