---
title: node.js+ionic+cordova移动混合开发环境部署
date: 2018-06-21 10:24:33
categories:
- 移动混合开发
tags:
- Ionic
- Cordova
- Nodejs
---

## 1、安装Node.js

```
https://nodejs.org/en/ 下载msi文件安装
```

## 2、安装openSDK1.8
```
1、访问：https://www.azul.com/downloads/zulu/zulu-windows/ 
2、下载：https://cdn.azul.com/zulu/bin/zulu8.30.0.1-jdk8.0.172-win_x64.zip
3、配置环境变量：新建 JAVA_HOME 值为：D:\Java\jdk
4、添加path： D:\Java\jdk
5、添加path： D:\Java\jdk\bin
```

## 3、更换国内npm镜像
```
1、写入配置文件： npm config set registry http://r.cnpmjs.org/
2、查看是否生效： npm config get registry
```

## 4、修改node目录用户组机安装Ionic及其Cordova
```
Mac下node目录：/usr/local/lib/node_modules
sudo chown -R johnson:staff  node_modules
npm install -g ionic
npm install -g cordova
```

## 5、安装apache ant
```
1、下载地址： https://ant.apache.org/bindownload.cgi
2、解压到： D:\Java\apache-ant-1.10.3
3、添加环境变量：ANT_HOME 值为： D:\Java\apache-ant-1.10.3
4、添加path:  %ANT_HOME%\bin
```

## 6、安装Android SDK
```
1、安装Android sdk到：D:\Java\Android\SDK
2、添加环境变量： ANDROID_HOME 值为： D:\Java\Android\SDK
3、添加path： %ANDROID_HOME%\tools
4、添加path： %ANDROID_HOME%\platform-tools
```

## 7、新建第一个测试项目
```
1、检查环境开发环境： ionic info
2、切换到项目目录
3、新建demo项目：ionic start demo
4、添加编译为Android版本： ionic cordova platform add android
5、编译：ionic build android
6、web端测试：ionic serve
7、运行在Android运行：ionic cordova run android
```


