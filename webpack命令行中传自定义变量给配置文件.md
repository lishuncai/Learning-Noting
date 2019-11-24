## webpack命令行中自定义变量给配置文件

#### 使用场景：

 很多时候我们的项目不是直接部署在域名根路径下的， 我需要在打包项目的时候可以指定项目部署的路径地址（或其他配置信息）,
 output.publicPath, 而无需在配置中写死而不方便更改地址。
 
 ---
 
#### 两个关键点：

 一、我们需要在配置文件中获取到package.json 文件scripts里的参数。
  1. 在命令行里使用给env对象增加属性，
  ``` webpack --env.NODE_ENV=local --env.production --progress ```
  当配置文件module.exports使用函数时，用参数env对象里就会有出现我们设置的参数
  ```
  module.exports = (env, argv) => {
  // Use env.<YOUR VARIABLE> here:
  console.log('NODE_ENV: ', env.NODE_ENV) // 'local'
  console.log('Production: ', env.production) // true

  return {
    entry: './src/index.js',
    output: {
      filename: 'bundle.js',
      path: path.resolve(__dirname, 'dist')
    }
  }
}
  ```
  参考https://www.webpackjs.com/guides/environment-variables/.
  不过要注意，process.env里是找不到的设置的参数的，在vue配置文件里使用函数输出时似乎没法引用到参数env，
  所以用函数应用的方法在vue就行不同了。 
  ###### vue和webpack的通用解决方法:使用 yargs，webpack4.x自带yargs，所以可直接引入：
  ```
  const argv = require('yargs').argv
  ```
 argv对象以键值形式保存参数，和env相同，不过我们无需通过env对象来传参数了。
 ```
 vue-cli-service build --publicPath=myPath
 ```
 此时，我们可以直接在argv对象里取值。
 
 ---
 
 二、 在命令行中传值给npm scripts
 
 在npm scripts 将变量值写死获取在.env文件里把值写死，和写在代码里没什么区别，如果我们项目的部署路径改了，难道还要修改代码再打包吗？  
 webpack-cli允许我们在命令行里直接传键值对或变量值给npm scripts, vue-cli也是一样的（毕竟基于webpack）。
 例如：
 ```
 // package.json: 
  "scripts": {
    "build": "vue-cli-service build --publicPath"
  }
 
  // 命令行：
  npm run build myPath
 ```
 或者：
  ```
 // package.json: 
  "scripts": {
    "build": "vue-cli-service build"
  }
 
  // 命令行：
  npm run build -- --publicPath=myPath --myVar=myVar
 ```
 
 
 
 这样，我们就可以在命令行里自定义变量给配置文件了。注意在vue的路由配置也要有更着变化哦
 ```
 base: process.env.BASE_URL
 ```
 ---
 
#### 扩展:
如果想在process.env里存值或修改里面的值，可以借助cross-env。
```
  "scripts": {
    "build": "cross-env NODE_ENV=production webpack --config build/webpack.config.js"
  }
```
或者增环境变量文件.env, 详情请了解vue-cli文档[环境变量与模式](https://cli.vuejs.org/zh/guide/mode-and-env.html)
 
 
 
 
 
 
