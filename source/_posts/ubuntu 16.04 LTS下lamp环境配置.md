---
layout: post
title: ubuntu 16.04 LTS下lamp环境配置
date: 2017-09-26 21:26:56
tags:
 - Linux
 - Mysql
 - Apache
 - PHP
categories:
 - linux
description: ubuntu 16.04 LTS下lamp环境配置
copyright: true
---

ctrl+alt+t打开终端

```
sudo apt-get update
sudo apt-get upgrade
```

### 安装apache2

    sudo apt-get install apache2

安装完成之后使用service apache2 status查看apahce2的状态，使用service apache2 restart重启apache2。

### 安装php7.0

    sudo apt-get install php7.0

安装完成之后可以通过php -v测试环境是否配置正确，或者通过 sudo vim /var/www/html/testphp.php 命令创建testphp.php文件,浏览器输入 http://localhost/testphp.php 进行访问，如果访问正常，则表示php安装成功。

### 安装mysql

    sudo apt-get install mysql-server

安装过程中记住自己设置的密码。使用mysql -u root -p命令，然后输入自己的密码进行数据库登录。

### 整合LAMP

#### 整合php和mysql
    sudo apt-get install php7.0-mysql

#### 整合php和Apache
    sudo apt-get install libapache2-mod-php7.0
    sudo service apache2 restart

#### 验证环境
Apache默认的网站根目录位于 /var/www/html/ ,进入这个目录，并创建 info.php

```
<?php 
phpinfo();
?>
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