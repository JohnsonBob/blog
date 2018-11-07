---
title: Webpack-dev-server项目配置
date: 2018年11月7日16:11:26
categories:
- 前端
- Vue
tags:
- Webpack
- Vue
- Webpack-dev-server
---


##### 1、安装依赖

```
npm i webpack-dev-server
npm i cross-env //配置环境变量依赖
npm i html-webpack-plugin   //webpack插件 生成入口html自动包含打包后js
```

##### 2、修改package.json文件如下：

```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "cross-env NODE_ENV=production webpack --config webpack.config.js",
    "dev": "cross-env NODE_ENV=development webpack-dev-server --config webpack.config.js"
},
```

##### 3、修改webpack配置文件webpack.config.js增加：
```
const config = {
    target: "web",
}
```

```
const config = {
    plugins: [
            new webpack.DefinePlugin({
                'process.env':{
                    NODE_ENV : isDev ? '"development"' : '"production"',
                }
            }),
            new HTMLPlugin(),
        ],
}
```

```
/**
 * 针对开发环境做以下配置
 */
if(isDev){
    config.devtool= '#cheap-module-eval-source-map';    //在浏览器中调试查看es6代码
    config.devServer = {
        port: 8000,
        host: '0.0.0.0',
        overlay: {
            error: true,    //在网页上显示错误信息
        },
        open: false,     //启动时自动打开浏览器
        historyApiFallback: {},     //webpack未识别的路由重定向
        //启动热加载
        hot: true,
    };

    //热加载
    config.plugins.push(
        new webpack.HotModuleReplacementPlugin(),
        new webpack.NoEmitOnErrorsPlugin(),
    );

}
```

```
module.exports = config;
```

##### 4、最终webpack.config.js文件为：
```
const path = require('path');
const VueLoaderPlugin = require('vue-loader/lib/plugin');
const isDev = process.env.NODE_ENV === 'development';  //判断是否是开发环境 cross-env中配置的变量都存在process.env中
const HTMLPlugin = require('html-webpack-plugin');
const webpack = require('webpack');

const config = {
    target: "web",
    entry: path.join(__dirname, 'src/index.js'), //入口文件
    output: {
        filename: "bundle.js",  //输出文件名
        path: path.join(__dirname, 'dist')   //输出文件路径
    },
    module: {
        rules: [
            {
                test: /\.vue$/,
                loader: "vue-loader"
            },
            {
                test: /\.css$/,
                use: ['style-loader','css-loader']
            },
            {
                test: /\.jsx$/,
                loader: "babel-loader"
            },
            {
                test: /\.styl$/,
                use: [
                    'style-loader',
                    'css-loader',
                    {
                        loader: "postcss-loader",
                        options: {
                            sourceMap:true,     //使用stylus-loader生成sourceMap
                        }
                    },
                    'stylus-loader'   //处理.styl文件为css文件后返回给上级处理
                ]
            },
            {
                test: /\.(gif|jpg|jpeg|png|svg)$/,
                use: [
                    {
                        loader: "url-loader",
                        options: {
                            limit: 1024,  //小于1024转换为base64直接放在js内容内
                            name:'[name].[ext]',  //名字使用原来的文件名 ext为扩展名
                        }
                    }
                ]
            }
        ]
    },
    plugins: [
        // 添加VueLoaderPlugin，mode-loader
        new VueLoaderPlugin(),
        new webpack.DefinePlugin({
            'process.env':{
                NODE_ENV : isDev ? '"development"' : '"production"',
            }
        }),
        new HTMLPlugin(),
    ],
};

/**
 * 针对开发环境做以下配置
 */
if(isDev){
    config.devtool= '#cheap-module-eval-source-map';    //在浏览器中调试查看es6代码
    config.devServer = {
        port: 8000,
        host: '0.0.0.0',
        overlay: {
            error: true,    //在网页上显示错误信息
        },
        open: false,     //启动时自动打开浏览器
        historyApiFallback: {},     //webpack未识别的路由重定向
        //启动热加载
        hot: true,
    };

    //热加载
    config.plugins.push(
        new webpack.HotModuleReplacementPlugin(),
        new webpack.NoEmitOnErrorsPlugin(),
    );

}

module.exports = config;


```

##### 5、启动调试项目
```
npm run dev
```

