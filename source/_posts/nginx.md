---
title: nginx安装
date: 2018-01-06 17:51:49
tags: nginx
categories:
keywords: nginx liunx
---
>安装nginx


# 安装开发环境
> Ububtu16.4安装方法：

安装g++只需要一条命令就可以了：
``` bash
$ sudo apt-get install build-essential #除了g++外，这条命令还会安装libc6-dev，make等好东东，很方便
```
> CentOS7安装方法：

`yum install gcc-c++`

## pcre zlib ssl安装
一般我们都需要先装pcre, zlib，前者为了重写rewrite，后者为了gzip压缩。

1.选定目录
`cd /usr/local/src`以下下载文件均放置在次文件夹下 请使用sudo权限
### pcre

[下载地址:ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/](ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/)

> Ububtu16.4安装方法：

``` bash
wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.41.tar.gz
tar -zxvf pcre-8.41.tar.gz
cd pcre-8.34
./configure
make
make install
```
<!--  more  -->
> CentOS7安装方法：

`yum install -y pcre pcre-devel`

### zlib
[下载地址:http://zlib.net/](http://zlib.net/)
> Ububtu16.4安装方法：

``` bash
wget http://zlib.net/zlib-1.2.11.tar.gz
tar -zxvf zlib-1.2.11.tar.gz
cd zlib-1.2.11
./configure
make
make install
```

 > CentOS7安装方法：

 `yum install -y zlib zlib-devel`

### ssl (不要make 否则有坑)
[下载地址:https://www.openssl.org/source/](https://www.openssl.org/source/)

> Ububtu16.4安装方法：

``` bash
wget https://www.openssl.org/source/openssl-1.0.2n.tar.gz
tar -zxvf openssl-1.0.2n.tar.gz

apt-get install openssl
apt-get install libssl-dev
```

> CentOS7安装方法：

`yum install -y openssl openssl-devel`

### nginx安装(我是直接安装的)
[下载地址:http://nginx.org/en/download.html](http://nginx.org/en/download.html)
> Ububtu16.4安装方法：

``` bash
wget http://nginx.org/download/nginx-1.12.2.tar.gz
tar -zxvf nginx-1.12.2.tar.gz
cd nginx-1.12.2
./configure
make
make install
```
> CentOS7安装方法：

``` bash
wget -c http://nginx.org/download/nginx-1.12.2.tar.gz
tar -zxvf nginx-1.12.2.tar.gz
cd nginx-1.12.2
./configure
make
make install
```

### 安装成功
进入/usr/local/nginx目录
``` bash
client_body_temp  fastcgi_temp  logs        sbin       uwsgi_temp
conf              html          proxy_temp  scgi_temp
```

### 启动
进入/sbin `请使用sudo权限`
启动：`./nginx`
停止：`./nginx -s stop`
重启：`./nginx -s reload`

### VUE NGINX 服务器配置
``` bash
    server {
        listen       8089;
        server_name  localhost;
        root   E:/git/Dofar/dist/;#文件地址
        index  index.html index.htm;#文件指向页面
        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
             try_files $uri $uri/ @router;#前端路由导航
             index index.html;
         }

        location @router {
            rewrite ^.*$ /index.html last;#前端路由导航
    }
```
### 负载均衡策略

nginx可以根据客户端IP进行负载均衡，在upstream里设置ip_hash，就可以针对同一个C类地址段中的客户端选择同一个后端服务器，除非那个后端服务器宕了才会换一个。

nginx的upstream目前支持的5种方式的分配

#### 1、轮询（默认）
每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能自动剔除。
>upstream backserver {
server 192.168.0.14;
server 192.168.0.15;
}

#### 2、指定权重
指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况。
>upstream backserver {
server 192.168.0.14 weight=10;
server 192.168.0.15 weight=10;
}

#### 3、IP绑定 ip_hash
每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。
>upstream backserver {
ip_hash;
server 192.168.0.14:88;
server 192.168.0.15:80;
}

#### 4、fair（第三方）
按后端服务器的响应时间来分配请求，响应时间短的优先分配。
>upstream backserver {
server server1;
server server2;
fair;
}

#### 5、url_hash（第三方）
按访问url的hash结果来分配请求，使每个url定向到同一个后端服务器，后端服务器为缓存时比较有效。
>upstream backserver {
server squid1:3128;
server squid2:3128;
hash $request_uri;
hash_method crc32;
}

在需要使用负载均衡的server中增加

>upstream backserver{
ip_hash;
server 127.0.0.1:9090 down; (down 表示单前的server暂时不参与负载)
server 127.0.0.1:8080 weight=2; (weight 默认为1.weight越大，负载的权重就越大)
server 127.0.0.1:6060;
server 127.0.0.1:7070 backup; (其它所有的非backup机器down或者忙的时候，请求backup机器)
}
location / {
    proxy_pass http://backserver/ ;
}

max_fails ：允许请求失败的次数默认为1.当超过最大次数时，返回proxy_next_upstream 模块定义的错误
fail_timeout:max_fails次失败后，暂停的时间


# 参考文档
[Nginx安装](http://www.nginx.cn/install)