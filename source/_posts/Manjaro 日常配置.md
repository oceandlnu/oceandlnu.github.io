---
layout: post
title: Manjaro 日常配置
date: 2018-05-29 10:47:23
tags:
 - linux
 - manjaro
categories:
 - 配置
description: Manjaro 日常配置
copyright: true
---

### 更换国内源

清华源：https://mirrors.tuna.tsinghua.edu.cn/
中科大：https://mirrors.ustc.edu.cn/
阿里源：https://opsx.alibaba.com/

> 列出可用中国镜像站列表：

	sudo pacman-mirrors -i -c China -m rank

勾选 https://mirrors.tuna.tsinghua.edu.cn/manjaro/ ，然后 OK -> 确定 。

> ArchlinuxCN 镜像

```
sudo nano /etc/pacman.conf
#在文件末尾添加以下两行
[archlinuxcn]
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

> 更新软件包缓存

	sudo pacman -Syy

> 安装 archlinuxcn-keyring 包导入 GPG key

	sudo pacman -S archlinux-keyring archlinuxcn-keyring

> 安装 yaourt

	sudo pacman -S yaourt

> 设置AUR源

```
sudo nano /etc/yaourtrc
# 去掉 # AURURL 的注释，修改为
AURURL="https://aur.tuna.tsinghua.edu.cn"
```

> 安装更新

	sudo pacman -Syu

### 常用软件

	sudo pacman -S git chromium filezilla netease-cloud-music

其他的软件就不多说了，大家可以自行去[AUR](https://aur.archlinux.org/)上查找.

### U盘挂载安装

	sudo pacman -S udiskie

设置udiskie -2命令为开机自动后台启动即可自动挂载

> 参考：https://wiki.archlinux.org/index.php/thunar#Automounting_of_large_external_drives

### 安装搜狗拼音

	sudo pacman -S fcitx-im fcitx-configtool fcitx-sogoupinyin

> 如果出现错误：无效或已损坏的软件包 (PGP 签名)

```
sudo nano /etc/pacman.conf
# 将所有的SigLevel = ××× 修改为 SigLevel = Never即可。
```

> 创建 `~/.xprofile` 文件，添加以下语句，否则只能在一部分窗口下输入。

```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx
```

> 注销再登录， 就可以使用搜狗输入法了。

### 安装WPS

WPS官方网站： http://linux.wps.cn/ 

	yaourt -S wps-office

打开WPS 如果出现一个字体缺少的对话框。

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

### 安装proxychains-ng

```
yaourt -S proxychains-ng
sudo nano /etc/proxychains.conf
#文件末尾修改
socks5 127.0.0.1 1080
```

使用样例：

	proxychains curl www.google.com