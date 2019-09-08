# 前端将 px 转换成 vw 的移动端适配方案 #


####  关键： postcss-px-to-viewport 可以在编译时将样式文件中的 px 单位转换为 vw 单位 ####
 
使用 postcss-px-to-viewport 插件
    
    npm install -D postcss-loader postcss-px-to-viewport
    
配置 postcss.config.js

    module.exports = {
      plugins: {
        'postcss-px-to-viewport': {
          unitToConvert: 'px',
          viewportWidth: 750,
          unitPrecision: 5, // 指定`px`转换为视窗单位值的小数位数（很多时候无法整除）
          propList: ['*'],
          viewportUnit: 'vw',
          fontViewportUnit: 'vw',
          selectorBlackList: ['weui-', '.ignore'], // 指定不转换为视窗单位的类
          minPixelValue: 1,
          mediaQuery: false,
          replace: true,
          exclude: /(\/|\\)(node_modules)(\/|\\)/,
          landscape: false,
          landscapeUnit: 'vw',
          landscapeWidth: 568
        }
      }
    }

webpack配置

    {
      test: /\.(scss|css)$/,
      use: [
        'css-loader',
        'sass-loader',
        'postcss-loader' /* 关键 */
      ]
    }
    
 
<h4>以上配置，将px单位转换为vw</h4>

<h4>问题： </h4>
  
  1. 第三方组件库如何不受影响；

  2. 自己写的样式我不需要全部转换为vw，部分样式属性还得用保留px单位怎么办？
  
#### 解决方法： ####
 
 问题1： 配置exclude属性：
 
     module.exports = {
      plugins: {
        'postcss-px-to-viewport': {
          exclude: /(\/|\\)(node_modules)(\/|\\)/,
        }
      }
    }
  
  将modules排除掉，以免影响到第三方库
  
  问题2：
  
   解决1: 配置 selectorBlackList 属性；
  
    selectorBlackList: ['weui-', '.ignore'], // 指定不转换为视窗单位的类
    
   * 此方法，网上都是这么说， 但本人在自搭建vue环境的组件里亲测无效，不知道哪里出了问题
  
  解决2： 利用postcss-px-to-viewport 不会转换@import引入的文件， 也不会转换scss变量, 函数等方式书写属性值的 px 单位；
  在全局样式文件中使用函数：
  
  > global.scss
  
      @function px($px) {
        @return $px + px;
      }
      
  > component.scss
  
    import global.scss;
    el {
      width: px(5); /* 保留px单位： 5px */
      height: 5px; /* 会被转换： *vw */
    }
      
  
 另外： 使用全局scss变量可以节省每次每个文件都要使用@import的麻烦 [#参考](./%E5%85%A8%E5%B1%80%E5%BC%95%E5%85%A5scss%E7%9A%84%E5%8F%98%E9%87%8F%E5%A3%B0%E6%98%8E.md)
 
