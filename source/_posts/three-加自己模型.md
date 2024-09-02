---
title: three+react|加自己模型
tags:
 - threejs
 - react
categories:
 - 3DGIS
 - WebGL
comments: true
date: 2024-09-02 20:37:05
photos: https://wallroom.io/img/3840x2160/bg-81f2d39.jpg
cover: https://wallroom.io/img/3840x2160/bg-81f2d39.jpg
---

# 添加自己的模型

前言，在[该项目](https://youxt-njnu.github.io/2024/02/25/three%E5%AE%9E%E6%88%981/)中加自己的模型，及问题解决；

先sketchfab上下载模型，在unity里调整位置、贴图、材质、动画，形成prefab；

利用[插件](https://github.com/Plattar/gltf-exporter?tab=readme-ov-file)，导出为gltf；"E:\PersonalCV\models\v1.142.0-unity.unitypackage"

在vite.config.ts里加入`assetsInclude: ['**/*.glb','**/*.gltf'],`

``` jsx
import { useGLTF } from "@react-three/drei"
import word2D from '../assets/3d/world_2d/world_2d.gltf'

const Map2D = ({ ...props }) => {
  const gltf = useGLTF(word2D);
  const scene = gltf.scene;

  return (
    <mesh {...props}>
      <primitive object={scene} />
    </mesh>
  )
}

export default Map2D
```

法线模型里的顶点，x的坐标都是正确数值的相反数，例如这个[模型](https://sketchfab.com/3d-models/cartoon-low-poly-world-map-e87fa1e143f84348a915b7fe1376d957)下下来就这样问题：

1. blender里，右上角切换object Mode为Edit Mode
2. A全选所有物体
3. Mesh ->Mirror->X Global
4. 按 `Ctrl + A` 并选择 `All Transforms`，应用变换

![image.png](https://s2.loli.net/2024/09/02/mZD8AeFVyHXajiz.png)

补充，针对法线反了的，1.2.4不变，3 mesh->Normals->Flip

针对贴图反了的，例如这个作者的很多[模型](https://skfb.ly/orJVW)，PS里 图像->图像旋转->垂直旋转画布/水平翻转画布