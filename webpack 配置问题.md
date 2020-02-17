# webpack 配置问题记录

### 1. 打包图片、其他资源，html文件中路径的问题

  如果不想用import在js中一个个引入，可以使用html-loader
  
  配置如下：
  
    ```
     {
        test: /\.html$/,
        use: {
          loader: 'html-loader',
          options: {
            minimize: true,
            attrs: ['img:src', 'img:data-src', 'audio:src']
          }
        }
      },
    ```
    
  注意: html-loader 一定要在file-loader之前
  
  打包后index.html文件可能回出现\<img src="[Object module]" />的问题， 解决如下：
  
    在file-loader中设置options的esModules属性为false
  
  ```
   use: [
      {
        loader: 'file-loader',
        options: {
          name: '[name].[hash:6].[ext]',
          esModule: false,
        }
      }
    ]
  ```
  
  ------------
  
  ### 
