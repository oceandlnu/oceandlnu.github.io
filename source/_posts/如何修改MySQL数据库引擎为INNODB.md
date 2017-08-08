---
layout: post
title: 如何修改MySQL数据库引擎为INNODB
date: 2017-05-07 13:41:08
tags:
 - MySQL
categories:
 - SQL
description: 如何修改MySQL数据库引擎为INNODB
copyright: true
---
对于MySQL数据库，如果你要使用事务以及行级锁就必须使用INNODB引擎。如果你要使用全文索引，那必须使用myisam。 INNODB的实用性，安全性，稳定性更高但是效率比MYISAM稍差，但是有的功能是MYISAM没有的。修改MySQL的引擎为INNODB，可以使用外键，事务等功能，性能高。本文主要介绍如何修改MySQL数据库引擎为INNODB，接下来我们开始介绍。

首先修改my.ini，在[mysqld]下加上：

	default-storage-engine=INNODB

其中的INNODB是要指定的数据库引擎名称。

用sql语句修改已经建成表的引擎：
	
	alter table tableName type=InnoDB  

下面贴出我的my.ini文件供参考：

```
[mysqld]  
 
basedir=C:\Program Files\VertrigoServ\Mysql\  
 
datadir=C:\Program Files\VertrigoServ\Mysql\data\  
 
port =3306 
 
key_buffer =64M 
 
max_allowed_packet =1M 
 
table_cache =128 
 
sort_buffer_size =512K 
 
net_buffer_length =8K 
 
read_buffer_size =256K 
 
read_rnd_buffer_size =512K 
 
myisam_sort_buffer_size =68M 
 
default-storage-engine=INNODB 
 
[mysqldump]  
 
quick  
 
max_allowed_packet =116M 
 
[mysql]  
 
no-auto-rehash  
 
# Remove the next comment character if you are not familiar with SQL  
 
#safe-updates  
 
[isamchk]  
 
key_buffer =20M 
 
sort_buffer_size =20M 
 
read_buffer =62M 
 
write_buffer =62M 
 
[myisamchk]  
 
key_buffer =20M 
 
sort_buffer_size =20M 
 
read_buffer =62M 
 
write_buffer =62M 
 
[mysqlhotcopy]  
 
interactive-timeout
```

按照以上的代码提示操作，我们就能够成功地修改MySQL数据库引擎为INNODB了。