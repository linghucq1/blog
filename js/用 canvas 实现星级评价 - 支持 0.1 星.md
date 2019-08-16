# 用 canvas 实现一个星级评价，并且支持 0.1 星。

大致思路：
1. 怎么画星星？
2. 怎么画有不同填充比例的星星？

只要能解决这两个问题，基本就成了。

### 1. 如何用 canvas 画一个星星?

没有思路。

当然，画星星这种事情肯定早就有人做过啦! [https://blog.csdn.net/qq_20417227/article/details/52034476](https://blog.csdn.net/qq_20417227/article/details/52034476)
![20160726114812333 _1_.jpg](https://i.loli.net/2019/08/16/F8oaRKxThyuP6Id.png)

如上图所示，利用大小两个圆，每旋转 72 度取一个点，用 lineTo 连接最后闭合，就是一个五角星了。

### 2. 如何给星星填充任意比例的颜色?

emmm，还是没思路。

看看别人怎么做的。
[https://github.com/craigh411/vue-star-rating](https://github.com/craigh411/vue-star-rating)
这是用 svg 实现的，看了下源码，是用渐变实现的填充比例。

原来是渐变啊，对啊，不是渐变还能是什么呢？我怎么就没想到。。。

``` javascript
function drawStar({x, y, r, R, rot, index}){
  const ctx = context;
  const gradient = ctx.createLinearGradient(x - R, 0 , x + R, 0)
  const stop = Math.min(Math.max((index + 1) * oneStarPercent - percent, 0), oneStarPercent)
  const rate = (oneStarPercent - stop) / oneStarPercent
  
  gradient.addColorStop(rate, ratingColor)
  gradient.addColorStop(Math.min(1, rate + 0.01), starColor)
  // ....
}
```
这里的重点就在 `addColorStop`，通过调整渐变的分界线，控制星星填充的比例。

搞定基础的，后面就水到渠成了。
[https://codepen.io/linghucq1/pen/pozyrpN](https://codepen.io/linghucq1/pen/pozyrpN)

step 评分间隔，0.5 半星，0.1 就是 0.1 星。

starNum 星星数量 10 星
![Snipaste_2019-08-16_09-56-55.png](https://i.loli.net/2019/08/16/kCbJIBplMDcgN6W.png)

除了五角星之外，还可以改变一下循环次数，变成：
手里剑
![Snipaste_2019-08-16_09-24-28.png](https://i.loli.net/2019/08/16/KhqL9XgR4fS8jdm.png)

锯齿星
![Snipaste_2019-08-16_09-25-24.png](https://i.loli.net/2019/08/16/rv4t7A56Y9ELnBu.png)

螺旋星
![Snipaste_2019-08-16_09-25-41.png](https://i.loli.net/2019/08/16/WCnvqiKaAk6GQ9L.png)

改变一下内外圆的比例还可以变成一个

丰满的五角星
![Snipaste_2019-08-16_09-26-11.png](https://i.loli.net/2019/08/16/OIl6EyoKcqRVPjQ.png)

当然，用 canvas 虽然能实现评分功能，但要想做类似烟花效果，跳动效果等还是有点复杂。。。这点就比不上 svg 了，canvas 的优势就在于，它是 js 写的啊。[狗头]