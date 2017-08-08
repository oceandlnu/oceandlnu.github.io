---
layout: post
title: 手把手搭建LAMP环境
date: 2016-09-22 21:26:56
tags:
 - Linux
 - Apache
 - MySQL
 - PHP
categories:
 - 配置
description: 手把手搭建LAMP环境
copyright: true
---

经过很多天的研究，LAMP环境总算顺利搭建完成，之前走过很多弯路，遇到过很多问题！下面讲下我亲身搭建lamp 的经验

首先讲下我的设备配置：VMware虚拟机，centos6.5x86镜像，网络连接模式是nat模式，桥接到手机开的wifi的无线网

首先是开启网卡，设置永久开启，输入vi /etc/sysconfig/network-scripts/ifcfg-eth0找到REBOOT设置为yes选项，然后wq保存退出，

重启网络service network restart,然后ifconfig查看自己的IP，好了，ping一下百度能联网，初步准备完成！如果还不能联网，自己去百度解决方法

1：安装和配置Apache

	yum install httpd

vi /etc/httpd/conf/httpd.conf  找到ServerName，设置为自己的域名，如果没有域名，可以设置为localhost:80

vi查找
```
1、命令模式下输入“/字符串”，例如“/Section 3”。
2、如果查找下一个，按“n”即可。
```
开启apache服务， /etc/rc.d/init.d/httpd start

在windows下打开浏览器输入你虚拟机的IP地址进行测试

![](/uploads/2016-09-22/4.png)

2：安装并配置MYSQL

yum install mysql mysql-server

MySQL设置root密码

mysql_secure_installation输入回车设置完密码，一路回车！

启动MYSQL    /etc/rc.d/inti.d/mysqld start

使用命令mysql -uroot -p回车 输入密码进入mysql数据库

![](/uploads/2016-09-22/5.png)

3:安装并配置PHP

	yum install php php-mysql php-gd libjpeg* php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc php-mbstring php-mcrypt php-bcmath php-mhash libmcrypt  

重启Apache和MySQL

可以在默认的代码目录下，上传PHP文件进行测试，默认目录在/var/www/html。可以在httpd.conf文件里修改路径

在/var/www/html目录下创建个测试文件，

vi /var/www/html/test.php

![](/uploads/2016-09-22/6.png)

然后保存退出，在windows下输入（你的虚拟机IP地址）/test.php就会出现以下界面

![](/uploads/2016-09-22/7.png)

好了，最后在连接下mysql数据库，我这里已经提前创建好数据库(db_1)和表(tb_1),也插入了几条记录

最后在test.php里面写入以下代码测试是否连接数据库

![](/uploads/2016-09-22/8.png)

退出保存wq，然后刷新浏览器查看测试：

![](/uploads/2016-09-22/9.png)

好了，最后说下，

可以根据自己的具体需要，来对Apache MySQL PHP进行配置。默认的配置文件路径如下：
Apache配置文件路径：/etc/httpd/conf/httpd.conf
MySQL配置文件路径： /etc/my.cnf
PHP配置文件路径：     /etc/php.ini