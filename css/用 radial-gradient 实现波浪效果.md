### 先上效果图

![](https://camo.githubusercontent.com/3d24dd38ae496d7bc93952b1c663b546b8586f74/68747470733a2f2f692e6c6f6c692e6e65742f323031392f30352f32352f3563653932633135643231363434383330312e676966)

[codepen: https://codepen.io/linghucq1/pen/zQjqZv?editors=1100](https://codepen.io/linghucq1/pen/zQjqZv?editors=1100)

简单画一下原理

![](https://camo.githubusercontent.com/0fec79d3e87f0c1bd7728e0a74039aa1cc42093b/68747470733a2f2f692e6c6f6c692e6e65742f323031392f30352f32352f3563653933303064626130343435353533312e6a7067)

图中的波浪线 L 就是我们要的，它是沿着上下两排相切圆的切线画出来的，只要画出 A 段然后重复，就可以得到我们想要的波浪效果。

首先要解决怎么画圆的问题，这里就要用到 css3 radial-gradient 属性，不了解这个属性的可以看一下[mdn](https://developer.mozilla.org/zh-CN/docs/Web/CSS/radial-gradient)或者[10个demo示例学会CSS3 radial-gradient径向渐变](https://www.zhangxinxu.com/wordpress/2017/11/css3-radial-gradient-syntax-example/)。我们这里要做的，就是画出两种颜色的圆并让它们相切。

先搞一个装波浪的框
``` html
<div class="wave"></div>
```
``` less
.wave {
  height: 100px;
  background-color: blue;
  margin-top: 100px;
}
```
<img src="https://user-gold-cdn.xitu.io/2019/5/25/16aef3208cc5051e?w=947&h=153&f=png&s=1057">

得到一个蓝色的长方形，现在我们在它的上边缘画一个圆。

``` less
.wave {
  height: 100px;
  background-color: blue;
  margin-top: 100px;
  position: relative;
  
  &::before {
    content: '';
    position: absolute;
    height: 100px;
    width: 100%;
    top: -50px;
    background: radial-gradient(50px circle, blue 50px, transparent)
  }
}
```
![](https://user-gold-cdn.xitu.io/2019/5/25/16aef3208d1deb70?w=945&h=212&f=png&s=1651)

用 background-size 让它重复
``` less
&::before {
    content: '';
    position: absolute;
    height: 100px;
    width: 100%;
    top: -50px;
    background: radial-gradient(50px circle, blue 50px, transparent) repeat-x;
    background-size: 100px;
  }
```
![](https://user-gold-cdn.xitu.io/2019/5/25/16aef32093d916c4?w=943&h=215&f=png&s=2476)

这里已经可以看到雏形了，如果我们把间隔拉大然后在中间加入白色圆
``` less
  &::before {
    content: '';
    position: absolute;
    height: 100px;
    width: 100%;
    top: -50px;
    background: radial-gradient(50px circle at 50px 50px, #fff 50px, transparent), radial-gradient(50px circle at 150px 50px, blue 50px, transparent);
    background-size: 200px 100px;
  }
```
![](https://user-gold-cdn.xitu.io/2019/5/25/16aef3209441ae3f?w=936&h=246&f=png&s=3033)

一个圆圆的波浪就完成了，现在我们要做的，就是调整圆的位置。根据开始的手绘图，我们可以计算出上下圆圆心的水平和垂直距离。其中水平距离为圆的半径 R，垂直距离为 √3R 也就是 1.732R。而且我们只要画出一个周期的波段（A）然后重复就行了。

``` less
  &::before {
    @height: 50px * 1.732;
    content: '';
    position: absolute;
    height: 100px;
    width: 100%;
    top: -@height/2;
    background: radial-gradient(50px circle at 0 0, #f0f 50px, transparent), radial-gradient(50px circle at 50px @height, blue 50px, transparent), radial-gradient(50px circle at 100px 0, #f0f 50px, transparent);
    background-size: 100px 100px;
    background-repeat: no-repeat;
  }
```
我们先把背景重复关掉来看单独的一段长什么样，
![](https://user-gold-cdn.xitu.io/2019/5/25/16aef320d83ec695?w=944&h=224&f=png&s=1801)

这里特意用了一个颜色作为区分，需要注意的是 top 的值为上下圆心垂直距离的一半。

把上面颜色改成白色就是一个波浪效果了。
![](https://camo.githubusercontent.com/2276680179de76ed7400b8cca4344cc46335a591/68747470733a2f2f692e6c6f6c692e6e65742f323031392f30352f32352f3563653933656638386463333435313834322e706e67)

现在再加一个平移动画和另一层波浪和半透明，就可以实现最开始的效果。

但是，如果你去尝试把 codepen 代码里面的 wave2 往上移，就会发现这个方案并不完美，目前还没有想到解决方案，不知道各位有什么好点子？

这里顺便安利一个 chrome 插件:  [web maker](https://chrome.google.com/webstore/detail/web-maker/lkfkkhfhhdkiemehlpkgjeojomhpccnh?utm_source=chrome-ntp-icon)，可以实时预览 html css js 编辑效果，还可以在线保存之前的代码，没事的时候就鼓捣鼓捣，很好用。