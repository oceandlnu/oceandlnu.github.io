---
layout: post
title: ubuntu 18.04 LTS la(n)mp环境配置
date: 2018-05-03 21:26:56
tags:
 - Linux
 - Mysql
 - Apache
 - PHP
categories:
 - linux
description: ubuntu 18.04 LTS la(n)mp环境配置
copyright: true
---

ctrl+alt+t 打开终端

```
sudo apt update
sudo apt upgrade
```

### 安装apache2

    sudo apt install apache2

安装完成之后使用service apache2 status查看apahce2的状态，使用service apache2 restart重启apache2。

### 安装nginx

    sudo apt install nginx
    
安装好之后，使用 dpkg -S nginx 命令来搜索 nginx相关文件，可以从命令显示结果看出 Nginx默认的安装位置是/etc/nginx目录，其配置文件nginx.conf也是在该目录下，并且在 etc/init.d 下有 nginx的启动程序，该目录下的程序都会在系统开启时启动。

启动 Nginx服务，使用下面两个命令任意一个即可：

    sudo /etc/init.d/nginx start 
    sudo service nginx start

使用 netstat -anp 则可以看到80端口已经处于 LISTEN状态了。 

直接查看80端口可以使用命令：sudo lsof -i :80

在浏览器输入 127.0.0.1后，就可以看见 Nginx的欢迎页面了。

### 安装php7.0

    sudo apt install php7.0

安装完成之后可以通过php -v测试环境是否配置正确，或者通过 sudo vim /var/www/html/testphp.php 命令创建testphp.php文件,浏览器输入 http://localhost/testphp.php 进行访问，如果访问正常，则表示php安装成功。

### 安装mysql

    sudo apt install mysql-server

安装过程中记住自己设置的密码。使用mysql -u root -p命令，然后输入自己的密码进行数据库登录。

### 整合LA(N)MP

#### 整合php和mysql
    sudo apt install php7.0-mysql

#### 整合php和Apache
    sudo apt install libapache2-mod-php7.0
    sudo service apache2 restart
    
#### Nginx 与 PHP-FPM集成
     
__注意：__
     
在nginx配置文件中修改网站根目录路径

确保nginx 中fastcgi_pass与PHP-fpm监听同一个sock

备份/etc/nginx/sites-available/default文件 
     
    sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default.back
     
PHP-FPM 与 Nginx 通信方式有两种，一种是TCP方式，一种是unix socket 方式。
     
Unix domain socket 

可以使同一台操作系统上的两个或多个进程进行数据通信。Unix domain sockets 的接口和 Internet socket 很像，但它不使用网络底层协议来通信。
     
服务器压力不大的情况下，tcp 和 socket 差别不大，但在压力比较满的时候，用套接字方式，效果确实比较好。
     
socket方式：
     
在 /etc/nginx/sites-available/default 配置文件中（网站根目录也在是这里更改）， Nginx已经为与 PHP-FPM的整合准备好了，只需要将下面这部分改好就可以了。

    sudo vim /etc/nginx/sites-available/default

```
    # Add index.php to the list if you are using PHP
    index index.html index.htm index.nginx-debian.html;
	
    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #   include snippets/fastcgi-php.conf;
    #
    #	# With php7.0-cgi alone:
    #	fastcgi_pass 127.0.0.1:9000;
    #	# With php7.0-fpm:
    #   fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    #}
```

修改为

```
    # Add index.php to the list if you are using PHP(在下面条目中添加index.php)
    index index.html index.htm index.nginx-debian.html index.php;
    
    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #将下面4行前面的#号去掉，启动支持php模块
    #
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
    #
    #   # With php7.0-cgi alone:
    #   fastcgi_pass 127.0.0.1:9000;
    #   # With php7.0-fpm:
        fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    }
```
然后再修改 PHP-FPM的配置文件 /etc/php/7.0/fpm/pool.d/www.conf
     ，(大概36行)如下:

     sudo vim /etc/php/7.0/fpm/pool.d/www.conf

     ;  与 Nginx监听同一个 sock
     listen = /run/php/php7.0-fpm.sock

sock文件路径为 /run/php/php7.0-fpm.sock 。

配置好后重启服务：
     
     sudo service nginx restart
     sudo service php7.0-fpm restart

#### 验证环境

Apache(Nginx)默认的网站根目录位于 /var/www/html/ ,进入这个目录，并创建 info.php

```
<?php 
phpinfo();
```

在浏览器中输入 http://localhost/info.php 。

#### 排错

如果 http://localhost/info.php 页面空白，请尝试 Ctrl+F5 强制刷新页面。
如果依然空白，说明php和apache之间还需要一些配置
编辑 /etc/apache2/apache2.conf
```
<FilesMatch \.php$>
SetHandler application/x-httpd-php
</FilesMatch>
```

重启Apache

    sudo service apache2 restart

刷新 http://localhost/info.php 。此时应该可以看见phpinfo中的内容了。

### 安装phpmyadmin
参考资料：https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-16-04

sudo apt-get install phpmyadmin php-mbstring php-gettext，安装的过程中选择apache2。

安装完成之后使用如下两个命令修改支持模块:
sudo phpenmod mcrypt
sudo phpenmod mbstring
修改完成之后sudo systemctl restart apache2重启apache2服务器。在浏览器输入http://localhost/phpmyadmin/，进入熟悉的页面。一切OK。

### phpstorm安装
官方下载地址：https://www.jetbrains.com/phpstorm/download/#section=linux-version

安装包下载完成之后，提取到你想要安装的位置，然后进入bin目录，输入./phpstorm.sh打开应用。