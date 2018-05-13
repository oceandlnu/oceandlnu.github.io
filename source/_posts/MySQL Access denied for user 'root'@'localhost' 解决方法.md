---
layout: post
title: MySQL Access denied for user 'root'@'localhost' 解决方法
date: 2018-04-10 17:56:08
tags:
 - Linux
 - MySQL
categories:
 - MySQL
description: MySQL Access denied for user 'root'@'localhost' 解决方法
copyright: true
---

>* 执行 mysql -uroot -ppassword 出现如下错误

    ERROR 1045 (28000): Access denied for user 'root'@'localhost'

### 解决方法

>* 停止mysql

```
# service mysql stop
//or
# /usr/local/mysql/support-files/mysql.server stop
```

>* 安全模式启动

```
//找到mysqld_safe所在位置
# find /usr/local/mysql -name "mysqld_safe"
# mysqld_safe --skip-grant-tables --skip-networking &
```

>* 安全模式进入mysql

```
# mysql -u root
//password替换为你要修改的密码
mysql> UPDATE mysql.user SET PASSWORD=PASSWORD('password') WHERE USER='root';
```

>* 启动mysql，问题解决

```
# service mysql start
//or
# /usr/local/mysql/support-files/mysql.server start
```