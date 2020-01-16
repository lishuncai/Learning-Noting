### vue项目请求预取文件问题（Prefetch）

vue文档相关连接 https://cli.vuejs.org/zh/guide/html-and-static-assets.html#%E6%8F%92%E5%80%BC

#### 现象

当vue项目配置路由懒加载后，加载首页时会发现多出很多js、css文件（看起来像是其他页面的模块，但是无法预览文件内容），
这时会疑惑我们使用懒加载，没有打开其他页面的情况下，为什么还会加载页面其他模块呢？

#### 真相

这是一种叫做链接预取的浏览器机制，详情参考[MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Link_prefetching_FAQ)。

不过它是消耗带宽的

#### 在vue.config.js关掉此功能

```

module.exports = {
  chainWebpack: config => {
    // 详情见https://cli.vuejs.org/zh/guide/html-and-static-assets.html#prefetch
    // config.plugins.delete('preload') // TODO: need test
    config.plugins.delete('prefetch') // TODO: need test
  }
}

```
