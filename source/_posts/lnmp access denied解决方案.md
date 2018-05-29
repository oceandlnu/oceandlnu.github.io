---
layout: post
title: lnmp access denied解决方案
date: 2018-05-12 17:56:08
tags:
 - Linux
 - Nginx
 - MySQL
 - PHP
categories:
 - Nginx
description: lnmp access denied解决方案
copyright: true
---

>* 前提：lnmp安装完成后，访问页面出现access denied

>* vim /usr/local/nginx/conf/vhost/xxx.conf 添加三行，如下

```
server {
  listen 8000;
  server_name alitong.com;
  access_log off;
  index index.html index.htm index.php;
  root /code/ali_childrens_electric_business_live;
  
  include /usr/local/nginx/conf/rewrite/ecshop.conf;
  #error_page 404 /404.html;
  #error_page 502 /502.html;
  
  #允许跨域，仅测试开放
  location / {
        add_header Access-Control-Allow-Origin *;
  }
  
  location ~ [^/]\.php(/|$) {
    #增加下面三行
    fastcgi_split_path_info ^(.+\.php)(/?.+)$;
    fastcgi_param PATH_INFO $fastcgi_path_info;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    #fastcgi_pass remote_php_ip:9000;
    fastcgi_pass unix:/dev/shm/php-cgi.sock;
    fastcgi_index index.php;
    include fastcgi.conf;
  }

  location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|flv|mp4|ico)$ {
    expires 30d;
    access_log off;
  }
  location ~ .*\.(js|css)?$ {
    expires 7d;
    access_log off;
  }
  location ~ /\.ht {
    deny all;
  }
}
```

>* 重启nginx，解决

	service nginx restart

>* 如果上面操作之后还不行 vim /usr/local/php/etc/php.ini

```
#yyp复制一行备份
cgi.fix_pathinfo=1
;cgi.fix_pathinfo=0
```