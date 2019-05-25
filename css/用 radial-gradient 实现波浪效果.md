### 先上效果图

<!-- <img src="https://i.loli.net/2019/05/25/5ce92c15d216448301.gif"> -->

[codepen: https://codepen.io/linghucq1/pen/zQjqZv?editors=1100](https://codepen.io/linghucq1/pen/zQjqZv?editors=1100)

简单画一下原理

<!-- <img src="https://i.loli.net/2019/05/25/5ce9300dba04455531.jpg"> -->

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
<!-- <img src="https://i.loli.net/2019/05/25/5ce9355d98df344170.png"> -->

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
![](https://i.loli.net/2019/05/25/5ce9363069d3715421.png)

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
![](https://i.loli.net/2019/05/25/5ce9375b8b73d22391.png)

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
![](https://i.loli.net/2019/05/25/5ce93957c479285165.png)

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
![](https://i.loli.net/2019/05/25/5ce93e77a9a2330601.png)

这里特意用了一个颜色作为区分，需要注意的是 top 的值为上下圆心垂直距离的一半。

把上面颜色改成白色就是一个波浪效果了。
![](https://i.loli.net/2019/05/25/5ce93ef88dc3451842.png)

现在再加一个平移动画和另一层波浪和半透明，就可以实现最开始的效果了。

但是，如果你去尝试把 codepen 代码里面的 wave2 往上移，就会发现这个方案并不完美，目前还没有想到解决方案，不知道各位有什么好点子？