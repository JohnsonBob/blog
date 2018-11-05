---
title: Mac自动挂载硬盘
date: 2018-07-14 21:08:33
categories:
- Mac
tags:
- Mac
- 挂载硬盘
---


```
diskutil list
diskutil info /dev/disk0s3
```

##### 1、输出结果结果中找到Volume UUID，如：

```
Volume UUID:              E45555B5-CD7D-36E9-8B14-A96B8BF97AAA
```

##### 2、设置开机自动加载：

```
sudo vim /etc/fstab
```

##### 3、在打开的文件末尾添加：

```
UUID=E45555B5-CD7D-36E9-8B14-A96B8BF97AAA /Users/johnson/UserData apfs auto
```
##### 4、 :wq 保存并退出，重启生效。