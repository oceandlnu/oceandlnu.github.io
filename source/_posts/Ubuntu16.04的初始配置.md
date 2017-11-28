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

这里换成[阿里源](http://mirrors.aliyun.com/)，如果要换成其他的源(比如国内速度较快源地址：[163源](http://mirrors.163.com/)、[搜狐源](http://mirrors.sohu.com/)、[清华源](https://mirrors.tuna.tsinghua.edu.cn/)、[中科大](https://mirrors.ustc.edu.cn/)、[北京交通大学源码](http://mirror.bjtu.edu.cn/cn/))，可以去搜索一下，原理相同

#### 查看系统版本及内核

首先查看自己的ubuntu系统的codename，这一步很重要，直接导致你更新的源是否对你的系统起效果，查看方法：

    lsb_release -a

```
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 16.04 LTS
Release:	16.04
Codename:	xenial
```

上面显示了一些ubuntu的版本信息，需要得到的是Codename，比如，我这里是xenial

#### 确认阿里源支持

登陆一下网页： http://mirrors.aliyun.com/ubuntu/dists/

该网页显示了阿里云支持的ubuntu系统下各个Codename版本，确保自己的Codename在该网页中存在（一般都会有的）

#### 更换源

##### 1.软件包管理中心（推荐）

在软件包管理中心“软件源”中选择“中国的服务器”下mirros.aliyun.com即可自动使用

##### 2.手动更改配置文件

先将原配置文件备份

    sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup

然后打开编辑sources.list

    sudo gedit /etc/apt/sources.list

在文件最前面复制以下内容（不是覆盖原文件）

Xenial(16.04)

```
#deb cdrom:[Ubuntu 16.04 LTS _Xenial Xerus_ - Release amd64 (20160420.1)]/ xenial main restricted
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


系统初始官方源（默认）：

```
#deb cdrom:[Ubuntu 16.04 LTS _Xenial Xerus_ - Release amd64 (20160420.1)]/ xenial main restricted

# See http://help.ubuntu.com/community/UpgradeNotes for how to upgrade to
# newer versions of the distribution.
deb http://cn.archive.ubuntu.com/ubuntu/ xenial main restricted
# deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial main restricted

## Major bug fix updates produced after the final release of the
## distribution.
deb http://cn.archive.ubuntu.com/ubuntu/ xenial-updates main restricted
# deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial-updates main restricted

## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu
## team, and may not be under a free licence. Please satisfy yourself as to
## your rights to use the software. Also, please note that software in
## universe WILL NOT receive any review or updates from the Ubuntu security
## team.
deb http://cn.archive.ubuntu.com/ubuntu/ xenial universe
# deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial universe
deb http://cn.archive.ubuntu.com/ubuntu/ xenial-updates universe
# deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial-updates universe

## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu 
## team, and may not be under a free licence. Please satisfy yourself as to 
## your rights to use the software. Also, please note that software in 
## multiverse WILL NOT receive any review or updates from the Ubuntu
## security team.
deb http://cn.archive.ubuntu.com/ubuntu/ xenial multiverse
# deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial multiverse
deb http://cn.archive.ubuntu.com/ubuntu/ xenial-updates multiverse
# deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial-updates multiverse

## N.B. software from this repository may not have been tested as
## extensively as that contained in the main release, although it includes
## newer versions of some applications which may provide useful features.
## Also, please note that software in backports WILL NOT receive any review
## or updates from the Ubuntu security team.
deb http://cn.archive.ubuntu.com/ubuntu/ xenial-backports main restricted universe multiverse
# deb-src http://cn.archive.ubuntu.com/ubuntu/ xenial-backports main restricted universe multiverse

## Uncomment the following two lines to add software from Canonical's
## 'partner' repository.
## This software is not part of Ubuntu, but is offered by Canonical and the
## respective vendors as a service to Ubuntu users.
# deb http://archive.canonical.com/ubuntu xenial partner
# deb-src http://archive.canonical.com/ubuntu xenial partner

deb http://security.ubuntu.com/ubuntu xenial-security main restricted
# deb-src http://security.ubuntu.com/ubuntu xenial-security main restricted
deb http://security.ubuntu.com/ubuntu xenial-security universe
# deb-src http://security.ubuntu.com/ubuntu xenial-security universe
deb http://security.ubuntu.com/ubuntu xenial-security multiverse
# deb-src http://security.ubuntu.com/ubuntu xenial-security multiverse
```

### 更新

    sudo apt-get update

执行命令出现错误：

```
Reading package lists... Done
E: Problem executing scripts APT::Update::Post-Invoke-Success
'if /usr/bin/test -w /var/cache/app-info -a -e /usr/bin/appstreamcli;
 then appstreamcli refresh > /dev/null;
 fi'
E: Sub-process returned an error code
```

ubuntu 16.04 经常出现内部错误，错误原因是 appstreamcli 意外停止，sudo apt-get update 的时候也出现错误如上，解决方法如下：

```
sudo pkill -KILL appstreamcli
wget -P /tmp https://launchpad.net/ubuntu/+archive/primary/+files/appstream_0.9.4-1ubuntu1_amd64.deb https://launchpad.net/ubuntu/+archive/primary/+files/libappstream3_0.9.4-1ubuntu1_amd64.deb
sudo dpkg -i /tmp/appstream_0.9.4-1ubuntu1_amd64.deb /tmp/libappstream3_0.9.4-1ubuntu1_amd64.deb
```

执行完上述命令之后再次运行sudo apt-get update就不会再出现上面的错误。

__参考：__ https://askubuntu.com/questions/774986/appstreamcli-hanging-with-100-cpu-usage-during-update


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

直接用dpkg安装，报错程序还有依赖其他的包，如下：

```
ubuntu下安装chrome出现了错误：dpkg：处理 google-chrome-stable (--install)时出错： 

  正在解压缩 google-chrome-stable (从 google-chrome-stable_current_i386.deb) ...
dpkg：依赖关系问题使得 google-chrome-stable 的配置工作不能继续：
 google-chrome-stable 依赖于 libcurl3；然而：
  未安装软件包 libcurl3。
dpkg：处理 google-chrome-stable (--install)时出错：
 依赖关系问题 - 仍未被配置
```

执行安装依赖

    apt-get -f install

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

##### 三、终端输入 google-chrome-stable 即可运行，如果出现报错如下：

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
    
#### 安装WPS

##### 前言

ubuntu 16 默认安装了liberoffie，其实蛮好用的，使用页比较简单，但是不知道为什么打开Windows下用微软office创建的PPT会卡死，很蛋疼，所以想要把office软件换成WPS的Linux 版本。

##### 开始安装

STEP 1

到金山WPS的官方网址下载对应版本的deb安装包。 
WPS官方网站： http://linux.wps.cn/ （区分64位、32位）

STEP2

安装WPS的deb，执行dpkg -i 即可。

    sudo dpkg -i wps-office_10.1.0.5672-a21_amd64.deb
    
如果提示缺少所需依赖，执行

    sudo apt-get -f install
    
如果提示还缺失依赖libpng-12.0，这个在默认的apt 仓库里没有。所以需要手动下载一下。 
下载地址： https://packages.debian.org/zh-cn/wheezy/amd64/libpng12-0/download

下载完成后，执行dpkg -i 安装这个依赖。

    sudo dpkg -i libpng12-0_1.2.49-1+deb7u2_amd64.deb

STEP 3

最后一步，打开WPS 会报出一个字体缺少的对话框。 

出现提示的原因是因为WPS for Linux没有自带windows的字体，只要在Linux系统中加载字体即可。

具体操作步骤如下：

1.下载缺失的字体文件，然后复制到Linux系统中的 /usr/share/fonts 文件夹中。

国外下载地址： https://www.dropbox.com/s/lfy4hvq95ilwyw5/wps_symbol_fonts.zip

国内下载地址： http://pan.baidu.com/s/1mh0lcbY

（上述数据来源网络，侵删）

下载完成后，解压并进入目录中，继续执行：

    sudo cp * /usr/share/fonts

2.执行以下命令,生成字体的索引信息：


    sudo mkfontscale

    sudo mkfontdir

3.运行fc-cache命令更新字体缓存。

    sudo fc-cache

4.重启wps即可，字体缺失的提示不再出现。

#### 利用Google浏览器创建应用(钉钉,微信)

该功能以前是谷歌自带功能，后续版本把这个功能去掉了，但是却可以这样做得以恢复

下面开始说方法
在这个目录下 ～/.local/share/applications/ 创建各自的desktop文件(vim xxx.desktop)

钉钉
```
#!/usr/bin/env xdg-open
[Desktop Entry]
Version=1.0
Terminal=false
Type=Application
Name=钉钉
Exec=/opt/google/chrome/google-chrome "--app=https://im.dingtalk.com/"
Icon=/home/ocean/appicon/dd.png
StartupWMClass=im.dingtalk.com
```
微信
```
#!/usr/bin/env xdg-open
[Desktop Entry]
Version=1.0
Terminal=false
Type=Application
Name=微信
Exec=/opt/google/chrome/google-chrome "--app=https://wx.qq.com/?lang=zh_CN"
Icon=/home/ocean/appicon/wx.png
StartupWMClass=wx.qq.com
```

图标路径可以自己改成对应的路径，不必按照案例来做，只要能访问到就是OK的
图标另存为保存即可

![](/uploads/2017-09-25/dd.png)

![](/uploads/2017-09-25/wx.png)

#### 安装deepin定制版QQ

1.下载deepin定制版QQ8.9的.deb包（暂不安装）

http://packages.deepin.com/deepin/pool/non-free/a/apps.com.qq.im/

2.安装crossover15我在网上一个疑似破解版的

https://pan.baidu.com/s/1kVuKGq7

安装deepin-crossover-helper

https://pan.baidu.com/s/1eRCV9CI

或者

下载[官网](http://media.codeweavers.com/pub/crossover/cxlinux/demo/)试用版：

http://media.codeweavers.com/pub/crossover/cxlinux/demo/crossover_16.2.5-1.deb

安装：

    sudo dpkg crossover_16.2.5-1.deb

[__下载破解文件__](/uploads/2017-09-25/Crack.tar.gz)

https://pan.baidu.com/s/1slTLv8T

如缺包则按提示安装。

    in my case I have installed the missing libnss-mdns 32-bit by typing: 
    apt-get install libnss-mdns:i386

创建一个容器，安装QQ8.9.

破解：

在命令行输入sudo nautilus打开一个root权限的文件管理器

如要求输入密码，按提示输入

之后会弹出一个文件管理器的界面。

找到路径: /opt/cxoffice/lib/wine

把破解文件 (Crack->winewrapper.exe.so) 拖到这里。提示已有文件，点”替换“破解完成。

3.安装deepin QQ8.9包

4.开始使用(如果无法启动，关机，重启系统即可)

PS：程序安装以后可能图标会在“其他”一类，请手动移到别的分类

__解决字体乱码__

如果碰到字体乱码的问题请下载宋体后放在crossover的font文件夹内

下载地址：https://pan.baidu.com/s/1skQm1n3 

参考目录：

    /opt/cxoffice/share/wine/fonts

### 安装配置shadowsocks

1.首先使用快捷键ctrl+alt+t，打开终端

2.安装shadowsocks-qt5

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

5.运行shadowsocks-qt5（可通过搜索功能找到），填写server address,server port,password,Encryptioin Method等信息([逗比根据地](https://doub.bid/sszhfx/),[自由上网](https://github.com/Alvin9999/new-pac/wiki/ss%E5%85%8D%E8%B4%B9%E8%B4%A6%E5%8F%B7))，其他的使用默认的就行了。

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