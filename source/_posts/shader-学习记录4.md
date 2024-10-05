---
title: shader-学习记录Ⅳ
date: 2024-08-28 15:55:26
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

# 记录-2D Matrics

## 平移

利用u_time, sin, cos构造移动，加到st；通过cross把这种移动突出出来；

```
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;
uniform float u_time;

// _size=vec2(width,height)
float box(in vec2 _st, in vec2 _size){
    _size = vec2(0.5) - _size*0.5;
    vec2 uv = smoothstep(_size,
                        _size+vec2(0.01),
                        _st);
    uv *= smoothstep(_size,
                    _size+vec2(0.001),
                    vec2(1.0)-_st);
    return uv.x*uv.y;
}

// input: st, crossSize
float cross(in vec2 _st, float _size){
    return  box(_st, vec2(_size,_size/4.)) +
            box(_st, vec2(_size/4.,_size));
}

void main(){
    vec2 st = gl_FragCoord.xy/u_resolution.xy;
    vec3 color = vec3(0.0);

    // To move the cross we move the space
    vec2 translate = vec2(cos(u_time),sin(u_time));
    st += translate*0.3;

    // Show the coordinates of the space on the background
    color = vec3(st.x,st.y,0.0);

    // Add the shape on the foreground
    color += vec3(cross(st,0.154)); // 突出translate中心点的移动

    gl_FragColor = vec4(color,1.0);
}
```

![img](https://miro.medium.com/v2/resize:fit:632/format:webp/1*GfW3IgV8F9bvbg7OijESlA.gif)

## 旋转

```
#ifdef GL_ES
precision mediump float;
#endif

#define PI 3.14159265359

uniform vec2 u_resolution;
uniform float u_time;

mat2 rotate2d(float _angle){
    return mat2(cos(_angle),-sin(_angle),
                sin(_angle),cos(_angle));
}

float box(in vec2 _st, in vec2 _size){
    _size = vec2(0.5) - _size*0.5;
    vec2 uv = smoothstep(_size,
                        _size+vec2(0.001),
                        _st);
    uv *= smoothstep(_size,
                    _size+vec2(0.001),
                    vec2(1.0)-_st);
    return uv.x*uv.y;
}

float cross(in vec2 _st, float _size){
    return  box(_st, vec2(_size,_size/4.)) +
            box(_st, vec2(_size/4.,_size));
}

void main(){
    vec2 st = gl_FragCoord.xy/u_resolution.xy;
    vec3 color = vec3(0.0);

    // move space from the center to the vec2(0.0)
    // st从0~1变成-0.5~0.5
    st -= vec2(0.5);
    // rotate the space,左乘，传入角度
    st = rotate2d( sin(u_time)*1.*PI ) * st;
    // move it back to the original place
    st += vec2(0.5);

    // Show the coordinates of the space on the background
    color = vec3(st.x,st.y,0.0);

    // Add the shape on the foreground
    color += vec3(cross(st,0.4));

    gl_FragColor = vec4(color,1.0);
}
```

![img](https://miro.medium.com/v2/resize:fit:620/format:webp/1*JaawntaOxPEcTdTQAVGs7A.gif)

组合

```
......

void main(){
    vec2 st = gl_FragCoord.xy/u_resolution.xy;
    vec3 color = vec3(0.0);

    // To move the cross we move the space
    vec2 translate = vec2(cos(u_time),sin(u_time));
    st += translate*0.35;
  
    st-=vec2(0.5);
    st = rotate2d(u_time*PI)* st;
    st+=vec2(0.5);

    // Show the coordinates of the space on the background
    color = vec3(st.x,st.y,0.0);

    // Add the shape on the foreground
    color += vec3(cross(st,0.25));

    gl_FragColor = vec4(color,1.0);
}
```

![img](https://miro.medium.com/v2/resize:fit:620/format:webp/1*oGGqsWHMYC5C5PXEk6NUcg.gif)

缩放

```
void main(){
    vec2 st = gl_FragCoord.xy/u_resolution.xy;
    vec3 color = vec3(0.0);

    st -= vec2(0.5);
    st = scale( vec2(sin(u_time)+1.704) ) * st;
    st += vec2(0.5);

    // Show the coordinates of the space on the background
    color = vec3(st.x,st.y,0.0);

    // Add the shape on the foreground
    color += vec3(cross(st,0.2));

    gl_FragColor = vec4(color,1.0);
}
```

> 关于数字 `1.704` 的影响：
>
> 1. **缩放调整** ：
>
> * `sin(u_time) + 1.704`：这个表达式通过添加 `1.704` 来确保 `sin` 函数的结果（范围在 [-1, 1]）永远是正的，因此 `sin(u_time) + 1.704` 的结果范围大约在 [0.704, 2.704]。
> * 这个范围决定了 `scale` 函数的缩放系数，影响了 `st` 坐标的伸缩。当你增大 `1.704`，实际上是在增加最小缩放系数的基线，使动画变得不那么剧烈。
>
> 1. **视觉效果** ：
>
> * 当 `1.704` 增大时，缩放变化的幅度减少，**因为 `sin(u_time)` 的变动被一个较大的常数稳定了。这导致视觉效果的动态变化较少，从而效果不那么明显**。
>
> 1. **周期性变化** ：
>
> * `sin` 函数是周期性的，其变化对整个场景的动态变化至关重要。增大 `1.704` 的值降低了 `sin` 函数变化对整体缩放的影响，从而使得效果变化不那么显著。
>
> st = scale( vec2(-0.1) ) * st;和st = scale( vec2(0.1) ) * st;的十字效果是一样的，背景效果翻转了(左正右负)；st = scale( vec2(0.) ) * st; 这个时候页面全白；
>
> 所以缩放调整数字大于1或小于-1之下，都是小幅度的缩放；

![image.png](https://s2.loli.net/2024/08/28/dw31YJQyHomFI8A.png)

颜色空间RGB和YUV

```
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;
uniform float u_time;

// YUV to RGB matrix
mat3 yuv2rgb = mat3(1.0, 0.0, 1.13983,
                    1.0, -0.39465, -0.58060,
                    1.0, 2.03211, 0.0);

// RGB to YUV matrix
mat3 rgb2yuv = mat3(0.2126, 0.7152, 0.0722,
                    -0.09991, -0.33609, 0.43600,
                    0.615, -0.5586, -0.05639);

void main(){
    vec2 st = gl_FragCoord.xy/u_resolution;
    vec3 color = vec3(0.0);

    // UV values goes from -1 to 1
    // So we need to remap st (0.0 to 1.0)
    st -= 0.5;  // becomes -0.5 to 0.5
    st *= 2.0;  // becomes -1.0 to 1.0

    // we pass st as the y & z values of
    // a three dimensional vector to be
    // properly multiply by a 3x3 matrix
    color = yuv2rgb * vec3(0., st.x, st.y);
    //color = rgb2yuv * vec3(0., st.x, st.y);
  
    gl_FragColor = vec4(color,1.0);
}
```

![image.png](https://s2.loli.net/2024/08/28/gLFMGhyQDfzAo2k.png)

# 推荐待更新。。。

[790 HUD &amp; FUI ideas | head up display, user interface, interface design (pinterest.com)](https://www.pinterest.com/patriciogonzv/hud-fui/)

[Oblivion radar (shadertoy.com)](https://www.shadertoy.com/view/4s2SRt)

[LYGIA Shader Library](https://lygia.xyz/space)

[Patricio Gonzalez Vivo](https://patriciogonzalezvivo.com/)
