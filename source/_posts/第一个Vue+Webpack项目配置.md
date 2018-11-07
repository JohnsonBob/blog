---
title: 第一个Vue+Webpack项目配置
date: 2018年11月7日11:33:06
categories:
- 前端
- Vue
tags:
- Webpack
- Vue
---


##### 1、项目初始化

```
npm inti
npm i webpack vue vue-loader css-loader vue-template-compiler
```

##### 2、创建src目录并在src目录下创建app.vue文件，在app.vue文件中写入：

```
<template>
    <die id = "test">{{text}}</die>
</template>

<script>
    export default{
        data() {
            return {
                text: 'abc'
            }
        }
    }
</script>

<style>

#test{
    color: red;
}

</style>
```

##### 3、在src下创建入口文件index.js，并写入：

```
import Vue from 'vue'
import App from './app.vue'


const root = document.createElement('div')
document.body.appendChild(root)

new Vue({
    render: (h) => h(App)
}).$mount(root)
```

##### 4、在根目录创建webpack配置文件webpack.config.js，并写入：
```
vue-loader@15.*之后除了必须带有VueLoaderPlugin 之外，还需另外单独配置css-loader。
```

```
const path = require('path')
const VueLoaderPlugin = require('vue-loader/lib/plugin');

module.exports = {
    entry: path.join(__dirname, 'src/index.js'), //入口文件
    output: {
        filename: "bundle.js",  //输出文件名
        path: path.join(__dirname, 'dist')   //输出文件路径
    },
    module: {
        rules: [
            {
                test: /.vue$/,
                loader: "vue-loader"
            },
            {
                test: /\.css$/,
                use: ['style-loader','css-loader']
            }
        ]
    },
    plugins: [
        // 添加VueLoaderPlugin，mode-loader
        new VueLoaderPlugin()
    ],
}

```

##### 5、在package.json文件scripts节点下添加：
```
"build": "webpack --config webpack.config.js"
```

##### 6、执行打包命令
```
npm run build
```



