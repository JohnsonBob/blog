---
title: CentOs安装Nginx+php
date: 2018-10-24 11:09:33
categories:
- Linux
- CentOs
tags:
- Nginx
- Apache
---



##### 1、下载php 并解压


```
1、 wget http://au1.php.net/distributions/php-5.6.37.tar.gz
2、tar -zxf php-5.6.37.tar.gz
```


##### 2、编译安装

```
1、yum install gcc libxml2 libxml2-devel
2、cd php-5.6.37
3、./configure --prefix=/usr/local/php --enable-fpm
4、make && make install
```


##### 3、报错：make: *** [sapi/cli/php] Error 1

```
1、 ln -s /usr/local/lib/libiconv.so.2 /usr/lib64/
2、 make ZEND_EXTRA_LIBS='-liconv'
3、 make ZEND_EXTRA_LIBS='-liconv' install
```

##### 4、配置php.ini文件

```
cp php.ini-production /usr/local/php/lib/php.ini
```

##### 5、配置php-fpm.conf文件

```
cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf
```

##### 6、配置php-fpm启动文件

```
1、cp sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm
2、chmod a+x /etc/init.d/php-fpm
3、/etc/init.d/php-fpm start
```

##### 7、查看cgi启动状态

```
netstat -tunlp |grep 9000
```

## 2、安装nginx
#### 1、安装依赖

```
 yum install gcc-c++  
 yum install pcre pcre-devel  
 yum install zlib zlib-devel  
 yum install openssl openssl--devel  
```

#### 2、下载

```
1、cd /usr/local 
2、wget http://nginx.org/download/nginx-1.13.12.tar.gz
3、tar -zxf nginx-1.13.12.tar.gz
4、cd nginx-1.13.12
```

#### 3、检查是否存在nginx 不存在则删除

```
1、find -name nginx
2、yum remove nginx
```

#### 4、使用--prefix参数指定nginx安装的目录,make、make install安装

```
1、./configure  //默认安装在/usr/local/nginx   
2、make  
3、make install      
```

#### 4、启动、重启、停止、强制关闭

```
1、/usr/local/nginx/sbin/nginx
2、 /usr/local/nginx/sbin/nginx –s reload
3、/usr/local/nginx/sbin/nginx –s stop
4、 pkill nginx
```
