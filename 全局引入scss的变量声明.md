# 如何在webpack项目中全局引入scss的变量声明


因为在使用scss语法开发组件的时候，当需要使用公共样式文件（.scss）的变量时，需要使用 @import "" 引入; 

而在每个组件文件中用 @import "" 引入十分繁琐，此时就需要全局引入scss的变量声明。

#### 使用loader ####

    npm install sass-resources-loader -D

#### 在webpack.config.js中配置 ####

    {
      test: /\.(scss)$/,
      use: [
        {
          loader: 'sass-resources-loader',
          options: {
            resources: path.resolve(__dirname, './src/assets/styles/global.scss')
          }
        },
        other-loaders...
      ]
    }
    
#### 这样，在其他.scss文件中就可以引入global.scss文件中的变量， 而无需使用@import。 ####

[#参考文章](https://segmentfault.com/a/1190000010324128)
