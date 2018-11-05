---
title: Centos的yum源更换为阿里云源
date: 2018-04-26 21:36:33
categories:
- Linux
- CentOs
tags:
- Linux
- CentOs
---



##### 1、备份

```
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

##### 2、下载新的CentOS-Base.repo 到/etc/yum.repos.d/

```
CentOS 5:
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-5.repo
CentOS 6:
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
CentOS 7:
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
```

##### 3、生成缓存

```
yum makecache
```