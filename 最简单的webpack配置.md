# 简单的webpack配置

#### webpack.config.js

```
  const path = require('path');
  const HtmlWebpackPlugin = require('html-webpack-plugin');
  const {CleanWebpackPlugin} = require('clean-webpack-plugin');

  const MiniCssExtractPlugin = require("mini-css-extract-plugin");//提取css到单独文件的插件
  const OptimizeCssAssetsPlugin = require('optimize-css-assets-webpack-plugin');//压缩css插件

  module.exports = {
    entry: './assets/main.js',
    mode: 'production',
    output: {
      filename: 'bundle.[hash:6].js',
      path: path.resolve(__dirname, 'dist'),
      publicPath: './'
    },
    module: {
      rules: [
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
        {
          test: /\.css$/,
          use: [
            {
              loader: MiniCssExtractPlugin.loader,
              options: {
                esModule: true
              }
            },
            {
              loader: 'css-loader'
            }
          ]
        },
        {
          test: /\.(png|svg|jpg|gif)$/,
          use: [
            {
              loader: 'file-loader',
              options: {
                limit: '8000',
                name: '[name].[hash:6].[ext]',
                esModule: false,
                outputPath: 'img'
              }
            }
          ]
        },
        {
          test: /\.(woff|woff2|eot|ttf|otf)$/,
          use: [
            {
              loader: 'file-loader',
              options: {
                name: '[name].[hash:6].[ext]',
                esModule: false,
                outputPath: 'font'
              }
            }
          ]
        },
        {
          test: /\.mp3$/,
          use: [
            {
              loader: 'file-loader',
              options: {
                name: '[name].[ext]',
                esModule: false,
                outputPath: 'audio'
              }
            }
          ]
        },
      ]
    },
    plugins: [
      new CleanWebpackPlugin(),
      new HtmlWebpackPlugin({
        template: "./index.html",
        title: ''
      }),
      new MiniCssExtractPlugin({
        filename: "[name].[hash:6].css"
      }),
      new OptimizeCssAssetsPlugin()
    ],
  };
```


#### package.json文件

```
{
  "name": "h5_active",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "dependencies": {
    "webpack-cli": "^3.3.11",
    "webpack": "^4.41.6"
  },
  "devDependencies": {
    "clean-webpack-plugin": "^3.0.0",
    "css-loader": "^3.4.2",
    "file-loader": "^5.0.2",
    "html-loader": "^0.5.5",
    "html-webpack-plugin": "^3.2.0",
    "mini-css-extract-plugin": "^0.9.0",
    "optimize-css-assets-webpack-plugin": "^5.0.3",
    "style-loader": "^1.1.3"
  },
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack --config webpack.config.js"
  },
  "author": "lishuncai",
  "license": "ISC"
}

```
