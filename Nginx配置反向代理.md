---
title: Nginx 初探
tags: Nginx
---

Nginx初探及第一次使用Nginx配置反向代理记录

<!--more-->

# Nginx

网页服务器

# 工作环境

- app
  node httpserver
  服务器（10.47.12.119:3010）
- Nginx
  服务器（10.47.12.119:80）
- 客户端
  IP：10.47.16.41

# Nignx配置文件

*/etc/nginx/nginx.conf*
```
http {
    include  /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile  on;
    #tcp_nopush  on;

    keepalive_timeout  65;

    #gzip  on;

    include  /etc/nginx/conf.d/*.conf;
}
```

*/etc/nginx/conf.d/test.conf*

```
server {
    listen 80;
    server_name devops.app.com;
    root /home/litong/www/MyTodoList;
    location /app/ {
        proxy_pass http://localhost:3010/;
    }
    location / {
        proxy_pass http://localhost:3010/;
    }
}
```

- http 主机（一个）
- server 域名（多个）
- location URL（多个）

# 常用Nginx操作

- nginx -t
- nginx -s reload
- 日志查看
```
cd /var/log/nginx/
tail -f error.log
```

# 遇到的问题

13：Permission denied while connecting to upstream

原因：是SeLinux的导致的

解决方案：
1. [关闭SELINUX](http://www.hpboys.com/824.html)

*简要步骤*
```
cd /usr/sbin/sestatus -v //SELinux status: enabled
setenforce 0
```

2. 执行下面的命令

```
setsebool -P httpd_can_network_connect 1
```