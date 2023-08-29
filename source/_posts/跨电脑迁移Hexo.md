---
title: 跨电脑迁移Hexo
date: 2023-08-28 15:00:38
photos: https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/22862D84D2E06B14286B09ABF9A53B72.jpg
tags: 
  - Hexo
  - 个人博客
  - 应用迁移
categories:     
  - 周边扩展
comments: true
cover: https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/15A21438BD46CC6CF21998480FDA2C83.jpg
---

# 直接上记录😎

之前换新的电脑，然后给电脑安装和配置好了nodejs；

直接把旧电脑里的博客文件夹拷到了新电脑里；

参考这位作者的做法，探了探环境：[hexo博客迁移到另一台电脑_littlehaes的博客-CSDN博客](https://blog.csdn.net/littlehaes/article/details/81503455)

```
git version
node -v
npm -v
hexo -v
```

发现我没装hexo，找了一开始的[入门教程](https://juejin.cn/post/7111237168168697886)，需要注意的是npm安装需要管理员权限(详情想了解的，参考我另一篇：从win10到win11|右键菜单)

```
npm install hexo-deployer-git --save
```

随后验证了下能不能用：

```
hexo clean
hexo g
hexo d
hexo s
```

很好！一切照常，后续有问题后续再说<(￣︶￣)↗[GO!]
