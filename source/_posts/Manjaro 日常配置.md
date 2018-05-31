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

勾选 `https://mirrors.tuna.tsinghua.edu.cn/manjaro/ `，然后 OK -> 确定 。

> `Arch Linux CN` 源

```
sudo nano /etc/pacman.conf
#在文件末尾添加以下两行
[archlinuxcn]
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

> 更新缓存

	sudo pacman -Syy

> 安装 `archlinuxcn-keyring` 包导入 GPG key

	sudo pacman -S archlinuxcn-keyring

> 安装 yaourt

	sudo pacman -S yaourt

> 设置AUR源

```
sudo nano /etc/yaourtrc
# 去掉 # AURURL 的注释，修改为
AURURL="https://aur.tuna.tsinghua.edu.cn"
```

> 菜单 -> 添加/删除软件 -> 首选项 -> AUR，打开 `启用AUR支持`  勾选 `从AUR检查更新`

> 安装更新

	sudo pacman -Syyu

### 常用软件

	sudo pacman -S chromium filezilla screenfetch netease-cloud-music

其他的软件就不多说了，大家可以自行去软件包管理器或者[AUR](https://aur.archlinux.org/)上查找。

### U盘挂载(系统默认已安装，如果没有执行下面命令安装)

	sudo pacman -S udiskie

设置udiskie -2命令为开机自动后台启动即可自动挂载，[参考](https://wiki.archlinux.org/index.php/thunar#Automounting_of_large_external_drives)

### 安装WPS

	yaourt -S wps-office ttf-wps-fonts

### 安装搜狗拼音

	yaourt -S fcitx-im fcitx-configtool fcitx-sogoupinyin

> 如果出现错误：无效或已损坏的软件包 (PGP 签名)。将所有的 `SigLevel = ×××` 修改为 `SigLevel = Never` 即可。

	sudo nano /etc/pacman.conf

> 创建 `~/.xprofile` 文件，添加以下语句，否则只能在一部分窗口下输入。

```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
```

> 对于jetbrians系列fcitx无法跟随的情况 设置 > fcitx配置 > 附加组件 > 勾选 `高级`> Fcitx XIM 前端 > 点击 `配置` > 勾选 `对XIM使用On The Spot风格`

> 重启系统，就可以使用输入法了。

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
yaourt -S jq tsocks
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

> 系统默认已经安装 `pip`，想重新安装执行`yaourt -S python-pip`

```
sudo pip install genpac
# 在当前目录(比如：/home/ocean/develop)下生成autoproxy.pac
genpac --format=pac --pac-proxy="SOCKS5 127.0.0.1:1080" --pac-precise --output="autoproxy.pac"
```

> 设置全局代理，在/etc/environment文件里添加auto_proxy/AUTO_PROXY

```
auto_proxy="file:///home/ocean/develop/autoproxy.pac"
AUTO_PROXY="file:///home/ocean/develop/autoproxy.pac"
```

> 重启系统生效

### 终端代理

	yaourt -S proxychains-ng

> 编辑/etc/proxychains.conf文件，将`socks4 127.0.0.1 9095`修改为`socks5 127.0.0.1 1080`

```
sudo nano /etc/proxychains.conf
socks5 127.0.0.1 1080
```

> 使用：

	proxychains yourcommand

> 例如：

	proxychains curl www.google.com

> [自由上网](https://github.com/Alvin9999/new-pac/wiki/ss%E5%85%8D%E8%B4%B9%E8%B4%A6%E5%8F%B7)

> [逗比根据地](https://doub.io/sszhfx/)

> [SSR 账号](http://ss.pythonic.life/)

> [老D博客](https://laod.cn/)

### 安装 VirtualBox

> 查看当前的内核版本， `uname -r` ，比如输出了 `4.14.44-1-MANJARO` 内核版本为 `414`

```
yaourt -S virtualbox linux414-virtualbox-host-modules virtualbox-ext-oracle
```

> `[kernel version]-virtualbox-host-modules`  根据内核版本选择，假如我的内核版本为 `3.7.4-1-MANJARO`，则安装 `linux37-virtualbox-host-modules`

> 添加当前用户到vboxusers，如果不需要使用USB外设，可以不执行此操作。

	sudo gpasswd -a [username] vboxusers

> 例如

	sudo gpasswd -a ocean vboxusers

重新启动系统或执行 `sudo vboxreload` ，[参考](https://wiki.manjaro.org/)index.php?title=Virtualbox

### 安装oh-my-zsh、powerline

> Manjaro 自带zsh，`zsh --version` 查看，如果没有安装 执行 `yaourt -S zsh`

oh-my-zsh：http://ohmyz.sh/

```
#安装 oh-my-zsh
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
#安装powerline及字体
yaourt -S powerline powerline-fonts
```

> 编辑 `nano .zshrc` 在最后添加

```
powerline-daemon -q
. /usr/lib/python3.6/site-packages/powerline/bindings/zsh/powerline.zsh
```
> 临时切换 bash

	bash

> 临时切换 zsh

	zsh

> 修改 bash 为默认 shell

	chsh -s /bin/bash

> 修改 zsh 为默认 shell

	chsh -s /bin/zsh
