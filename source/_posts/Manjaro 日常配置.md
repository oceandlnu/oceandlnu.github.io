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

	sudo pacman -S git chromium filezilla netease-cloud-music screenfetch

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

### SSR Client安装配置脚本

```
# 下载
curl https://raw.githubusercontent.com/the0demiurge/CharlesScripts/master/charles/bin/ssr -o "ssr"
# 或者
wget https://raw.githubusercontent.com/the0demiurge/CharlesScripts/master/charles/bin/ssr -O "ssr"
# 添加执行权限
chmod a+x ssr
sudo ln -s /home/ocean/develop/ssr /usr/bin/ssr
# 安装依赖
yaourt -S curl jq tsocks -y
# 安装ssr客户端
ssr install
# 配置
ssr config
# 启动
ssr start
# 停止
ssr stop
# 重启
ssr restart
# 卸载
ssr uninstall
```

### Pac 全局代理

```
yaourt -S python-pip
sudo pip install --upgrade pip
sudo pip install genpac
sudo pip install --upgrade genpac
# 在当前目录(比如：/home/ocean/develop)下生成autoproxy.pac
genpac --format=pac --pac-proxy="SOCKS5 127.0.0.1:1080" --pac-precise --output="autoproxy.pac"
```

> 设置全局代理，在/etc/environment文件里添加auto_proxy/AUTO_PROXY

```
auto_proxy="file:///home/ocean/develop/autoproxy.pac"
#或者
AUTO_PROXY="file:///home/ocean/develop/autoproxy.pac"
```

> 重启系统

### 终端代理

	yaourt -S proxychains-ng

> 编辑/etc/proxychains.conf文件，将socks4 127.0.0.1 9095（tor代理）修改为socks5 127.0.0.1 1080（shadowsocks代理） 

```
sudo nano /etc/proxychains.conf
socks5 127.0.0.1 1080
```

> 使用：

	proxychains yourcommand

> 例如：

	proxychains curl www.google.com

### 干货分享

> [自由上网](https://github.com/Alvin9999/new-pac/wiki/ss%E5%85%8D%E8%B4%B9%E8%B4%A6%E5%8F%B7)

> [逗比根据地](https://doub.io/sszhfx/)

> [SSR 账号](http://ss.pythonic.life/)

> [老D博客](https://laod.cn/)

### 安装zsh、oh-my-zsh、powerline

```
#安装zsh
yaourt -S zsh
#安装 oh-my-zsh
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
#安装powerline及字体
yaourt -S powerline powerline-fonts
```

> nano .zshrc

```
#在最后增加
powerline-daemon -q
. /usr/lib/python3.6/site-packages/powerline/bindings/zsh/powerline.zsh
```
