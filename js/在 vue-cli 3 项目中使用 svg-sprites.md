关于 svg sprites 是什么以及为什么要用它，可以参考：[未来必热：SVG Sprite技术介绍](https://www.zhangxinxu.com/wordpress/2014/07/introduce-svg-sprite-technology)。

下面要说的是如何在 vue-cli 3 项目中使用 svg sprite。

1、要使用 svg，首先你得有 svg :dog:，现在很多图标网站都提供 svg 格式下载，比如阿里巴巴 iconfont：

<img src='https://i.loli.net/2019/05/17/5cde26a76da0e78765.png'>

如果你的团队使用了比如蓝湖之类的工具，也可以直接下载 svg 格式的图标。

2、将下载的 svg 图标放在一个文件夹内（当然也可以分类别放在多个文件夹），比如这样：

<img src='https://i.loli.net/2019/05/17/5cde276b73b9e67169.png'>

你可能注意到了 icons 下的 index.js，它长这样：

``` javascript
// 全局注册 svg-icon 组件
import Vue from 'vue'
import SvgIcon from 'src/components/svg-icon'
Vue.component('svg-icon', SvgIcon)

/**
 * 下面这段代码的作用类似于:
 require('./svg/clock.svg')
 require('./svg/menu-today-member.svg')
 require('./svg/menu-member.svg')
 .
 .
 .
 */
const req = require.context('./svg', false, /\.svg$/)
const requireAll = requireContext => requireContext.keys().map(requireContext)
requireAll(req)
```

其中的 SvgIcon 长下面这样，传入 name 和 size 控制图标，保持用法和其他 UI 框架差不多：

``` javascript
<template>
  <svg class="svg-icon" :style="{ width: iconSize, height: iconSize }" aria-hidden="true">
    <use :xlink:href="iconName"/>
  </svg>
</template>

<script>
  export default {
    name: 'SvgIcon',
    props: {
      name: {
        type: String,
        required: true
      },
      size: {
        type: [Number, String],
        default: '1em'
      }
    },
    computed: {
      iconSize() {
        return typeof this.size === 'number' ? `${this.size}px` : this.size
      },
      iconName() {
        return `#icon-${this.name}`
      }
    }
  }
</script>

<style scoped>
  .svg-icon {
    margin-right: .2em;
    fill: currentColor;
    overflow: hidden;
  }
</style>
```

3、svg 也引入了，组件也写好了，接下来就是怎么处理成 svg sprites 的问题，首先添加 svg-sprite-loader：

``` bash
yarn add svg-sprite-loader -D
```

然后在 vue.config.js 中添加：

``` javascript
const path = require('path')

const resolve = dir => {
  return path.join(__dirname, dir)
}

module.exports = {
  .
  .
  .
  chainWebpack: config => {
    .
    .
    .
    // 将 icons 目录排除在 svg 默认规则之外
    config.module.rule('svg').exclude.add(resolve('src/assets/icons')).end();
    // 用 svg-sprite-loader 处理 icons 下的 svg
    config.module
      .rule('icons')
      .test(/\.svg$/)
      .include.add(resolve('src/assets/icons'))
      .end()
      .use('svg-sprite-loader')
      .loader('svg-sprite-loader')
      .options({
        symbolId: 'icon-[name]'
      })
      .end()
  },
  .
  .
  .
}
```
这一步处理完，打开浏览器调试工具应该就可以看到如下结构了：

<img src='https://i.loli.net/2019/05/17/5cde2ce9b64f421815.png'>

现在在需要用图标的地方：
``` javascript
.
.
<Button>
    <span style="display: flex; align-items: center">
        <svg-icon name="edit" size="1.1em"></svg-icon>
        <span>编辑</span>
    </span>
</Button>
.
.
```

然后我们就可以看到 

<img src="https://i.loli.net/2019/05/17/5cde2e070e22413393.png">

4、实际使用会发现有几个问题：
- 首先是在 svg 中可能会有一些奇怪的东西存在，比如 sketch 留下的注释，当然是要压缩掉啦。
- 另外改变某些 svg 的 fill 属性无效？？？仔细一看发现不能改变 fill 的 svg 文件里都已经有了 fill 这个属性，那有没有办法把它干掉呢？当然可以，手动删掉就行，emm...... 然后又顺藤摸瓜找到了 svgo，具体用法：[github svgo](https://github.com/svg/svgo)、[SVG精简压缩工具svgo简介和初体验](https://www.zhangxinxu.com/wordpress/2016/02/svg-compress-tool-svgo-experience/)。

添加 svgo-loader：

``` bash
yarn add svgo svgo-loader -D
```

然后修改 vue.config.js

``` javascript
const path = require('path')

const resolve = dir => {
  return path.join(__dirname, dir)
}

module.exports = {
  .
  .
  .
  chainWebpack: config => {
    .
    .
    .
    // 将 icons 目录排除在 svg 默认规则之外
    config.module.rule('svg').exclude.add(resolve('src/assets/icons')).end();
    // 用 svg-sprite-loader 处理 icons 下的 svg
    config.module
      .rule('icons')
      .test(/\.svg$/)
      .include.add(resolve('src/assets/icons'))
      .end()
      .use('svg-sprite-loader')
      .loader('svg-sprite-loader')
      .options({
        symbolId: 'icon-[name]'
      })
      .end()
      .use('svgo-loader') // 添加 svgo-loader
      .loader('svgo-loader')
      .options({
        plugins: [
          { removeAttrs: { attrs: 'fill' } } // 添加插件移除 svg 中的 fill 属性
        ]
      })
      .end()
  },
  .
  .
  .
}
```

然后我们就看到所有的注释和 fill 属性都没有啦！

<img src="https://i.loli.net/2019/05/17/5cde3251b0ef821098.png">

到此，vue-cli 3 使用 svg sprites 成功