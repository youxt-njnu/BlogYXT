---
title: shader-学习记录Ⅴ
date: 2024-09-04 09:40:26
tags: 
  - shader
  - webgl
categories: 
  - 图形学
photos: https://wallroom.io/img/5120x2880/bg-d896c9d.jpg
cover: https://wallroom.io/img/5120x2880/bg-d896c9d.jpg
---

# 前言

学习资料：[The Book of Shaders: Shaping functions](https://thebookofshaders.com/05/)

# 记录-patterns

fract(),取小数部分；

实现pattern的绘制：

``` glsl

#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;
uniform float u_time;

float circle(in vec2 _st, in float _radius){
    vec2 l = _st-vec2(0.5);
    return 1.-smoothstep(_radius-(_radius*0.01),
                        _radius+(_radius*0.01),
                         dot(l,l)*4.0);
}

vec3 pattern2d(in float x, in float y) {
    vec2 st = gl_FragCoord.xy/u_resolution;
    vec3 color = vec3(0.0);

    st.x *= x;   
    st.y *= y; // Scale up the space by 3
    st = fract(st); // Wrap around 1.0
    
    color = vec3(st,0.0);
    color = vec3(circle(st,0.5));
    return color;
}

void main() {
	vec3 color = pattern2d(2.1,5.);
	gl_FragColor = vec4(color,1.0);
}
```

可以在pattern上，结合旋转、平移、缩放，来丰富pattern

判断奇偶行，可以通过mod取余，加上三元运算符或step，来形成0和1的判断；

Truchet Tiles的绘制，通过给不同的区间设置index，根据index来判断旋转多少度；

# 记录-random

rand()得到的类似于fract(sin(...))，在一片随机内，部分地方存在着较大波动；

利用fract, sin, dot来实现2D下的随机；

可以构建grid形态下的随机灰度填充；

# 记录-noise

mix()进行线性插值；

在一些 noise 的应用中你会发现程序员喜欢用他们自己的三次多项式函数（func），而不是用 smoothstep()。

mix(rand(i),rand(i+1.0),func);

二维插值；

value noise和gradient noise的区别； simplex noise

结合distance field来绘制；

三维插值；

# 记录-Cellular Noise

求纹理空间下的坐标，到指定坐标的最小距离，加到颜色上，进行呈现；

类似于Voronoi图；

通过遍历周边8个格网，进行综合；

Voronoi算法；

voro-nose;


# 记录-Fractal Brownian Motion/Fractal Noise

利用sin函数，修改frequency和amplitude

> 通过在循环（循环次数为 octaves，一次循环为一个八度）中叠加噪声，并以一定的倍数（lacunarity，间隙度）连续升高频率，同时以一定的比例（gain，增益）降低 噪声 的振幅，最终的结果会有更好的细节。这项技术叫“分形布朗运动（fractal Brownian Motion）”（fBM），或者“分形噪声（fractal noise）”

> 这项技术被广泛地应用于构造程序化风景。fBm 的自相似性能够很完美地模拟山脉，因为山脉形成过程中的腐蚀形成了这种不同尺度上的自相似性

fbm构造山谷和山脊 —— turbulence效果

使用fbm模拟fbm —— 云的效果


# 其他


图像处理

* 纹理
* 图像处理
* 卷积核
* 滤镜
* 其他效果

模拟

* 乒乓
* Conway生命游戏
* 水波
* 水彩
* 反应扩散

3D 图形

* 灯光
* 法线贴图
* 凹凸贴图
* Ray marching
* 环境贴图 (spherical and cube)
* 折射和反射
