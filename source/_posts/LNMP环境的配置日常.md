---
layout: post
title: LNMP环境的配置日常
date: 2016-11-12 21:26:56
tags:
 - Linux
 - Nginx
 - MySQL
 - PHP
categories:
 - 基本操作
description: LNMP环境的配置日常
---
在搭建好的lnmp环境中使用配置，使环境易于管理。

找到mysql配置文件：
find / -name my.cnf
注释掉bind-address = 0.0.0.0，使数据库支持远程访问。
防火墙禁止了3306端口，以iptable为例
vi /etc/sysconfig/iptables
增加下面一行：
-A RH-Firewall-1-INPUT -m state –state NEW -m tcp -p tcp –dport 3306-j ACCEPT
重启后生效

```bash
service iptables start
nginx.conf去掉ci的index.php:
location /{
try_files $uri $uri/ /index.php?$uri&$args;
}
```
```bash
设置php.ini的cgi.fix_pathinfo=1
登录mysql:
mysql -u root -p
查看所有数据库:
show databases;
创建数据库:
create database name;
选择数据库:
use databasename;
直接删除数据库:
drop database name;
显示表:
show tables;
表的详细描述:
describe tablename;
查看mysql用户信息:
use mysql;
select from mysql.user;
添加mysql用户(本地/远程):
grant all privileges on . to admin@localhost identified by ‘password’ with grant option;
grant all privileges on . to admin@”%” identified by ‘password’ with grant option;
刷新内容：
flush privileges;
查看mysql端口:
show global variables like ‘port’;
```

计划任务：
编辑/etc/crontab文件，每天凌晨3点半备份mysql数据库。
30 3 run-parts /data/mysql/mysql_clear.sh
mysql_clear.sh文件内容：

```bash
#设置mysql备份目录
folder=/data/backup_sql/
cd $folder
day=`date +%Y%m%d`
rm -rf $day
mkdir $day
cd $day
#数据库服务器，一般为localhost
host=localhost
#用户名
user=root
#密码
password=root
#要备份的数据库
db=wechat
#数据要保留的天数
days=3
mysqldump -h$host -u$user -p$password $db>backup.sql
zip backup.sql.zip backup.sql
rm -rf backup.sql
cd ..
day=`date -d “$days days ago” +%Y%m%d`
rm -rf $day
```