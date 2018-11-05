---
title: CentOs7安装XAMPP
date: 2018-10-11 14:41:33
categories:
- Linux
- CentOs
tags:
- Linux
- Xampp
---



##### 1、根据需要选择相应的安装文件，这里我们选择了对应的64位xampp-linux-x64-5.6.15-2-installer.run


```
1、 chmod -R 755 xampp-linux-1.8.3-5-installer.run
2、 ./xampp-linux-1.8.3-5-installer.run
```

##### 2、安装完成后启动xampp服务

```
1、 /opt/lampp/lampp start
```

##### 3、停用xampp服务

```
1、 /opt/lampp/lampp stop
```

##### 4、启用和停用mysql

```
1、 /opt/lampp/lampp startmysql
2、 /opt/lampp/lampp stopmysql
```

##### 5、xampp中的命令工具在/opt/lampp/bin/目录中

```
打开 ~/.bashrc 文件  
在最后一行加入  
# PATH  
export PATH=/opt/lampp/bin:$PATH  
保存退出执行该文件中的命令  
source ~/.bashrc  
```

##### 6、连接mysql

```
1、 mysql -uroot -p
```

##### 7、远程连接phpmyadmin

```
1、 vi /opt/lampp/etc/extra/httpd-xampp.conf

修改为如下：

# since XAMPP 1.4.3
<Directory "/opt/lampp/phpmyadmin">
    AllowOverride AuthConfig Limit
#    Require local
    Order allow,deny
    Allow from all
    Require all granted
    ErrorDocument 403 /error/XAMPP_FORBIDDEN.html.var
</Directory>

```


##### 8、设置服务器目录权限

```
1、查看运行权限 ps -ef|grep httpd
2、修改用户和用户组： chown -R daemon:daemon /d2/server/nongyaojianguan/public_html/
3、修改文件夹权限：chmod -R 755 /d2/server/nongyaojianguan/public_html/

```

