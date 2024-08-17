---
title: shader-学习记录Ⅰ
date: 2024-08-16 15:52:26
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

[GLSL Editor (thebookofshaders.com)](https://thebookofshaders.com/edit.php) 在线的shader编辑调试

[patriciogonzalezvivo/glslGallery (github.com)](https://github.com/patriciogonzalezvivo/glslGallery) 看着不错，但是提交自己的上去后，提交不到对应的服务器（直白些就是俺试完了用不了。。。。。

[GLSL Gallery (patriciogonzalezvivo.github.io)](https://patriciogonzalezvivo.github.io/glslGallery/) 作者的一些shader，可以借鉴学习

# 记录

## shader

GPU特点：blind（不知道其他的pipe/thread在干啥），memoryless（不知道以前干了啥）

片元着色器：

* void main() {} 主函数
* 内置全局变量，如gl_FragColor
* 默认输出：gl_fragColor
* 默认输入：gl_fragCoord  (是varing)     //    x,y都从0到1
* 数据类型，vec4，精度如float
* 标准化normalized, 映射map
* 宏，#defines, #ifdef, #endif, #ifndef
* 注意，对于浮点的数字最好养成加上.的习惯

uniform:

* cpu给gpu的
* 一些所有thread都一样的，不会变的值

支持内置数学函数，如sin()

## shaping functions

### step

The [`step()`](https://thebookofshaders.com/glossary/?search=step) interpolation receives two parameters. 

The first one is the limit or threshold, while the second one is the value we want to check or pass. 

Any value under the limit will return `0.0` while everything above the limit will return `1.0`.

### smoothstep

[【Shader Graph】SmoothStep节点详解及其应用-CSDN博客](https://blog.csdn.net/weixin_61427881/article/details/127839417)

### sin/cos

加上u_time: 实现沿x轴的移动

乘上PI类似的：实现曲线的收缩

加上值：实现曲线上下移动

外面乘上值：实现曲线拉伸

取绝对值abs()：弹跳球轨迹

取小数部分fract()

取上下整数ceil(), floor()

y = mod(x,0.5); // return x modulo of 0.5
y = fract(x); // return only the fraction part of a number
y = ceil(x);  // nearest integer that is greater than or equal to x
y = floor(x); // nearest integer less than or equal to x
y = sign(x);  // extract the sign of x
y = abs(x);   // return the absolute value of x
y = clamp(x,0.0,1.0); // constrain x to lie between 0.0 and 1.0
y = min(0.0,x);   // return the lesser of x and 0.0
y = max(0.0,x);   // return the greater of x and 0.0

搜索关键词：generative art

> [Generative Art: 50 Best Examples, Tools &amp; Artists (2021 GUIDE) — AIArtists.org](https://aiartists.org/generative-art-design)


# 几个shader

## **鼠标坐标**


```glsl
// 03 code2
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution; // 屏幕 宽，高
uniform vec2 u_mouse;
uniform float u_time;

void main() {
   vec2 mousePos;
// vec2 st = gl_FragCoord.xy/u_resolution; // 把输入纹理坐标缩放到0~1
// gl_FragColor = vec4(st.x,st.y,0.0,1.0);
vec2 center = u_resolution/2.; // 获取中心点的屏幕坐标
   mousePos = (u_mouse.xy-center)/center; // 把鼠标位置缩放到-1~1
   mousePos = mousePos*0.5 + 0.5; // 把鼠标位置缩放到0~1
   gl_FragColor = vec4(mousePos.x,mousePos.y,0.0,1.0);
}
```

## **刷油漆**

* 获取从0到1空间坐标
* 做出0到1渐变灰
* 制造函数和对应区间
* 把函数涂上颜色

![image.png](https://s2.loli.net/2024/08/16/p9GByiLT3MqlPCS.png)

```glsl
// 05 code1
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;
uniform vec2 u_mouse;
uniform float u_time;

// 返回值区间：
// abs(y-x)===0, return 1
// abs(y-x) 0~0.02, return abs(y-x)
// abs(y-x) >0.02, return 0
float plot(vec2 st) {
   return smoothstep(0.02,0.0,abs(st.y-st.x));
}
void main() {
 vec2 st = gl_FragCoord.xy/u_resolution;
    float y = st.x;
    vec3 color = vec3(y);
    // 每个点只有x起作用，所以x一样的，显示的颜色是一样的；
    // color由三个x组成，所以是灰度，而且渐变大
    
    float line = plot(st);
    // color = line*vec3(.0,1.,.0); // 绘制出了绿色的线
    // color = (1.-line)*color; // 把线的地方扣成黑色，方便后续加色
    // 分割出线，加上背景
    color = (1.-line)*color + line*vec3(.0,1.,.0);
 gl_FragColor = vec4(color,1.0);

}
```

## 变种

st: 基于屏幕的纹理坐标

y: 和st有关系的函数

color: 展示y的灰度颜色

line: 通过st和y绘制的样条函数。。

color: 给line函数进行叠加显示

![image.png](https://s2.loli.net/2024/08/16/Qoudz1HApWlqJUK.png)

```glsl
#ifdef GL_ES
precision mediump float;
#endif

uniform vec2 u_resolution;
uniform vec2 u_mouse;
uniform float u_time;

// 比较st的x^5, 0, y的关系
// y<x^5的，都0
// y在0~x^5，都原值
// y=0的，都1
float plot(vec2 st,float y) {
    return smoothstep(y,0.0,st.y);
}
void main() {
	vec2 st = gl_FragCoord.xy/u_resolution;
	float y = pow(st.x,5.);
	vec3 color = vec3(y);
	float line = plot(st,y);
	color = (1.-line)*color + line*vec3(.0,1.,.0);
	gl_FragColor = vec4(color,1.0);
}
```

取个反再锐化下

```glsl
float plot(vec2 st,float y) {
    return smoothstep(y-0.02,y,st.y);
}
```

![image.png](https://s2.loli.net/2024/08/16/z6xAHaYDJFeQ7ct.png)



求个交集

```glsl
float plot(vec2 st,float y) {
    return smoothstep(y-0.03,y,st.y)-smoothstep(y-0.01,y,st.y);
}
```

![image.png](https://s2.loli.net/2024/08/16/O4LcxMDmrB2toTP.png)

更自然些的交集

```
float plot(vec2 st,float y) {
    return smoothstep(y-0.03,y,st.y)-smoothstep(y,y+0.03,st.y);
}
```

![image.png](https://s2.loli.net/2024/08/16/Ze68o1mKhicjrbW.png)

step需要分片来看：

* step(): 左片(<0.5), 右片(>0.5) ->y,也就是传进polt的pct
* smoothstep: 左片，所有纹理点都是1
* smoothstep: 右片，纹理点的分割按照(1-0.02, 1, st.y)进行，下0，中原值，上1

![image.png](https://s2.loli.net/2024/08/17/sel5gGTcNRP2iJq.png)


# 可能的bug

**没定义精度** `No precision specified for (float)`

> presion mediump floa**t;**

**fragment shader做完了不显示颜色：**

> 没给gl_FragColor赋值

**没找到函数** `'setColor' : no matching overloaded function found`

> C里面把函数写在void main()前面，或者需提前声明




# 推荐待更新

[Flong.com • Golan Levin &amp; Collaborators 的作品](https://www.flong.com/)

[Polynomial Shaping Functions - Golan Levin and Collaborators (flong.com)](https://www.flong.com/archive/texts/code/shapers_poly/)

[Exponential Shaping Functions - Golan Levin and Collaborators (flong.com)](https://www.flong.com/archive/texts/code/shapers_exp/)

[Circular &amp; Elliptical Shaping Functions - Golan Levin and Collaborators (flong.com)](https://www.flong.com/archive/texts/code/shapers_circ/index.html)

[Bezier and Other Parametric Shaping Functions - Golan Levin and Collaborators (flong.com)](https://www.flong.com/archive/texts/code/shapers_bez/)

---

[Inigo Quilez :: computer graphics, mathematics, shaders, fractals, demoscene and more (iquilezles.org)](https://iquilezles.org/)

[Inigo Quilez :: computer graphics, mathematics, shaders, fractals, demoscene and more (iquilezles.org)](https://iquilezles.org/articles/functions/)

[Inigo Quilez :: computer graphics, mathematics, shaders, fractals, demoscene and more (iquilezles.org)](https://iquilezles.org/articles/functions/)

---

![img](https://thebookofshaders.com/05/kynd.png)

---

[Creation by Silexars (shadertoy.com)](https://www.shadertoy.com/view/XsXXDn)

---

[LYGIA Shader Library](https://lygia.xyz/)

[Graphtoy](https://graphtoy.com/)
