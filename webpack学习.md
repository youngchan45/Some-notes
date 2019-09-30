### webpack部分-- 手动设置+引入vue

npm install cnpm -g --registry=https://registry.npm.taobao.org

1. 初始化

   ```
   npm init -y
   ```

2. 本地安装webpack和webpack-cli

   ```javascript
   npm i webpack webpack-cli -D
   ```

3. 创建项目目录

```
webpack-demo
|- /public
    |- index.html
|- /src
    |- main.js
|- package-lock.json
|- package.json
|- webpack.config.js
```

4. 配置webpack.config.js

   ```javascript
   const path = require('path');
   
   module.exports = {
       entry: './src/main.js',
       output: {
           filename:'bundle.js',
     		path:path.resolve(__dirname,'dist')
       },
       mode:'development'
   };
   ```

   此时可在终端输入

   ```
   npx webpack
   ```

   则会自动生成一个bundle.js文件

5. webpack-dev-server //提供一个简单的web服务器，并能够实时重新加载等等

   ```
   npm i webpack-dev-server -D
   ```

   在package.json中配置 //Json使用双引号

   ```json
    {
       "name": "development",
       "version": "1.0.0",
       "description": "",
       "main": "webpack.config.js",
       "scripts": {
         "test": "echo \"Error: no test specified\" && exit 1",
   +     "dev": "webpack-dev-server --hot --port 3000 --inline --open --contentBase public",
       },
       "keywords": [],
       "author": "",
       "license": "ISC",
       "devDependencies": {
         "html-webpack-plugin": "^2.29.0",
         "webpack": "^3.0.0",
         "webpack-cli": "^3.3.7",
         "clean-webpack-plugin": "^3.0.0",
       }
     }
   ```

   安装后在终端输入

   ```
   npm run dev
   ```

    然后在index.html中引入<script src='/bundle.js'>即可进行自动加载

6. 安装html-webpack-plugin和clean-webpack-plugin插件

   ```
   npm i html-webpack-plugin -D
   npm i clean-webpack-plugin -D
   ```

   在webpack.config.js中配置

   ```javascript
   const path = require('path');
   + const HtmlWebpackPlugin = require('html-webpack-plugin');
   + const { CleanWebpackPlugin } = require('clean-webpack-plugin');
   
     module.exports = {
       entry: {
         app: './src/main.js'
       },
   +   plugins: [
   +     new HtmlWebpackPlugin({
   +       template:'./public/index.html',
   +     }),
   +     new CleanWebpackPlugin(),
   +   ],
   +   devServer: {
   +     contentBase: './dist',
   +     port: 8000
   +   },
       output: {
         filename: 'bundle.js',
         path: path.resolve(__dirname, 'dist')
       }
     };
   ```

   此时可以把index.html中的<script src='/bundle.js'>注释掉，因为html-webpack-plugin会在内存中根据指定的模板页面，生成一份内存中的首页，同时自动把打包好的bundle注入到页面底部

7. 创建.css文件，在main.js中配置

   ```
   import './css/index.css'
   ```

   安装style-loader和css-loader

   ```javascript
   npm i style-loader css-loader //从右到左安装
   ```

   在webpack.config.js中配置

   ```javascript
   const path = require('path');
   const HtmlWebpackPlugin = require('html-webpack-plugin');
   const { CleanWebpackPlugin } = require('clean-webpack-plugin');
   
   module.exports = {
       entry: './src/main.js',
       output: {
           filename: 'bundle.js',
           path: path.resolve(__dirname, 'dist'),
       },
       plugins: [
           new HtmlWebpackPlugin({
               template:'./public/index.html',
           }),
           new CleanWebpackPlugin(),
       ],
       devServer: {
           contentBase: './dist',
           port: 8000
       },
   +   module: {
   +       rules: [
   +           {
   +               test: /\.css$/,
   +               use: [
   +                   'style-loader',
   +                   'css-loader'
   +               ]
   +           }       
   +       ]
   +   }
   }
   ```


8. 安装处理url的loader

   ```javascript
   npm i url-loader file-loader -D
   ```

   在webpack.config.js配置

   ```javascript
   const path = require('path');
   const HtmlWebpackPlugin = require('html-webpack-plugin');
   const { CleanWebpackPlugin } = require('clean-webpack-plugin');
   
   module.exports = {
       entry: './src/main.js',
       output: {
           filename: 'bundle.js',
           path: path.resolve(__dirname, 'dist'),
       },
       plugins: [
           new HtmlWebpackPlugin({
               template:'./public/index.html',
           }),
           new CleanWebpackPlugin(),
       ],
       devServer: {
           contentBase: './dist',
           port: 8000
       },
      module: {
          rules: [
              {
                  test: /\.css$/,
                  use: [
                      'style-loader',
                      'css-loader'
                  ]
              }，
   +  {
   +    test: /\.(png|jpg|gif)$/,
   +    loader: 'url-loader'
   +  }
          ]
      }
   }
   ```


9. 安装处理字体的loader（存疑）

10. 安装babel

    ```javascript
    npm install -D babel-loader @babel/core @babel/preset-env webpack
    ```

    配置

    ```
    module: {
      rules: [
        {
          test: /\.m?js$/,
          exclude: /(node_modules|bower_components)/,
          use: {
            loader: 'babel-loader',
            options: {
              presets: ['@babel/preset-env']
            }
          }
        }
      ]
    }
    ```
    
    创建名为 .babelrc的新文件来配置 Babel
    
    ```
    { 
      "presets": ["@babel/preset-env"] 
    }
    ```

### vue部分

1. 安装vue

   ```
   npm i vue -S
   ```

2. 安装vue-loader vue-template-compiler

   ```javascript
   npm i vue-loader vue-template-compiler -D
   ```

3. 新增规则

   ```javascript
   {
   test:/\.vue$/,
   loader:['vue-loader']
   }
   ```

4. 添加插件（经常忘记）

   ```javascript
   // webpack.config.js
   const VueLoaderPlugin = require('vue-loader/lib/plugin')
   
   module.exports = {
     module: {
       rules: [
         // ... 其它规则
         {
           test: /\.vue$/,
           loader: 'vue-loader'
         }
       ]
     },
     plugins: [
       // 请确保引入这个插件！
       new VueLoaderPlugin()
     ]
   }
   ```

5. 在main.js中导入vue模块

   ```javascript
   import Vue from 'vue'
   ```

6. 在src下建立具体组件xx.vue，需包含<template></template>、<script></script>、<style></style>三部分

7. 在main.js中导入.vue组件

   ```javascript
   import login from './login.vue'
   ```

8. 在main.js中创建vm实例，添加组件，使用render方法

   ```javascript
   var vm =new Vue({
       el:'#app',
       //以前注册组件的方法
       components:{
   			login
   				}
   //使用render方法(与data、el同级)
   render:function(creatElements){
   	return creatElements(login)
   }
   })
   ```

9. 页面中创建id为app的div元素作为vm实例要控制的区域