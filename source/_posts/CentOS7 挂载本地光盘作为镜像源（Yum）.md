---
title: CentOS7 挂载本地光盘作为镜像源
date: 2018-06-23 15:28:33
categories:
- Linux
- CentOs
tags:
- Linux
- CentOs
- 挂载虚拟光驱
---


### 1、将光盘镜像复制到文件夹下并挂载

```
mkdir /mnt/iso
mount -t iso9660 -o loop /root/yumISO/CentOS-7-x86_64-Everything-1804.iso /mnt/iso
```

### 2、配置开机自动挂载

```
vim /etc/fstab
添加：/root/yumISO/CentOS-7-x86_64-Everything-1804.iso /mnt/iso  iso9660  loop 0 0
```

### 3、配置源
#### 3.1 备份源
```
cd /etc/yum.repos.d/
mkdir repo_bak
mv *.repo repo_bak
vim CentOS-Base.repo
```

#### 3.2 添加源配置（在repoCentOS-Base.repo文件中添加如下内容）

```
[CentOS7-Localsource]  
name=CentOS7  
baseurl=file:///mnt/iso  
enabled=1  
gpgcheck=0 
```

#### 3.3 生成本地yum缓存

```
yum clean
yum makecache
yum list
```
