---
layout: post
title: 解决mysql_connect()，出现警告的问题
date: 2017-07-04 12:47:23
tags:
 - MySQL
 - PHP
categories:
 - MySQL
description: 解决mysql_connect()，出现警告的问题
copyright: true
---

PHP 5个版本,5.2、5.3、5.4、5.5，怕跟不上时代，新的服务器直接上5.5，但是程序出现如下错误：
```
Deprecated: mysql_connect(): The mysql extension is deprecated and will be removed in the future: use mysqli or PDO instead in
```
看意思就很明了，说mysql_connect这个模块将在未来弃用，请你使用mysqli或者PDO来替代。

#### 解决方法1：

禁止php报错
```
display_errors = On
改为
display_errors = Off
```
鉴于这个服务器都是给用户用的，有时候他们需要报错(...都是给朋友用的,^_^)，不能这做，让他们改程序吧，看方案2.

#### 解决方法2：

常用的php语法连接mysql如下
```php
<?php
$link = mysql_connect('localhost', 'user', 'password');
mysql_select_db('dbname', $link);

改成mysqi
<?php
$link = mysqli_connect('localhost', 'user', 'password', 'dbname');
```
常用mysql建表SQL如下
```php
<?php
//  老的
mysql_query('CREATE TEMPORARY TABLE `table`', $link);
// 新的
mysqli_query($link, 'CREATE TEMPORARY TABLE `table`');
```
#### 解决方法3：

在php程序代码里面设置报警级别
```php
<?php
error_reporting(E_ALL ^ E_DEPRECATED);
```
Deprecated的问题就这样解决掉了，不过还是建议大家尽快取消mysql的用法，全部都走向mysqli或者mysqlnd等等。mysql确实是太不安全而且太老旧了。