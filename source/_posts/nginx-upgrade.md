---
title: nginx upgrade & add module
date: 2016-12-22 
category: nginx
tags: nginx
---

# nginx upgrade & add module

## 目标
* 升级nginx版本 1.10.1 --> 1.10.2
* 添加缓存模块
* 添加文件上传模块和上传进度模块

## 准备工作

### 查看 nginx 已安装模块
```shell
$nginx -V
--prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib64/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-file-aio --with-threads --with-ipv6 --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_ssl_module --with-cc-opt='-O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector --param=ssp-buffer-size=4 -m64 -mtune=generic'
```
### 备份原nginx

### 下载新版本的nginx和需要添加的模块并解压
#### 下载nginx 1.10.2
```shell
$wget http://nginx.org/download/nginx-1.10.2.tar.gz
```
#### 下载 nginx-upload-module
````shell
$wget https://codeload.github.com/vkholodkov/nginx-upload-module/tar.gz/2.2.0
````
#### 下载 nginx-upload-progress-module
````shell
$wget https://github.com/masterzen/nginx-upload-progress-module/archive/v0.9.2.tar.gz
````
#### 下载 ngx_cache_purge
```shell
$wget https://codeload.github.com/FRiCKLE/ngx_cache_purge/tar.gz/2.3
```

#### 解压
```
$ tar zxf nginx-1.10.2.tar.gz
$ tar zxf nginx-upload-module-2.2.0.tar.gz
$ tar zxf nginx-upload-progress-module.tar.gz
$ tar zxf ngx_cache_purge-2.3.tar.gz
```

## 升级并添加新模块


````
./configure --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib64/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-file-aio --with-threads --with-ipv6 --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_ssl_module --add-module=/root/nginxinstall/nginx-upload-module-2.0.12 --add-module=/root/nginxinstall/nginx-upload-progress-module-82b35fc --add-module=/root/nginxinstall/ngx_cache_purge-2.3 --with-cc-opt='-O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector --param=ssp-buffer-size=4 -m64 -mtune=generic'
````

