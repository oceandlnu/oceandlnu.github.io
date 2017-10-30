---
layout: post
title: CentOS安装python3.6和第三方库
date: 2017-09-07 17:56:08
tags:
 - Linux
 - Python3
categories:
 - Python3
description: CentOS安装python3.6和第三方库
copyright: true
---

如果本机安装了python2，尽量不要管他，使用python3运行python脚本就好，因为可能有程序依赖目前的python2环境，

比如yum！！！！！

不要动现有的python2环境！

一、安装python3.6

1. 安装依赖环境
	
	# yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel

2.下载Python3 https://www.python.org/ftp/python

	# wget https://www.python.org/ftp/python/3.6.3/Python-3.6.3.tgz

3.安装python3
　　我个人习惯安装在/usr/local/python3（具体安装位置看个人喜好）
　　创建目录：

	# mkdir -p /usr/local/python3

解压下载好的Python-3.x.x.tgz包(具体包名因你下载的Python具体版本不同而不同，如：我下载的是Python3.6.3.那我这就是Python-3.6.3.tgz)
```
# tar -zxvf Python-3.6.3.tgz
# chmod -R 777 Python-3.6.3
```
4.进入解压后的目录，编译安装。
```
# cd Python-3.6.3
# ./configure --prefix=/usr/local/python3
# make (或者 make install)
```
删除文件指令

	rm -rf [filename]
5.建立python3的软链

	# ln -s /usr/local/python3/bin/python3 /usr/bin/python3

6.并将/usr/local/python3/bin加入PATH
```
# vim ~/.bash_profile

# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs

PATH=$PATH:$HOME/bin:/usr/local/python3/bin

export PATH
```

按ESC，输入:wq回车退出。
修改完记得执行下面的命令，让上一步的修改生效：
```
# source ~/.bash_profile
　　检查Python3及pip3是否正常可用：

# python3 -V
Python 3.6.2
# pip3 -V
pip 9.0.1 from /usr/local/python3/lib/python3.6/site-packages (python 3.6)
```
7.不行的话在创建一下pip3的软链接(我也不清楚这一步有什么用)

	# ln -s /usr/local/python3/bin/pip3 /usr/bin/pip3

二、安装pip以及setuptools
　　毕竟丰富的第三方库是python的优势所在，为了更加方便的安装第三方库，使用pip命令，我们需要进行相应的安装。

1、安装pip前需要前置安装setuptools

命令如下：
```
wget --no-check-certificate  https://pypi.python.org/packages/source/s/setuptools/setuptools-19.6.tar.gz#md5=c607dd118eae682c44ed146367a17e26

tar -zxvf setuptools-19.6.tar.gz

cd setuptools-19.6

python3 setup.py build

python3 setup.py install
```
如果前面没布置好环境的话，就要苦逼一下了：

报错： RuntimeError: Compression requires the (missing) zlib module

我们需要在linux中安装zlib-devel包，进行支持。
```
　　yum install zlib-devel

　　需要对python3.5进行重新编译安装。

　　cd python3.6.1

　　make && make install
```
又是漫长的编译安装过程。

重新安装setuptools
```
　　python3 setup.py build

　　python3 setup.py install
```
2、安装pip

命令如下：
```
wget --no-check-certificate  https://pypi.python.org/packages/source/p/pip/pip-8.0.2.tar.gz#md5=3a73c4188f8dbad6a1e6f6d44d117eeb

tar -zxvf pip-8.0.2.tar.gz

cd pip-8.0.2

python3 setup.py build

python3 setup.py install
```
如果没有意外的话，pip安装完成。

如果没有搞好环境的话，会碰见亲切的报错：
```
　　pip3 install paramiko

　　报这个错

　　pip is configured with locations that require TLS/SSL, however the ssl module in Python is not available.
```
然后开始进行如下操作
```
　　yum install openssl
　　yum install openssl-devel
　　cd python3.6.1
　　make && make install
```