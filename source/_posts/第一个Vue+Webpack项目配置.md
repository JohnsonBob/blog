---
title: 第一个Vue+Webpack项目配置
date: 2018年11月7日11:33:06
categories:
- 前端
- Vue
tags:
- Webpack
- Vue
- Stylus
- postcss
- babel
---


##### 1、项目初始化

```
npm inti
npm i webpack vue vue-loader css-loader vue-template-compiler style-loader
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
                test: /\.vue$/,
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

##### 7、图片处理 url-loader

###### 7.1、安装环境 url-loader file-loader
```
npm i url-loader file-loader
```

###### 7.2、webpack.config.js配置如下
```
module.exports = {
	module: {
		rules: [
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
	}
}
```

##### 8、使用CSS预处理框架Stylus

###### 8.1、安装环境 stylus-loader stylus
```
npm i stylus-loader stylus
```

###### 8.2、webpack.config.js配置如下
```
module.exports = {
	module: {
		rules: [
			{
                test: /\.styl$/,
                use: [
                    'style-loader',
                    'css-loader',
                    'stylus-loader'  //处理.styl文件为css文件后返回给上级处理
                ]
            }
		]
	}
}
```

##### 9、使用postcss和babel

###### 9.1、安装依赖
```
npm i postcss-loader autoprefixer babel-loader babel-core
npm install babel-preset-env babel-plugin-transform-vue-jsx
```

###### 9.2、根目录创建babel配置文件.babelrc
```
//babel配置文件
{
  "presets": [
    "env"
  ],
  "plugins": [
    "transform-vue-jsx"
  ]
}
```

###### 9.3、根目录创建postcss配置文件postcss.config.js
```
//postcss配置文件  后处理css
const autoprefixer = require('autoprefixer');

module.exports= {
    plugins:[
        autoprefixer(),
    ]
};
```

###### 9.4、webpack.config.js文件修改
```
const config = {
    module: {
        rules: [
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
        ]
    }
}
```