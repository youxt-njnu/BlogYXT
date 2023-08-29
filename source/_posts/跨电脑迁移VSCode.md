---
title: 跨电脑迁移VSCode
date: 2023-08-28 15:01:36
photos: https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/20230511202603.png
tags: 
  - VSCode
  - 应用迁移
categories:
 - 开发工具
comments: true
cover: https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/20230511202603.png
---

# 直接上记录😎

参考[教程]([https://juejin.cn/post/7066622158184644621)，打开了设置同步，手工调整冲突的设置；

调整完了之后提示[VSCode版本兼容性问题](https://github.com/microsoft/vscode/issues/113439)：`Settings sync cannot be turned on because the current version is not compatible with the sync service. · Issue #113439 · microsoft/vscode (github.com)`

想着升级下不就没事了，先看看现在是啥版本的(三个点→Help→About)

![](https://s2.loli.net/2023/08/28/bAJhLxSsPtTD7GZ.png)

然后Settings里输入Update，我这里修改成了手动更新manual，重启下

![](https://s2.loli.net/2023/08/28/M65KYtDHA3OWBar.png)

左下角就能找到最后一项check for updates...，然后等待更新完。

![](F:\marktextImg\2023-08-28-15-44-04-image.png)

之后同步就打开了（我已经开了，在上图里面最下角第三项：Settings Sync is On）

问题解决✌️

# 补充|Git协同的恢复

迁移完了后，我旧环境和新环境夹杂下，准备测试下Git还能用不，一测就报错了[Visual Studio Code提交代码提示“Make sure you configure your ‘user.name‘ and ‘user.email‘ in git.”_Sun_小杰杰哇的博客-CSDN博客](https://blog.csdn.net/qq_41271930/article/details/117514127)

更完整的操作见这里：[查看或者修改本地 Git 用户名和邮箱地址 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/165646665)

```c
//修改用户名，xxx 处填写你的用户名
git config --global user.name "xxx"
//修改邮箱地址，xxx 处填写你的邮箱地址
git config --global user.email "xxx"
//查看用户名
git config user.name
//查看邮箱地址
git config user.email 
```

剩下的测试，我参考了[这篇](https://blog.csdn.net/EvelynHouseba/article/details/105426477)，以及我的记忆，最后push成功😉
