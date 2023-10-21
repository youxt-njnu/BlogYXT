---
title: 基于需求的参考~
tags:
  - AR
  - 需求
categories:
  - Unity
comments: true
date: 2023-09-11 09:46:05
photos: https://4k.wpcoder.cn/wp-content/uploads/2023/08/1_1692251675-1600x1000.png
cover: https://4k.wpcoder.cn/wp-content/uploads/2023/08/1_1692251675-1600x1000.png
---
# 图片叠加/切换

遇到图片状态切换的情况，一种是设置不同的状态选择不同的图片进行叠加，一种就是参考下面的进行图片的合成，其他后续没有思考了

[Unity中Sprite和Texture2D之间的关系。_texture2d sprite-CSDN博客](https://blog.csdn.net/wzjssssssssss/article/details/79421915)

[Unity Sprite和Texture2D互转_sprite转texture-CSDN博客](https://blog.csdn.net/iningwei/article/details/88537706)

[Unity-两张图片叠加合成一张图片_unity 图片叠加-CSDN博客](https://blog.csdn.net/Wmayy_123/article/details/103292839)

[【精选】在Unity里将多个Sprite（精灵图）动态合成一个Sprite_unity sprite 合并_mkr67n的博客-CSDN博客](https://blog.csdn.net/mkr67n/article/details/117030977)

[【精选】图片正常模式混合（透明度混合）公式_带透明度的图片混合 算法-CSDN博客](https://blog.csdn.net/mkr67n/article/details/117026093)

# 添加监听

在c#转Lua中，针对事件的监听处理遇到了麻烦，当时找的一些教程~，不过好像麻烦还是没有解决。。

[Unity 自定义事件 AddListerner_unity addlistener-CSDN博客](https://blog.csdn.net/YuAnHandSome/article/details/105579553)

[关于unity中AddListener的传参问题的研究_unity addlistener-CSDN博客](https://blog.csdn.net/qq_42097011/article/details/103712333)

# 高亮实现

后来是针对材质实现的高亮，当时也搜了下插件，但就是c#到Lua这一套还没咋实现。。。

[Unity 物体高亮实现_unity物体高亮_Maddie_Mo的博客-CSDN博客](https://blog.csdn.net/weixin_43925843/article/details/122176666)

[【Unity步步升】物体高亮的简单实现方案，如闪烁、Outline等...（插件免费）_quickoutline unity-CSDN博客](https://blog.csdn.net/PinaColadaONE/article/details/123394993)

[Unity 免费的高亮插件——QuickOutline_unity quick outline-CSDN博客](https://blog.csdn.net/f402455894/article/details/119673613)

[Highlight Plus - Unity3D物体高亮插件使用教学_zhufeng6819的博客-CSDN博客](https://blog.csdn.net/zhufeng6819/article/details/126080467)

# 卡点动画语音

之后是采用的协程，来卡的点和时间。之前想针对animation来卡，后来没实现。。。。

[Animations.AnimatorController - Unity 脚本 API](https://docs.unity.cn/cn/2020.3/ScriptReference/Animations.AnimatorController.html)

[AnimationState-time - Unity 脚本 API](https://docs.unity.cn/cn/2021.3/ScriptReference/AnimationState-time.html)

[unity-小白学习Animator建立动画的状态切换_unity animator动画切换_mr.chenyuelin的博客-CSDN博客](https://blog.csdn.net/weixin_44453949/article/details/115371362)

[Unity - Scripting API: AudioSource (unity3d.com)](https://docs.unity3d.com/ScriptReference/AudioSource.html)

# 获取物体

需要平衡是通过代码获取到物体，还是通过拖拽实现。感觉前者主要针对已经固定的那种prefab，后者灵活性更强

[Unity寻找子物体的几种方法_unity获取子物体-CSDN博客](https://blog.csdn.net/zhaihao1996/article/details/106329314)
