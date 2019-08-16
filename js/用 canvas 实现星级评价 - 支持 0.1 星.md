# 用 canvas 实现一个星级评价，并且支持 0.1 星。

大致思路：
1. 怎么画星星？
2. 怎么画有不同填充比例的星星？

只要能解决这两个问题，基本就成了。

### 1. 如何用 canvas 画一个星星?

没有思路。

当然，画星星这种事情肯定早就有人做过啦! [https://blog.csdn.net/qq_20417227/article/details/52034476](https://blog.csdn.net/qq_20417227/article/details/52034476)

### 2. 如何给星星填充任意比例的颜色?

emmm，还是没思路。

看看别人怎么做的。
[https://github.com/craigh411/vue-star-rating](https://github.com/craigh411/vue-star-rating)

原来是渐变啊，对啊，不是渐变还能是什么呢？我怎么就没想到。。。

搞定基础的，后面就水到渠成了。
[https://codepen.io/linghucq1/pen/pozyrpN](https://codepen.io/linghucq1/pen/pozyrpN)

step 评分间隔，0.5 半星，0.1 就是 0.1 星。

starNum 星星数量:

10 星
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