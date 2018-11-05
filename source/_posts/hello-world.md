---
title: Hello World
date: 2018-04-26 09:18:33
categories:
tags:
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## Quick Start

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

### 修改默认语言为中文

``` 
修改hexo配置文件 _config.yml文件
language: zh-Hans
```

### 启动分类和标签

``` 
修改主题 _config.yml文件
menu:
  home: / || home
  tags: /tags/ || tags
  categories: /categories/ || th
  archives: /archives/ || archive
```

### 创建分类页面

``` 
hexo new page categories
编辑categories文件夹下的index.md
```

```
---
title: 分类
type: "categories" #将页面的类型设置为categories
date: 2018-11-05 16:12:21
---
```

### 创建标签页面

``` 
hexo new page tags
编辑categories文件夹下的index.md
```

```
---
title: 标签
type: "tags" #将页面的类型设置为tags
date: 2018-11-05 16:12:21
---
```



More info: [Deployment](https://hexo.io/docs/deployment.html)
