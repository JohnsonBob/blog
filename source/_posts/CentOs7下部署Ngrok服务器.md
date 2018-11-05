---
title: CentOs7下部署Ngrok服务器
date: 2018-05-02 19:18:33
categories:
- Linux
- CentOs
tags:
- Linux
- Ngrok
- 内网穿透
- GO环境
---


### 1、安装git与gcc

##### 1、安装git与gcc

```
yum install mercurial git bzr subversion  gcc 
```

##### 2、 安装go语言环境

```
cd /usr/local/bin/
去官网https://golang.org/dl/下载最新安装包 
tar -zxf go1.10.2.linux-amd64.tar.gz 
```

##### 3、添加环境变量

```
vi /etc/profile 
添加：
#go lang
export GOROOT=/usr/local/bin/go
export PATH=$PATH:$GOROOT/bin
export PATH=/usr/local/git/bin:$PATH  

执行：
source /etc/profile

#查看是否安装成功
go version
go version go1.10.2 linux/amd64 
```


### 2、安装Ngrok


##### 1、下载
```
cd /usr/local/bin/ngrok
git clone https://github.com/tutumcloud/ngrok.git ngrok
```

##### 2、生成证书
```
cd ngrok 
NGROK_DOMAIN=”data.i-sanya.com”
openssl genrsa -out base.key 2048
openssl req -new -x509 -nodes -key base.key -days 10000 -subj "/CN=$NGROK_DOMAIN" -out base.pem
openssl genrsa -out server.key 2048
openssl req -new -key server.key -subj "/CN=$NGROK_DOMAIN" -out server.csr
openssl x509 -req -in server.csr -CA base.pem -CAkey base.key -CAcreateserial -days 10000 -out server.crt
```

##### 3、替换证书
```
cp base.pem assets/client/tls/ngrokroot.crt
cp server.crt assets/server/tls/snakeoil.crt
cp server.key assets/server/tls/snakeoil.key
```

##### 4、编译生成ngrokd和ngrok
```
make release-server release-client
```

##### 5、启动Ngrok：
```
./bin/ngrokd -tlsKey=server.key -tlsCrt=server.crt -domain="data.i-sanya.com" -httpAddr=":8081" -httpsAddr=":8082"
```

##### 6、编译生成ngrok（windows客户端）：
```
GOOS=windows GOARCH=amd64 make release-client
成功会在bin目录下看到windows_amd64文件夹
```

##### 7、windows端运行 
```
创建一个文件，命名为ngrok.cfg，写入一下内容
server_addr: "你的域名:4443"
trust_host_root_certs: false

再创建一个启动bat文件，命名为start.bat
ngrok -config=ngrok.cfg -subdomain 映射本地的域名 本地的端口
如ngrok -config=ngrok.cfg -subdomain sb 8081
```

##### 8、设置域名解析
```
添加两条A记录：data.i-sanya.com和*.data.i-sanya.com，指向所在的Ngrok服务器ip。
```

##### 9、设置开机启动
```
vim /etc/init.d/ngrok
添加：
cd /usr/local/bin/ngrok/ngrok
./bin/ngrokd -tlsKey=server.key -tlsCrt=server.crt -domain="data.i-sanya.com" -httpAddr=":8081" -httpsAddr=":8082"
```
