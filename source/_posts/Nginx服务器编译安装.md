---
title: Nginx服务器搭建
categories: 后端
tags:
  - 服务器
abbrlink: 60842
date: 2019-12-25 20:42:08
---

1.下载
```
wget http://nginx.org/download/nginx-1.18.0.tar.gz
```
2.复制，解压
```
cp nginx-1.18.0.tar.gz /usr/local/
tar -xzf nginx-1.18.0.tar.gz
cd nginx-1.18.0
```
3.安装编译环境
```
yum update
yum install -y gcc gcc-c++ pcre pcre-devel zlib zlib-devel openssl openssl-devel
```
4.编译安装
```
#添加用户和组
groupadd www
useradd -g www www
#配置
./configure --prefix=/usr/local/nginx-1.18.0  --user=qgydevops  --group=qgydevops  --with-http_ssl_module --with-http_gzip_static_module --with-http_sub_module --with-http_stub_status_module
（用户组可以不添加，不配置）

#编译，安装
make && make instal
```
5.配置环境变量
```
vim /etc/profile.d/nginx.sh
文件内写入：
export NGINX_HOME=/usr/local/nginx
export PATH=$NGINX_HOME/sbin:$PATH
保存文件退出执行：
chmod +x /etc/profile.d/nginx.sh
source /etc/profile.d/nginx.sh
```


6.修改配置文件
```
vim /usr/local/nginx/conf/nginx.conf
```
7.文件内添加
```
user    www;
worker_processes  8;  
pid      /usr/local/nginx/nginx.pid;
worker_rlimit_nofile 65535;
error_log /data/wwwlog/error_nginx.log crit;
events {
    use  epoll;
    worker_connections  65535;
    multi_accept on;
}


http {
    limit_req_zone $binary_remote_addr zone=one:10m rate=1r/s;
    limit_conn_zone $binary_remote_addr zone=addr:10m;
    include       mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    server_names_hash_bucket_size 128;
    client_header_buffer_size    512k;
    client_max_body_size  100m;
    client_body_buffer_size 10m;
    large_client_header_buffers  4 512k;
    sendfile       on;
    tcp_nopush     on;
    tcp_nodelay    on;
    server_tokens off;
    keepalive_timeout  65;
    send_timeout        10;
    client_body_timeout  10;
    client_header_timeout  10;
    
    fastcgi_connect_timeout 300;
    fastcgi_send_timeout 300;
    fastcgi_read_timeout 300;
    fastcgi_buffer_size 64k;
    fastcgi_buffers 4 64k;
    fastcgi_busy_buffers_size 128k;
    fastcgi_temp_file_write_size 128k;
    fastcgi_intercept_errors on;

    gzip on;
    gzip_min_length 256k;
    gzip_buffers    16  8k;
    gzip_http_version  1.1;
    gzip_comp_level  6;
    gzip_proxied any;
    gzip_vary  on;
    gzip_types
    text/xml application/xml application/atom+xml application/rss+xml application/xhtml+xml image/svg+xml
    text/javascript application/javascript application/x-javascript
    text/x-json application/json application/x-web-app-manifest+json
    text/css text/plain text/x-component
    font/opentype application/x-font-ttf application/vnd.ms-fontobject
    image/x-icon;
    gzip_disable "MSIE [1-6]\.(?!.*SV1)";
    open_file_cache max=65535 inactive=20s;
    open_file_cache_valid 30s;
    open_file_cache_min_uses 2;
    open_file_cache_errors on;
    include vhost/*.conf;
}
```

 8.http{}配置中添加
```java
server {
      listen 80;
      server_name 你的IP地址;
      root /home/RH;
      index index.html;

      }
```
9.重启服务，配置生效
```
nginx –s reload
```

10.访问nginx，现在你可以通过公网ip (本地可以通过 localhost /或 127.0.0.1 ) 查看nginx 服务返回的信息。
```
curl -i localhost
```
 

11.修改文件夹权限，否则无法访问
```
chmod 777 RH
```

12.。浏览器访问 http://你的IP地址


### 设置开机启动

13.在系统服务目录里创建nginx.service文件
```
vi /usr/lib/systemd/system/nginx.service
```

14.写入内容如下：
```
#服务的说明
[Unit]
#描述服务
Description=nginx
#描述服务类别
After=network.target

#服务运行参数的设置  
[Service]
#forking是后台运行的形式
Type=forking
#服务的具体运行命令
ExecStart=/usr/local/nginx/sbin/nginx
#重启命令
ExecReload=/usr/local/nginx/sbin/nginx -s reload
#停止命令
ExecStop=/usr/local/nginx/sbin/nginx -s quit
#表示给服务分配独立的临时空间
PrivateTmp=true

#运行级别下服务安装的相关设置，可设置为多用户，即系统运行级别为3  
[Install]
WantedBy=multi-user.target
```

15.设置开机自启动
```
systemctl enable nginx.service
```

16.查看nginx状态
```
systemctl status nginx.service
```

17.重启nginx
```
pkill -9 nginx
ps aux | grep nginx
systemctl start nginx
```


18.查看服务状态
```
systemctl status nginx.service
```

#### 备案访问问题：

1.域名需要实名认证、

2.购买的国内云服务器。默认访问80端口，服务器需要备案，否则无法访问；访问非80端口不需要备案。

3.购买的国外云服务器。不需要备案。