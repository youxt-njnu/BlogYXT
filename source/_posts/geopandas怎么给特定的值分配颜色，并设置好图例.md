---
title: geopandas怎么给特定的值分配颜色，并设置好图例
date: 2023-07-23 14:49:12
tags:
 - python绘图
---

# 参考链接

https://matplotlib.org/stable/api/_as_gen/matplotlib.axes.Axes.legend.html

https://kanoki.org/2020/08/30/matplotlib-scatter-plot-color-by-category-in-python/

https://github.com/geopandas/geopandas/issues/2279

https://www.sioe.cn/yingyong/yanse-rgb-16/

# 说明

坑比较多：

* 构建颜色表
* 指南针、比例尺
* 对应颜色到属性
* 自绘制图例
* 图例的属性排版

效果

![](https://cdn.jsdelivr.net/gh/youxt-njnu/blog-img/geopandas%E7%BB%98%E5%9B%BE1.png)
