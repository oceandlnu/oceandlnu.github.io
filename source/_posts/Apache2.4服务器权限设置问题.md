---
layout: post
title: Apache2.4服务器权限设置问题
date: 2016-08-25 13:47:23
tags:
 - Apache
categories:
 - 配置
description: Apache2.4服务器权限设置问题
---
Apache 从2.2升级到 Apache2.4.x 后配置文件 httpd.conf 的设置方法有了大变化，以前是将 deny from all 全部改成 Allow from all 实现外网访问，现在是将 Require all denied 以及 Require local 都改为 Require all granted 就可以了。

.htaccess 如果不起作用将 LoadModule rewrite_module modules/mod_rewrite.so 前面的注释（#）去掉就可以了。
下面看一下 Apache2.4 的变化：（官方英文说明）

### 所有的请求都被拒绝

2.2上的配置
```
Order deny,allow
Deny from all
```
2.4上的配置

	Require all denied

### 所有请求都是允许的

2.2上的配置
```
Order allow,deny
Allow from all
```
2.4上的配置
```
Require all granted
```

### 在域中的所有主机都可以访问example，所有其他外网主机的访问被拒绝

2.2上的配置
```
Order Deny,Allow
Deny from all
Allow from example.org
```
2.4上的配置

	Require host example.org

要想外网访问将 Require local 改为 Require all granted 。

### 经常会用到的： Require all denied Require all granted Require host xxx.com Require ip 192.168.1 192.168.2 Require local

举例说明

仅允许IP：192.168.0.1 访问
```
Require all granted
Require ip 192.168.0.1
```
仅禁止IP：192.168.0.1访问
```
Require all granted
Require not ip 192.168.0.1
```
允许所有访问
```
Require all granted
```
拒绝所有访问
```
Require all denied
```
默认是 Require local 仅允许本地访问。
还有好多变化，可以去官方说明详细看一下，不过只有英文版的。软件变化无常，建议大家升级前详细阅读官方更新文档，以免来个措手不及。