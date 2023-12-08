---
title: Geospatial API学习
tags:
 - 软件端AR
 - ARCore
categories:
 - AR开发
comments: true
date: 2023-10-21 21:33:05
photos: https://4k.wpcoder.cn/wp-content/uploads/2023/09/1_1695198805-1600x780.png
cover: https://4k.wpcoder.cn/wp-content/uploads/2023/09/1_1695198805-1600x780.png
---
# 官方网站

Google AR Developer: [https://developers.google.com/ar?hl=en](https://developers.google.com/ar?hl=en)

Google's Immersive Geospaial Challenge: [https://googlesimmersive.devpost.com/](https://googlesimmersive.devpost.com/)


# 在线赛

## 要求

our latest competition where we invite you to either use Geospatial Creator to build an augmented reality experience in Adobe Aero or Unity, or use Photorealistic 3D Tiles to create a 3D virtual, immersive experience.

可以基于栖霞山或者是你以前的数据，也可以利用cesium做一个，就是参考以前的（因为有GIS基础）


## 学习

找一个网上的，使用geospatial creator进行创作的一个例子；

1. 官网提供[案例](https://www.youtube.com/watch?v=MDcyG9MAMAo)

场景设计（就可以参考mapbox，cesium那边的案例）：

* 基本地图（看情况设置精度）
* 特定地点（点击查看高清晰地图和模型）
* 专题地图（左下角按钮切换呈现，例如三维的人口密度图，地形阶梯图这种类似的）
* 加入一些GIS里的缓冲区这种的分析

理想状态下，给到的API也就类似他们的一个C#脚本，然后在脚本中修改参数，实现对应的呈现。

#### GET STARTED WITH GEOSPATIAL CREATOR ![](https://res.cloudinary.com/devpost/image/fetch/s--8hQxSUpC--/c_limit,f_auto,fl_lossy,q_auto:eco,w_900/https://a.slack-edge.com/production-standard-emoji-assets/14.0/apple-large/1f933.png)

* [Register for the hackathon](https://googlesimmersive.devpost.com/register?flow%5Bdata%5D%5Bchallenge_id%5D=19201&flow%5Bname%5D=register_for_challenge)
* To get started in Unity, download the [ARCore Extensions for AR Foundation](https://developers.google.com/ar/develop/unity-arf/getting-started-extensions), and follow the instructions [here](https://developers.google.com/ar/geospatialcreator/unity/quickstart)
* To get started in Adobe Aero, sign up for the [Adobe Aero Geospatial Pre-release](https://forms.office.com/pages/responsepage.aspx?id=Wht7-jR7h0OUrtLBeN7O4cBei4bwCXlKnlkBhxWTf-JUNE9RRU1RQlEyVkVETzUzWVNRS0hPSjFPVS4u), and submit a form [here](https://forms.gle/UxhasZxtCoFFVBSq5) for confirmation
* Learn about the [tools you could use for your project](https://googlesimmersive.devpost.com/resources)
* Start Brainstorming

#### GET STARTED WITH PHOTOREALISTIC 3D TILES ![](https://res.cloudinary.com/devpost/image/fetch/s--GD3abraw--/c_limit,f_auto,fl_lossy,q_auto:eco,w_900/https://a.slack-edge.com/production-standard-emoji-assets/14.0/apple-large/1f30f.png)

* [Register for the hackathon](https://googlesimmersive.devpost.com/register?flow%5Bdata%5D%5Bchallenge_id%5D=19201&flow%5Bname%5D=register_for_challenge)
* Set up your [Google Cloud account](https://console.cloud.google.com/google/maps-apis/start?utm_referrer=https%3A%2F%2Fwww.google.com%2F)
* Learn about the [tools you could use for your project](https://googlesimmersive.devpost.com/resources)
* Start Brainstorming

how you can use 3D city imagery, heat mapping, layered visualizations, and other customizations using Photorealistic 3D Tiles.

可以使用兼容的渲染器和可视化库来渲染我们的3 d Tiles，比如 CesiumJS 和 deck.gl

[参考案例1（故事地图）](https://3dtiles.carto.com/)
