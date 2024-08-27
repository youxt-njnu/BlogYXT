---
title: shader-学习记录Ⅲ
date: 2024-08-23 14:33:26
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

# 记录-shapes

step(float edge, float x)类似于if

vec3(float * float) 相当于把两个进行and

```
uniform vec2 u_resolution;

void main(){
    vec2 st = gl_FragCoord.xy/u_resolution.xy;
    vec3 color = vec3(0.0);

    // Each result will return 1.0 (white) or 0.0 (black).
    float left = step(0.1,st.x);   // Similar to ( X greater than 0.1 )
    float bottom = step(0.1,st.y); // Similar to ( Y greater than 0.1 )

    // The multiplication of left*bottom will be similar to the logical AND.
    color = vec3( left * bottom );

    gl_FragColor = vec4(color,1.0);
}
```

可以把使用这两个left, bottom，简化为使用1个

```
vec2 bl = step(vec2(0.1),st)
```

应用

```
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;
uniform vec2 u_mouse;
uniform float u_time;

void main(){
    vec2 st = gl_FragCoord.xy/u_resolution.xy;
    vec3 color = vec3(0.0);

    // bottom-left
    vec2 bl = step(vec2(0.1),st);
    float pct = bl.x * bl.y;

    // top-right
    // vec2 tr = step(vec2(0.1),1.0-st);
    // pct *= tr.x * tr.y;

    color = vec3(pct);

    gl_FragColor = vec4(color,1.0);
}
```

![image.png](https://s2.loli.net/2024/08/23/1IsKBDcLtamyz4v.png)

smoothstep()的形状效果：边缘更平缓

floor()的形状效果：阶梯式

![image.png](https://s2.loli.net/2024/08/23/lWRELpChy643jBM.png)

绘制圆：

* 参考圆规的思路，以st中心点为圆心，判断st范围内的每个点，是否在圆的范围内；—— distance(), sqrt(), length()
* 通过step切出来

```glsl
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;
uniform vec2 u_mouse;
uniform float u_time;

void main(){
	 vec2 st = gl_FragCoord.xy/u_resolution;
    float pct = 0.0;

    // a. The DISTANCE from the pixel to the center
    pct = distance(st,vec2(0.5));

    // b. The LENGTH of the vector
    //    from the pixel to the center
    // vec2 toCenter = vec2(0.5)-st;
    // pct = length(toCenter);

    // c. The SQUARE ROOT of the vector
    //    from the pixel to the center
    // vec2 tC = vec2(0.5)-st;
    // pct = sqrt(tC.x*tC.x+tC.y*tC.y);
  
    float clipRes = 1.-step(0.5,pct);

    vec3 color = vec3(pct);
    // color = vec3(1.-clipRes*pct);

	gl_FragColor = vec4( color, 1.0 );
}
```

左边是三种方法求距离后的color，右边是加入了step和1.-的使用；

![image.png](https://s2.loli.net/2024/08/26/jvztG13xlTeMI7W.png)

“distance field”，类似缓冲区；

```
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;
uniform vec2 u_mouse;
uniform float u_time;

void main(){
  vec2 st = gl_FragCoord.xy/u_resolution.xy;
  // st.x *= u_resolution.x/u_resolution.y;
  vec3 color = vec3(0.0);
  float d = 0.0;

  // Remap the space to -1. to 1.
  st = st *2.-1.;

  // Make the distance field
  d = length( abs(st)-.3 );
  //d = length( min(abs(st)-.3,0.) );
  //d = length( max(abs(st)-.3,0.) );

  // Visualize the distance field
  gl_FragColor = vec4(vec3(d),1.0);
  // gl_FragColor = vec4(vec3(fract(d*12.216)),1.0); // *的数值越大，d略微变化下呈现的小数部分就更多样

  // Drawing with the distance field
  gl_FragColor = vec4(vec3( step(.3,d) ),1.0);
  gl_FragColor = vec4(vec3( step(.3,d) * step(d,.4)),1.0);
  gl_FragColor = vec4(vec3( smoothstep(.3,.4,d)* smoothstep(.6,.5,d)) ,1.0);
}
```


![img](https://s2.loli.net/2024/08/27/CZM4AR9LVQlF3e2.png)

不同曲线设置下的形状表现，可以用来做花朵、雪花、齿轮这些。。。

```
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;
uniform vec2 u_mouse;
uniform float u_time;

void main(){
    vec2 st = gl_FragCoord.xy/u_resolution.xy;
    vec3 color = vec3(0.0);

    vec2 pos = vec2(0.5)-st; // -0.5~0.5

    float r = length(pos)*2.0;
    float a = atan(pos.y,pos.x);

    float f = cos(a*3.);
    f = abs(cos(a*3.));
   f = abs(cos(a*2.5))*0.236+0.116;
   f = abs(cos(a*12.)*sin(a*3.))*.8+.1;
    f = smoothstep(-.5,1., cos(a*10.))*0.2+0.5;

    //color = vec3(smoothstep(f,f+.01,r));
    color = vec3(1.-smoothstep(f,f+0.01,r) );

    gl_FragColor = vec4(color, 1.0);
}
```

![image.png](https://s2.loli.net/2024/08/27/LoCVqmx6WvzeMKh.png)

# 推荐待更新。。。

实现下面的效果：

![img](https://thebookofshaders.com/07/mondrian.jpg)


[Square shaped shaders | thndl](https://thndl.com/square-shaped-shaders.html)

https://mstdn.thndl.com/@baldand

https://www.mattdesl.com/

lygia: [LYGIA Shader Library](https://lygia.xyz/draw), [LYGIA Shader Library sdf函数](https://lygia.xyz/draw)

[像素精神 (pixelspiritdeck.com)](https://pixelspiritdeck.com/)
