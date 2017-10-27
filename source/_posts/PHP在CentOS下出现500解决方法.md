---
layout: post
title: PHP在CentOS下出现HTTP ERROR 500解决方法
date: 2017-08-26 13:41:08
tags:
 - PHP
 - CentOS
categories:
 - Linux
description: PHP在CentOS下出现HTTP ERROR 500解决方法
copyright: true
---

如图，出现HTTP ERROR 500说明你的php代码里可能有错，默认php的错误不会输出到浏览器

![](/uploads/2017-08-26/1.png)

修改 /etc/php.ini 把display_errors = Off 改成 display_errors = On，建议开发完之后关闭，只在开发的时候开启此项

![](/uploads/2017-08-26/2.png)

```
#向上搜索 ?display_errors
#向下搜索 /display_errors
# n 下一个 N 上一个

vi /etc/php.ini

#找到display_errors

display_errors = Off
改成
display_errors = On

```

重启apache 刷新页面看到显示错误信息，我们看到错误提示为require_once(): Failed opening required，打开文件请求失败，由于Linux下/表示根目录，我这是从windows直接移植过来的，将前面的分号去掉。

	#重启服务器apache
	service httpd restart

![](/uploads/2017-08-26/3.png)

如果出现 "PHP Fatal error:  Uncaught think\\exception\\ErrorException: mkdir(): Permission denied in..." 实际原因是/var/www/html/mvc_article/目录是由httpd服务的用户来读写，默认不是apache这个账户来读写，所以就抛出了没有创建目录的权限，类似写这个目录的权限也没有，需要向下面这样改变mvc_article的权限(或者读写用户和组)

	#此文件夹以及目录下的所有文件和文件夹都赋予777权限
	chmod -R 777 文件夹
	或者
	#单个赋予权限
	chmod 777 文件夹/文件名

最后，大功告成~

参考： http://blog.csdn.net/zhengzizhi/article/details/74845190