---
title: Angular中app.module模块的语法详解
date: 2018年11月9日17:37:32
categories:
- 前端
- Angular
tags:
- Angular
---


##### 1、定义模块的语法如下：

```
@NgModuel({
    declarations: [],   // 用到的组件，指令，管道
    providers: [],      // 依赖注入服务 
    imports: [],        // 导入需要的模块
    exports: [],        // 导出的模块，跨模块交流
    entryComponents: [] // 需提前编译好的模块
    bootstrap: []       // 设置根组件

})
export class AppModule { }
```

##### 2、其中最重要的属性是：
1. declarations：声明本模块中拥有的视图类。Angular有三种视图类：组件、指令和管道。
1. exports：declarations的子集，可用于其它模块的组件模板。
1. imports：本模块声明的组件模板需要的类所在的其它模块。
1. providers：服务的创建者，并加入到全局服务列表中，可用于应用任何部分。
1. bootstrap：指定应用的主视图（称为根组件），它是所有其它视图的宿主。只有根模块才能设置bootstrap属性。
