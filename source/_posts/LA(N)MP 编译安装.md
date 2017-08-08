---
layout: post
title: LA(N)MP 编译安装
date: 2016-10-18 21:26:56
tags:
 - Linux
 - Apache
 - MySQL
 - PHP
categories:
 - 配置
description: LA(N)MP 编译安装
copyright: true
---

+ 注意：使用 su 或者 sudo 超级管理员权限

### PHP
```bash
# 下载 php 源码包
[root@VM_77_151_centos ~]# wget http://php.net/get/php-7.1.1.tar.gz/from/a/mirror
[root@VM_77_151_centos ~]# tar -zxvf mirror  
#安装依赖
[root@VM_77_151_centos ~]# yum install gcc gcc++ libxml2-devel
[root@VM_77_151_centos ~]# cd php-7.1.1
# --with-config-file-path=path 指定配置路径
[root@VM_77_151_centos php-7.1.1]# ./configure --prefix=/usr/local/php --enable-fpm 
[root@VM_77_151_centos php-7.1.1]# make
[root@VM_77_151_centos php-7.1.1]# make install
# 拷贝 php.ini-development 或 php.ini-production 文件 更名为到/usr/local/php7/lib目录 更名为 php.ini
[root@VM_77_151_centos php-7.1.1]# cp php.ini-development /usr/local/php7/lib/php.ini
```
### MySQL
```bash
# 下载 mysql 源码包
[root@VM_77_151_centos ~]# wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.17.tar.gz
[root@VM_77_151_centos ~]# tar -zxvf mysql-5.7.17.tar.gz  
# 安装依赖和工具
[root@VM_77_151_centos ~]# yum install cmake gcc-c++ ncurses-devel perl-Data-Dump boost boost-doc boost-devel
[root@VM_77_151_centos ~]# cd cd mysql-5.7.17
[root@VM_77_151_centos mysql-5.7.17]# cmake \
-DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
-DMYSQL_DATADIR=/mydata/mysql/data \
-DSYSCONFDIR=/etc \
-DMYSQL_USER=mysql \
-DWITH_MYISAM_STORAGE_ENGINE=1 \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_ARCHIVE_STORAGE_ENGINE=1 \
-DWITH_MEMORY_STORAGE_ENGINE=1 \
-DWITH_READLINE=1 \
-DMYSQL_UNIX_ADDR=/var/run/mysql/mysql.sock  \
-DMYSQL_TCP_PORT=3306 \
-DENABLED_LOCAL_INFILE=1 \
-DENABLED_DOWNLOADS=1 \
-DWITH_PARTITION_STORAGE_ENGINE=1 \
-DEXTRA_CHARSETS=all \
-DDEFAULT_CHARSET=utf8 \
-DDEFAULT_COLLATION=utf8_general_ci \
-DWITH_DEBUG=0 \
-DMYSQL_MAINTAINER_MODE=0 \
-DWITH_SSL:STRING=bundled \
-DWITH_ZLIB:STRING=bundled \

# 如果出现报这个（boost）错误
-- Download failed, error: 
CMake Error at cmake/boost.cmake:194 (MESSAGE):
You can try downloading
http://sourceforge.net/projects/boost/files/boost/1.59.0/boost_1_59_0.tar.gz
manually using curl/wget or a similar tool

# 在 cmake 后面加上（# 号去掉）
#-DDOWNLOAD_BOOST=1 \
#-DWITH_BOOST=/usr/share/doc/boost-doc-1.59.0/ \
#-DOWNLOAD_BOOST_TIMEOUT=3600

# 或者
[root@VM_77_151_centos mysql-5.7.17]# wget http://sourceforge.net/projects/boost/files/boost/1.59.0/boost_1_59_0.tar.gz
[root@VM_77_151_centos mysql-5.7.17]# mkdir /usr/share/doc/boost-doc-1.59.0
[root@VM_77_151_centos mysql-5.7.17]# cp ./boost_1_59_0.tar.gz /usr/share/doc/boost-doc-1.59.0/

[root@VM_77_151_centos mysql-5.7.17]# make

# 如果编译过程出现以下错误，内存不足，可以建立swap分区解决
c++: internal compiler error: Killed (program cc1plus)
Please submit a full bug report,
with preprocessed source if appropriate.
See <http://bugzilla.redhat.com/bugzilla> for instructions.
make[2]: *** [sql/CMakeFiles/sql.dir/item_geofunc.cc.o] Error 4
make[1]: *** [sql/CMakeFiles/sql.dir/all] Error 2
make: *** [all] Error 2

[root@VM_77_151_centos mysql-5.7.17]# make
[100%] Built target my_safe_process
[root@VM_77_151_centos mysql-5.7.17]# make install
# 创建 mysql 用户和 mysql 用户组
[root@VM_77_151_centos ~]# groupadd -r mysql && useradd -r -g mysql -s /bin/false -M mysql
# 创建数据库存放目录，以及权限修改
[root@VM_77_151_centos mysql]# mkdir -p /usr/local/mysql/data && chown -R root:mysql /usr/local/mysql/
[root@VM_77_151_centos mysql]# chown -R mysql:mysql /usr/local/mysql/data/
[root@VM_77_151_centos mysql]# chmod -R go-rwx /usr/local/mysql/data/
# 初始化 MySQL 数据库
[root@VM_77_151_centos bin]# /usr/local/mysql/bin/mysqld --initialize-insecure --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data
# 启动
[root@VM_77_151_centos bin]# /usr/local/mysql/support-files/mysql.server start
[root@VM_77_151_centos lib]# echo "/usr/local/mysql/lib" > /etc/ld.so.conf.d/mysql.conf
```
### Apache
```bash
[root@VM_77_151_centos ~]# wget http://mirror.bit.edu.cn/apache//httpd/httpd-2.4.25.tar.gz
[root@VM_77_151_centos ~]# tar -zxvf httpd-2.4.25.tar.gz
# 安装依赖 apr apr-util pcre
[root@VM_77_151_centos ~]# wget http://archive.apache.org/dist/apr/apr-1.5.2.tar.gz
[root@VM_77_151_centos ~]# wget http://archive.apache.org/dist/apr/apr-util-1.5.4.tar.gz
[root@VM_77_151_centos ~]# tar -zxvf apr-1.5.2.tar.gz
[root@VM_77_151_centos ~]# tar -zxvf apr-util-1.5.4.tar.gz
[root@VM_77_151_centos ~]# mv apr-1.5.2 apr
[root@VM_77_151_centos ~]# mv apr-util-1.5.4 apr-util
[root@VM_77_151_centos ~]# mv apr apr-util httpd-2.4.25/srclib/
[root@VM_77_151_centos ~]# wget https://fossies.org/linux/misc/pcre-8.39.tar.gz
[root@VM_77_151_centos ~]# tar pcre-8.39.tar.gz
[root@VM_77_151_centos ~]# cd pcre-8.39
[root@VM_77_151_centos pcre-8.39]# ./configure --prefix=/usr/local/pcre-8.39
# 编译 安装
[root@VM_77_151_centos ~]# cd ~./httpd-2.4.25
[root@VM_77_151_centos httpd-2.4.25]# ./configure --prefix=/usr/local/apache -with-pcre=/usr/local/pcre-8.39/bin/pcre-config -with-included-apr
[root@VM_77_151_centos httpd-2.4.25]# make
[root@VM_77_151_centos httpd-2.4.25]# make install
[root@VM_77_151_centos httpd-2.4.25]# cd /usr/local/apache/bin
[root@VM_77_151_centos bin]# ./apachectl -k start
# 防火墙启动80端口
[root@VM_77_151_centos bin]# firewall-cmd --zone=public --add-port=80/tcp --permanent
[root@VM_77_151_centos bin]# systemctl restart firewalld.service
```
### Nginx
```bash
[root@VM_77_151_centos ~]# wget http://nginx.org/download/nginx-1.10.3.tar.gz
[root@VM_77_151_centos ~]# tar -zxvf 1.10.3.tar.gz
[root@VM_77_151_centos ~]#cd nginx-1.10.3/
[root@VM_77_151_centos nginx-1.10.3]#  ./configure --prefix=/usr/local/nginx --with-pcre=../pcre-8.39
[root@VM_77_151_centos nginx-1.10.3]# make
[root@VM_77_151_centos nginx-1.10.3]# make install
[root@VM_77_151_centos nginx-1.10.3]# cd /usr/local/nginx/
# 启动 Nginx 前先将 Apache 的进程关闭
[root@VM_77_151_centos nginx]# ./sbin/nginx
```
### 配置PHP
```bash
# 拷贝默认配置
[root@VM_77_151_centos sbin]# cd /usr/local/php7/etc/
[root@VM_77_151_centos etc]# cp php-fpm.conf.default php-fpm.conf
[root@VM_77_151_centos etc]# cd php-fpm.d
[root@VM_77_151_centos php-fpm.d]# cp www.conf.default www.conf
# 启动 php-fpm
[root@VM_77_151_centos php-fpm.d]# cd /usr/local/php7/sbin/
```
### 配置 Nginx 解析 .php 文件
```bash
[root@VM_77_151_centos nginx]# vim conf/nginx.conf
# 添加如下配置
#http {
#    server {
#        location ~ \.php$ {
#                fastcgi_pass   127.0.0.1:9000;
#                fastcgi_index  index.php;
#                include /usr/local/nginx/conf/fastcgi_params;
#                #include fastcgi_params;
#                fastcgi_split_path_info         ^(.+\.php)(/.+)$;
#                fastcgi_param PATH_INFO         $fastcgi_path_info;
#                fastcgi_param PATH_TRANSLATED   $document_root$fastcgi_path_info;
#                fastcgi_param SCRIPT_FILENAME   $document_root$fastcgi_script_name;
#       }
#    }
#}
# 重启 Nginx
[root@VM_77_151_centos nginx]# sbin/nginx -s reload
```
### 参考

[Linux学习笔记：用fdisk工具分区，swap分区的管理](http://blog.csdn.net/tongyijia/article/details/50783083)
[PHP环境LAMP/LNMP安装与配置](http://www.imooc.com/learn/703)
[CentOS 7.1编译安装MySQL5.7.7rc](https://typecodes.com/web/centos7compilemysql.html?utm_source=tuicool&utm_medium=referral)