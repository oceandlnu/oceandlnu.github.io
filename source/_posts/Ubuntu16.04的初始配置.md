---
layout: post
title: Ubuntu 16.04的初始配置
date: 2017-09-25 13:47:23
tags:
 - linux
 - ubuntu
categories:
 - 配置
description: Ubuntu 16.04的初始配置
copyright: true
---

### 删除

删除Amazon图标

    sudo apt-get remove unity-webapps-common

删除libreoffice

    sudo apt-get remove libreoffice-common

删除无用安装包

    sudo apt-get autoremove

### 换源

这里换成[阿里源](http://mirrors.aliyun.com/)，如果要换成其他的源([163源](http://mirrors.163.com/))，可以去搜索一下，原理相同

#### 1、软件包管理中心（推荐）

在软件包管理中心“软件源”中选择“中国的服务器”下mirros.aliyun.com即可自动使用

#### 2、手动更改配置文件

先将原配置文件备份

    sudo mv /etc/apt/sources.list /etc/apt/sources.list.backup

然后新建一个配置文件

    sudo gedit /etc/apt/sources.list

在文件最前面添加以下条目

Xenial(16.04)

```
# deb cdrom:[Ubuntu 16.04 LTS _Xenial Xerus_ - Release amd64 (20160420.1)]/ xenial main restricted
deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
```


或者输入以下内容

```
# deb cdrom:[Ubuntu 16.04 LTS _Xenial Xerus_ - Release amd64 (20160420.1)]/ xenial main restricted
deb-src http://archive.ubuntu.com/ubuntu xenial main restricted #Added by software-properties
deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted
deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted multiverse universe #Added by software-properties
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted multiverse universe #Added by software-properties
deb http://mirrors.aliyun.com/ubuntu/ xenial universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates universe
deb http://mirrors.aliyun.com/ubuntu/ xenial multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates multiverse
deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse #Added by software-properties
deb http://archive.canonical.com/ubuntu xenial partner
deb-src http://archive.canonical.com/ubuntu xenial partner
deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted multiverse universe #Added by software-properties
deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe
deb http://mirrors.aliyun.com/ubuntu/ xenial-security multiverse
```

### 更新

    sudo apt-get update

### 安装

更新系统源并且升级软件

```
sudo apt-get update
sudo apt-get upgrade
```

#### 安装vim

    sudo apt-get install vim

#### 安装Chrome

##### 一、通过直接下载安装Google Chrome浏览器deb包。
打开Ubuntu终端，以下为32位版本，使用下面的命令。

    wget https://dl.google.com/linux/direct/google-chrome-stable_current_i386.deb

以下为64位版本，使用下面的命令。

    wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb

下载好后，32 位安装命令:

    sudo dpkg -i google-chrome-stable_current_i386.deb

64 位安装命令:

    sudo dpkg -i google-chrome-stable_current_amd64.deb 

直接用dpkg安装，报错浏览器还有依赖其他的包

```
ubuntu下安装chrome出现了错误：dpkg：处理 google-chrome-stable (--install)时出错： 

  正在解压缩 google-chrome-stable (从 google-chrome-stable_current_i386.deb) ...
dpkg：依赖关系问题使得 google-chrome-stable 的配置工作不能继续：
 google-chrome-stable 依赖于 libcurl3；然而：
  未安装软件包 libcurl3。
dpkg：处理 google-chrome-stable (--install)时出错：
 依赖关系问题 - 仍未被配置
```

先执行

    apt-get -f install

然后在用dpkg安装

    dpkg -i google-chrome-stable_current_amd64.deb

##### 二、添加 Google Chrome 的PPA

安装Google Chrome浏览器官方PPA，打开终端然后运行下面的命令，下载签名密钥，然后执行安装命令：

```
wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add

sudo sh -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'

sudo apt-get update

sudo apt-get install google-chrome-stable
```

上面不行换下面的

```
wget -q -O - https://raw.githubusercontent.com/longhr/ubuntu1604hub/master/linux_signing_key.pub | sudo apt-key add

sudo sh -c 'echo "deb [ arch=amd64 ] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'

sudo apt-get update

sudo apt-get install google-chrome-stable
```

如果安装Google Chrome unstable 版本

    sudo apt-get install google-chrome-beta

如果安装Google Chrome beta 版本

    sudo apt-get install google-chrome-unstable

##### 三、终端输入google-chrome-stable即可运行，如果出现报错如下：

```
[0807/144244.712736:FATAL:nss_util.cc(627)] NSS_VersionCheck("3.26") failed. NSS >= 3.26 is required
Please upgrade to the latest NSS, and if you still get this error,
contact your distribution maintainer.
```

原因是说 NSS (Network Security Services ) https://en.wikipedia.org/wiki/Network_Security_Services 的版本太低，错误原因可能是由于chrome升级造成的。

解决方案，NSS升级：

    sudo apt install --reinstall libnss3

#### 安装搜狗输入法

首先卸载fcitx

    sudo apt-get remove fcitx

下载GDebi，这是一个用于安装你自己手动下载包的GUI程序，它会根据软件仓库这一实用的特性，来解算依赖关系

也可以命令行模式运行，其功能和GUI模式下完全一样

    sudo apt-get install gdebi

安装完成后，切换工作目录至搜狗文件所在的目录，如果你不确定可以继续输入ls，就可以查看当前目录中的文件

    cd 下载

然后输入

    sudo gdebi {搜狗deb包名称}

执行命令后注销，再次登陆后，点击右上角齿轮->系统设置->语言支持->键盘输入法系统 选项旁边选择fcitx，点击上面 选择或删除语言 那里添加安装好的搜狗输入法

可以使用快捷键Ctrl+空格切换输入法

#### 安装网易云音乐

先下载安装包网易云音乐

然后进入下载目录并执行安装命令
```
cd ~/下载
sudo gdebi {网易云deb包名称}
```

或者

    sudo dpkg -i <deb安装包名称>

如果出现依赖问题，需要先强制安装依赖，然后再执行安装命令

    sudo apt-get -f install

安装好之后启动网易云音乐

    netease-cloud-music

### 安装配置shadowsocks

1.首先使用快捷键ctrl+alt+t，打开我们的终端窗口

2.接着安装shadowsocks-qt5

```
sudo add-apt-repository ppa:hzwhuang/ss-qt5
sudo apt-get update
sudo apt-get install shadowsocks-qt5

```
3.然后安装genpac

```
sudo apt-get install python-pip
sudo pip install genpac
```

4.接下来使用genpac生成autoproxy.pac

    genpac -p "SOCKS5 127.0.0.1:1080" --output="autoproxy.pac"

该命令会在/home/xxx/下生成autoproxy.pac（其中xxx是用户名，比如我的是/home/ocean/）

5.运行shadowsocks-qt5（可通过搜索功能找到），填写server address,server port,password,Encryptioin Method等信息，其他的使用默认的就行了。

6.最后一步，打开 系统设置->网络->网络代理，将 方法 改为 自动，配置 Url填”file:///home/ocean/autoproxy.pac”，然后 应用到整个系统 即可

7.打开浏览器，现在可以开始科学上网了

### 移动Unity位置以及点击图标放大缩小

从 Ubuntu 11.04 中首次发布 Unity 以来，它就一直被固定在系统左侧。但从 Ubuntu 16.04 开始，用户已经可以手动选择将 Unity 栏放在桌面左侧或是底部显示，目前还没办法将其移动到顶部或右侧。

#### 移动到底部

    gsettings set com.canonical.Unity.Launcher launcher-position Bottom

#### 移动到左侧

    gsettings set com.canonical.Unity.Launcher launcher-position Left

#### 点击应用程序 Launcher 图标最小化功能
```
gsettings set org.compiz.unityshell:/org/compiz/profiles/unity/plugins/unityshell/ launcher-minimize-window true
```

### Ubuntu-实时显示网速-cpu-内存

安装
```
sudo add-apt-repository ppa:fossfreedom/indicator-sysmonitor  
sudo apt-get update
sudo apt-get install indicator-sysmonitor  
indicator-sysmonitor &
```
然后Ctrl+C就可以实现后台运行indicator-sysmonitor，看下图标效果，效果很不错！

![](/uploads/2017-09-25/1.png)

为了方便为程序添加开机启动！点击标题栏上图标，弹出菜单，选择Preferences，出现如下界面：

![](/uploads/2017-09-25/2.png)

勾选Run on startup，这样就能开机启动了。切换到 Advanced 选项，可以对要显示的信息的格式进行设置。比如要显示网速，则加上net选项，点击Test，直到效果满意再点击保存。

![](/uploads/2017-09-25/3.png)

### 安装FTP客户端filezilla

    sudo apt-get install filezilla

### ssh远程登录脚本

创建脚本

    vim conn.sh
    
输入以下内容

```$xslt
#!/bin/bash

user=用户名
host=服务器地址
passwd=密码

echo 'passwd:'$passwd
ssh $user@$host
```

设置执行权限755，然后执行脚本
```$xslt
sudo chmod 755 conn.sh
./conn.sh
```