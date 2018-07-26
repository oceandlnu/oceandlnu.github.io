---
layout: post
title: Apache/Nginx重写URL配置
date: 2018-04-10 17:56:08
tags:
 - Linux
 - Apache
 - Nginx
categories:
 - Nginx
 - Apache
description: Apache/Nginx重写URL配置
copyright: true
---

>* 作者系统是Ubuntu，CentOS及其他linux发行版请自行变更
   PHP框架是ThinkPHP5.0，官方文档说的有些模糊，所以自行补充了一下
   有错误欢迎随时指出

可以通过URL重写隐藏应用的入口文件index.php,下面是相关服务器的配置参考：

### [Apache]

1.启用 `rewrite` 模块

```bash
sudo a2enmod rewrite
#或者
sudo ln -s /etc/apache2/mods-available/rewrite.load /etc/apache2/mods-enabled/rewrite.load
```

2.编辑配置文件 `/etc/apache2/apache2.conf`，找到自己web根目录对应的位置

```php
<Directory /var/www/>
Options Indexes FollowSymLinks
AllowOverride None
Require all granted
</Directory>
```

3.将 `AllowOverride None` 改为 `AllowOverride All`

4.重启服务 `sudo service apache2 restart`

5.把下面的内容保存为 `.htaccess` 文件放到应用入口文件的同级目录下(默认已创建，如果没有自己创建)

```php
<IfModule mod_rewrite.c>
Options +FollowSymlinks -Multiviews
RewriteEngine on

RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^(.*)$ index.php?/$1 [QSA,PT,L]
</IfModule>
```

### [Nginx]

在Nginx低版本中，是不支持PATHINFO的，但是可以通过在Nginx中配置转发规则实现，

编辑文件 `/etc/nginx/sites-available/default`：

```php
 server { // …..省略部分代码
   root /var/www/html;
   //找到这个模块，然后填入下面的配置
 }
  location / { // …..省略部分代码
   if (!-e $request_filename) {
   rewrite  ^(.*)$  /index.php?s=/$1  last;
   break;
    }
 }
```

其实内部是转发到了ThinkPHP提供的兼容URL，利用这种方式，可以解决其他不支持PATHINFO的WEB服务器环境。

如果你的应用安装在二级目录，Nginx的伪静态方法设置如下，其中 `/tp5/public/` 是所在的目录名称。
```php
location /tp5/public/ {
    if (!-e $request_filename){
        rewrite  ^/tp5/public/(.*)$  /tp5/public/index.php?s=/$1  last;
    }
}
```
原来的访问URL： `http://serverName/index.php/模块/控制器/操作/[参数名/参数值...]`

设置后，我们可以采用下面的方式访问： `http://serverName/模块/控制器/操作/[参数名/参数值...]`

如果你没有修改服务器的权限，可以在 `index.php` 入口文件做修改，这不是正确的做法，并且不一定成功，视服务器而定，只是在框架执行前补全 `PATH_INFO` 参数

```php
$_SERVER['PATH_INFO'] = $_SERVER['REQUEST_URI' ];
```

最后重启服务器

```bash
sudo service nginx restart
```