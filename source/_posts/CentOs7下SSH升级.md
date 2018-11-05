---
title: CentOs7下SSH升级
date: 2018-04-26 16:51:33
categories:
- Linux
- CentOs
tags:
- Linux
- SSH
- Telnet
---


### 1、开启telnet防止意外导致ssh无法连接

##### 1、安装 telnet 避免 ssh 无法登录

```
yum -y install xinetd telnet telnet-server  
```

##### 2、 允许 root 账号登陆

```
echo  'pts/0'  >>/etc/securetty
echo 'pts/1' >>/etc/securetty
```

##### 3、添加防火墙端口

```
vi /etc/sysconfig/iptables  
-A INPUT -p tcp -m state --state NEW -m tcp --dport 23 -j ACCEPT  
```

##### 4、重启服务关闭 firewalld
```
systemctl restart iptables  
systemctl disable firewalld  
systemctl stop firewalldT  
```

##### 5、注册服务
```
systemctl enable telnet.socket  
systemctl start telnet.socket  
systemctl enable xinetd  
systemctl start xinetd  
systemctl restart telnet.socket    
```

### 2、升级SSH


##### 1、查看ssh版本号
```
ssh -V 
```

##### 2、下载OpenSSH
```
yum install wget
wget  https://mirror.vdms.io/pub/OpenBSD/OpenSSH/portable/openssh-7.7p1.tar.gz 
```

##### 3、备份ssh
```
mv /usr/bin/ssh /usr/local/bin/shh.bak
```

##### 4、解压：
```
tar -zxf openssh-7.7p1.tar.gz
```

##### 5、安装依赖包：
```
yum -y install pam-devel.x86_64 zlib-devel.x86_64 gcc openssl-devel 
```

##### 6、删除旧版shh
```
#rpm -e --nodeps `rpm -qa | grep openssh`
查看相关包：rpm -qa|grep openssh
删除相关包：rpm -e --nodeps xxx
```

##### 7、编译安装新版
```
cd /usr/local/bin/openssh-7.7p1
./configure 
make && make install
```

##### 8、配置sshd服务并加入开机启动
```
cp /usr/local/bin/openssh-7.7p1/contrib/redhat/sshd.init /etc/init.d/sshd
chmod +x /etc/init.d/sshd
```

##### 9、修改SSHD服务文件
```
vim /etc/init.d/sshd
修改以下内容
SSHD=/usr/sbin/sshd 为 SSHD=/usr/local/sbin/sshd
/usr/sbin/ssh-keygen -A 为 /usr/local/bin/ssh-keygen -A 
保存退出
chkconfig --add sshd 
```

##### 10、查看系统启动服务是否增加改项
```
chkconfig --list |grep sshd
#显示如下：sshd         0:off    1:off    2:on    3:on    4:on    5:on    6:off  
```

##### 11、允许root用户远程登录
```
cp /usr/local/bin/openssh-7.7p1/sshd_config  /usr/local/etc/sshd_config
vim  /usr/local/etc/sshd_config
添加：PermitRootLogin  yes

ps:
因为openssh安装好默认是不执行sshd_config文件的，所以即使在sshd_config中配置允许root用户远程登录，但是不加上这句命令，还是不会生效！
vim /etc/init.d/sshd
在 $SSHD $OPTIONS && success || failure这一行上面加上一行  
OPTIONS="-f /usr/local/etc/sshd_config" 
```

##### 12、重启生效
```
service sshd start 
```

### 3、删除Telnet

##### 12、安装测试ssh可用后删除Telnet
```
查看相关包： rpm -qa | grep telnet
删除相关包：rpm -e --nodeps xxx
systemctl disable xinetd
systemctl disable telnet.socket
```