---
layout: post
title: PHP在CentOS下出现HTTP ERROR 500解决方法
date: 2017-10-26 13:41:08
tags:
 - MySQL
categories:
 - SQL
description: PHP在CentOS下出现HTTP ERROR 500解决方法
copyright: true
---

![](/uploads/2017-10-26/1.png)

如上图，出现HTTP ERROR 500说明你的php代码里可能有错，默认php的错误不会输出到浏览器
可以修改/etc/php.ini 把

display_errors = Off
改成
display_errors = On

![](/uploads/2017-10-26/2.png)

```
#相关命令
vi /etc/php.ini
#找到display_errors(命令?display_errors，n向上，m向下)
display_errors = Off
改成
display_errors = On
```

然后重启apache 再访问就应该会显示错误。

	service httpd restart

![](/uploads/2017-10-26/3.png)

然后看到错误首先想是不是路径不对，window和Linux查找路径方法不一样，将路径改好之后

然后发现还不行，实际原因是/var/www/html/mvc_article/目录是由httpd服务的用户来读写，默认不是apache这个账户来读写，所以就抛出了没有创建目录的权限，类似也有写这个目录的权限没有，也需要向下面这样改变runtime的读写用户和组

	chmod -R 777 文件夹（该文件夹以及目录下得所有文件/文件夹都赋予权限）
	或者
	chmod 777 文件夹/文件名（一个一个赋予权限）

最后，大功告成~