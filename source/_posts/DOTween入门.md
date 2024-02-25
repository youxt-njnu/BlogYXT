---
title: DOTween学习
tags:
 - Unity
 - DOTween
categories:
 - AR开发
comments: true
date: 2024-02-17 08:42:05
photos: https://wallroom.io/img/1920x1080/bg-f1b246b.jpg
cover: https://wallroom.io/img/1920x1080/bg-f1b246b.jpg
---
# DOTween入门

[(81条消息) DOTween 插件下载及基本使用说明_dgtweening下载_四月的小白的博客-CSDN博客](https://blog.csdn.net/LittleWhiteLv/article/details/101026616#:~:text=using%20DG.Tweening%3B%20public%20class%20Shake%20%3A%20MonoBehaviour%20%7B,0%29%2C%2030%2C%20360%2C%20false%2C%20false%29%3B%20%7D%20%7D%20%E7%84%B6%E5%90%8E%E6%8B%96%E6%8B%BD%E8%87%B3%E9%9C%80%E8%A6%81%E9%9C%87%E5%8A%A8%E7%9A%84%E7%89%A9%E4%BD%93%E4%B8%8A%E5%8D%B3%E5%8F%AF%E3%80%82)

面板Tools->Demigiant->DoTween utility panel

组件常用参数说明：

AutoPlay:自动播放动画

AutoKill:自动删除动画

Duration:动画时长

Delay:延迟时长（过一段时间再播放）

Ease:动画播放的速度曲线

Loop:循环的次数（-1表示一直执行）

LoopType:循环模式（Restart:重新开始，Yoyo:来回摆动，Incremental:增量模式）

PathType：路径模式（CatmullRom：曲线；Linear:直线）

ClosePath:路径是否首尾相连

LocalMovement:是否是本地坐标

Orientation:运动朝向（ToPath:朝向路径方向，Look At Transform:朝向物体，Look At Position：朝向坐标点）

Events中是动画各状态时的对应事件添加按钮，和Button的触发事件添加方法一样

[(81条消息) unity中的DG.Tweening详解_忽然602的博客-CSDN博客](https://blog.csdn.net/qq_20179331/article/details/130592347)

DG.Tweening, Unity插件，创建Tween动画；

```cs
using DG.Tweening;
```

主要方法：

```cs
DOTween.To().OnComplete();
Sequence sequence=DOTween.Sequence();
sequence.Append(transform.DOMoveX()); // DOTween.Sequence().Append().Append().Append();
DoTween.Delay();
DOTween.SetLoops();


Tween tween=transform.DOMoveX(10f,1f).SetEase(Ease.InOutQuint); //Tween还是Tweener
DOTween.Kill(tween);
// or
tween.SetAutoKill(false); //取消自动销毁
tween.Pause(); //设置暂停
```

```cs
door.transform.DOMoveX(); //需要把对应door添加到脚本交互界面上
door.transform.DORotate();
door.transform.DOScale();
transform.DOPlayForwards(); // 需要把脚本拖拽到对应物体上
transform.DOPlayBakwards();
transform.DOShakePosition();

Renderer renderer = GetComponent<Renderer>(); //直接获取到项目中的renderer
renderer.material.DOFade(0f, 1f);
```

新建任务面板，将脚本挂载到UI面板上，以实现UI面板的移动

```cs
public void OnClickButton() {}
```

官方文档：[DOTween - Documentation](http://dotween.demigiant.com/documentation.php#creatingSequence)

一些案例：[(81条消息) DoTween的使用与详解_牧guo的博客-CSDN博客](https://blog.csdn.net/xiaoguomumu/article/details/75243425)

一些方法：[DoTween在lua中的添加以及DoTween的常用方法 - old_Host - 博客园 (cnblogs.com)](https://www.cnblogs.com/zanzz/p/17077863.html)

官网案例：

 `<mark>`basic `</mark>`

![](F:\marktextImg\2023-07-07-17-13-49-image.png)

Cube(w animation): DOTweenAnimaton.cs

Logo: DOTweenAnimation.cs

Image: Image, DOTweenAnimation.cs

Text、Text: Text(用了rich text), DOTweenAnimation.cs

Button: Button(OnClick(), 设置了DOTweenAnimation.DOPlay()

dotween里的动画曲线：[Unity Dotween插件的运动曲线（Ease）介绍Ease选项Ease效果示例以及C#修改动画曲线功能_dotween ease_十幺卜入的博客-CSDN博客](https://blog.csdn.net/qq_33789001/article/details/124408540)

dotween里的旋转模式：[关于Dotween旋转以及OnValidate函数的解读 - 军酱不是酱 - 博客园 (cnblogs.com)](https://www.cnblogs.com/JunJiang-Blog/p/14883620.html)

unity来控制旋转和朝向：[Unity 控制物体旋转、朝向的一些方法_dolookat_幻冬的博客-CSDN博客](https://blog.csdn.net/weixin_43994445/article/details/96354427#:~:text=1%2C%20transform.DORotate%20%28%29%20transform.DORotate%28new%20Vector3%280%2C%2060%2C%200%29%2C%200.3f%29%3B,Rotation%20DOTween%E7%9A%84%2C%20%E6%B3%A8%E6%84%8F%E4%BB%96%E7%9A%84%E5%8F%82%E6%95%B0%E6%98%AF%E4%B8%AA%E4%B8%89%E5%85%83%E6%95%B0%2C%20%E8%A6%81%E6%98%AF%E6%83%B3%E4%B8%8E%E5%8F%A6%E4%B8%80%E4%B8%AA%E7%89%A9%E4%BD%93%E7%9A%84%E6%9C%9D%E5%90%91%E4%B8%80%E8%87%B4%2C%20%E5%B0%B1%3A%20transform.DORotate%28other.transform.eulerAngles%2C%200.3f%29%3B%201)
