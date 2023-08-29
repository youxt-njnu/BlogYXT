---
title: 个人博客搭建汇总Hexo+Github
date: 2023-04-30 15:53:26
photos: https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/22862D84D2E06B14286B09ABF9A53B72.jpg
tags: 
  - Hexo
  - 个人博客
categories:     
  - 周边扩展
comments: true
cover: https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/15A21438BD46CC6CF21998480FDA2C83.jpg
---

# 前置信息

[wordpress建站](WordPress为什么免费？搭建WordPress网站要花多少钱？ - 窝小力的文章 - 知乎 https://zhuanlan.zhihu.com/p/91557826)

阿里云学生机：[link1](https://developer.aliyun.com/article/872989)、[link2](https://developer.aliyun.com/plan/student)、[link3](https://developer.aliyun.com/article/766669)

[优秀博客订阅](https://www.ohyee.cc/friends)

[张鑫旭博客](https://www.zhangxinxu.com/)

[廖雪峰博客](https://www.liaoxuefeng.com/)

[网站集合](http://www.webhub123.com/#/home/more)

[运维、linux](https://www.lutixia.cn/)

# 重要代码

```python
hexo clean #清除缓存文件db.json和已生成的静态文件public
hexo g #生成网站静态文件到默认设置的 public 文件夹(hexo generate 的缩写)
hexo d #自动生成网站静态文件，并部署到设定的仓库(hexo deploy 的缩写)
hexo s # 启动服务预览，hexo server

hexo n "article title" #新建一篇文章,hexo new post

hexo server #Hexo会监视文件变动并自动更新，无须重启服务器
hexo server -s #静态模式
hexo server -p 5000 #更改端口
hexo server -i 192.168.1.1 #自定义 IP
```

# 博客构建

[hexo+butterfly，搭建博客](https://juejin.cn/post/7111237168168697886)：作者个人博客推荐、nodejs、Github、个人域名、Butterfly主题
[hexo+Github搭建博客](https://zhuanlan.zhihu.com/p/35668237)：内容和上一篇类似，但更详细了些，对应的是matery主题
[GitHub+hexo](https://zhuanlan.zhihu.com/p/26625249)：里面有一些基本命令，对应的是Next主题，也介绍了图床、markdown语法

[自动备份Hexo中的博客源文件](https://cloud.tencent.com/developer/article/1964356)：[手动备份链接](https://blog.51cto.com/u_12877374/2853805)（针对上面链接中的备份博客源文件进行了细化），此外，介绍了自动备份的方案

[GitHub+PicGo构建图床](https://zhuanlan.zhihu.com/p/353775844)

[对于butterfly相关的配置](https://www.antmoe.com/posts/75a6347a/)、[也是相关设置](https://butterfly.js.org/posts/4aa8abbe/#Tabs)

[图标](https://fontawesome.com/)

[valine评论系统设置](https://www.iszy.cc/posts/Valine/)

[自定义侧边栏](https://butterfly.js.org/posts/ea33ab97/#%E4%BE%8B%E5%AD%90)

[在butterfly主题中添加rss订阅 | 老猫的博客 (dclef.icu)](https://dclef.icu/2022/09/24/%E5%9C%A8butterfly%E4%B8%BB%E9%A2%98%E4%B8%AD%E6%B7%BB%E5%8A%A0rss%E8%AE%A2%E9%98%85/)

手动更新：

```python
git add .
git commit -m "updateinf"
# git pull --rebase origin master
git push -u origin master
```

Page Front-matter

```
MARKDOWN---
title:
date:
type: （tags,link,categories这三个页面需要配置）
comments: (是否需要显示评论，默认true)
description:
top_img: (设置顶部图)
mathjax:
katex:
---
```

Post Front-matter

```
---
title:
date:
tags:
categories:
keywords:
description:
top_img: （除非特定需要，可以不写）
comments  是否显示评论（除非设置false,可以不写）
cover:  缩略图
toc:  是否显示toc （除非特定文章设置，可以不写）
toc_number: 是否显示toc数字 （除非特定文章设置，可以不写）
copyright: 是否显示版权 （除非特定文章设置，可以不写）
mathjax:
katex:
hide:
---
```

# 目录结构

参考：[hexo博客迁移到另一台电脑_littlehaes的博客-CSDN博客](https://blog.csdn.net/littlehaes/article/details/81503455)

![](https://s2.loli.net/2023/08/28/RvFk7qQjOT5IceG.png)

# 错误处理

[YAMLException: can not read a block mapping entry; a multiline key may not be an implicit key](https://blog.csdn.net/swy_swy_swy/article/details/105326420)：md文档中的头部信息里，键值对之间需要有空格

[unexpected token问题](https://one-more-tech.gitlab.io/Hexo-%E9%97%AE%E9%A2%98%E6%B1%87%E6%80%BB,%E4%BD%BF%E7%94%A8html%E5%86%99%E6%96%87%E7%AB%A0,unexpected-token%E7%AD%89%E9%97%AE%E9%A2%98/index/)

[Hexo部署错误：err: Error: Spawn failed_error: spawn failed at childprocess.&lt;anonymous&gt; (/_cuntou0906的博客-CSDN博客](https://blog.csdn.net/weixin_44231148/article/details/124075537)

# 未整理

https://www.antmoe.com/posts/75a6347a/

看到了评论的设置这一块

https://blog.51cto.com/u_15127636/3257851
https://butterfly.js.org/
https://winney07.github.io/2018/08/02/%E5%9C%A8Hexo%E5%8D%9A%E5%AE%A2%E4%B8%AD%E5%8F%91%E5%B8%83%E6%96%87%E7%AB%A0/
https://winney07.github.io/2018/08/02/%E5%9C%A8Hexo%E5%8D%9A%E5%AE%A2%E4%B8%AD%E5%8F%91%E5%B8%83%E6%96%87%E7%AB%A0/
https://bore.vip/archives/5b629e16/
https://butterfly.js.org/posts/4aa8abbe/#%E6%96%87%E7%AB%A0%E5%B0%81%E9%9D%A2
