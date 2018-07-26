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

```bash
sudo pacman-mirrors -i -c China -m rank
```

勾选 `https://mirrors.tuna.tsinghua.edu.cn/manjaro/ `，然后 OK -> 确定 。

> `Arch Linux CN` 软件源

```bash
sudo nano /etc/pacman.conf
#在文件末尾添加以下两行
[archlinuxcn]
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

> 更新缓存

```bash
sudo pacman -Syy
```

> 安装 `archlinuxcn-keyring` 包导入 GPG key

```bash
sudo pacman -S archlinuxcn-keyring
```

> 安装更新

```bash
sudo pacman -R thunar-archive-plugin
sudo pacman -Syyu
```

> 安装 yaourt

```bash
sudo pacman -S yaourt
```

> `AUR` 软件源

```bash
sudo nano /etc/yaourtrc
# 去掉 # AURURL 的注释，修改为
AURURL="https://aur.tuna.tsinghua.edu.cn"
```

> 菜单 -> 添加/删除软件 -> 首选项 -> AUR，打开 `启用AUR支持`  勾选 `从AUR检查更新`

### 常用软件

```bash
sudo pacman -S chromium filezilla screenfetch netease-cloud-music obs-studio
```

> `chromiun`安装 `flash`

```bash
yaourt -S pepper-flash
```

> 微信、TIM

```bash
yaourt -S deepin-wechat deepin.com.qq.office
```

> git配置

```bash
ssh-keygen -t rsa -C "ocean"
git config --global user.name "oceandlnu"
git config --global user.email "oceandlnu@gmail.com"
```

> 删除孤立软件包(慎用)

```bash
sudo pacman -Rs $(pacman -Qtdq)
```

+ 有需要可以自行去 `软件包管理器(添加/删除软件)` 或者[AUR](https://aur.archlinux.org/)查找软件。

### 自由截图快捷键设置

+ 菜单 -> 设置 -> 键盘 -> 应用程序快捷键 -> 添加 -> 命令 `xfce4-screenshooter -r` 点击确定，接下来会提示设置快捷键，我设置为 `ctrl+alt+A`

### 安装深度截图

```bash
yaourt -S deepin-screenshot
```

### 移动设备挂载

+ 系统默认已安装 `udiskie`，如果没有执行下面命令安装 `sudo pacman -S udiskie`

+ 菜单 -> 设置 -> 可移动驱动器和介质 -> 选择 `存储器` -> 勾选 `热插拔时挂载可移动驱动器` `插入后挂载可移动介质`(或执行`usidkie -2` 命令设置为开机启动)

### 日常开发

```bash
yaourt -S charles mysql-workbench postman-bin redis-desktop-manager haroopad
```

+ 破解 `charles`

```bash
sudo mv charles.jar /usr/share/java/charles
```

+ 如果 `redis-desktop-manager`打开失败

```bash
yaourt -R redis-desktop-manager
yaourt -S redis-desktop-manager-bin
yaourt -R redis-desktop-manager-bin
yaourt -S redis-desktop-manager
```

#### phpstorm

下载地址：http://www.jetbrains.com/

```bash
sudo nano /etc/hosts
#添加下面一行
0.0.0.0 account.jetbrains.com
```

解压安装包进入bin目录

```bash
./phpstorm.sh &
```

获取激活码，注册码激活：

http://idea.iteblog.com/
http://idea.lanyus.com/

配置文件存放目录：`~/.PhpStorm2018.1`

#### sublime text

[Sublime Text 日常配置](https://oceandlnu.github.io/2017/01/18/Sublime%20Text%20%E6%97%A5%E5%B8%B8%E9%85%8D%E7%BD%AE/)

#### wechat-dev-tool

在 `～/.local/share/applications/` 目录下创建desktop文件(nano xxx.desktop)

wechat-dev-tool

```bash
[Desktop Entry]
Version=1.0
Type=Application
Name=wechat-dev-tool
Icon=/home/ocean/develop/wechat-dev-tool/app/images/icon.png
Exec="/home/ocean/develop/wechat-dev-tool/nw" %f
Comment=The Drive to Develop
Categories=Development;IDE;
Terminal=false
```

### 安装WPS

```bash
yaourt -S wps-office ttf-wps-fonts
```

### 安装搜狗拼音

```bash
yaourt -S fcitx-im fcitx-configtool fcitx-sogoupinyin
```

+ 创建 `.xprofile` 文件，添加以下语句，否则只能在一部分窗口下输入。

```bash
nano ~/.xprofile
#添加以下语句
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
```

### SSR Client安装配置脚本

```bash
cd ~/develop
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

> [自由上网](https://github.com/Alvin9999/new-pac/wiki/ss%E5%85%8D%E8%B4%B9%E8%B4%A6%E5%8F%B7)

> [逗比根据地](https://doub.io/sszhfx/)

> [SSR 账号](http://ss.pythonic.life/)

> [老D博客](https://laod.cn/)

### 代理配置(Pac/SwitchyOmega 二选一)

#### Pac 全局代理

> 系统默认已经安装 `pip`，重新安装执行`yaourt -S python-pip`

```bash
sudo pip install genpac
# 在当前目录(比如：/home/ocean/develop)下生成autoproxy.pac
genpac --format=pac --pac-proxy="SOCKS5 127.0.0.1:1080" --pac-precise --output="autoproxy.pac"
```

> 设置全局代理，在`environment`文件里添加 `auto_proxy/AUTO_PROXY`

```bash
sudo nano /etc/environment
#添加下面任意一行
auto_proxy="file:///home/ocean/develop/autoproxy.pac"
AUTO_PROXY="file:///home/ocean/develop/autoproxy.pac"
```

#### SwitchyOmega 代理配置

[SwitchyOmega](https://switchyomega.com/index.html)

[SwitchyOmega Github](https://github.com/FelisCatus/SwitchyOmega/releases)

安装完成后，点击右上角 `SwitchyOmega` -> 选项

1.情景模式 -> proxy

| 网址协议 | 代理协议  | 代理服务器 | 代理端口 |
| :----- | :------- | :------- | :----- |
| (默认)  | SOCKS5   | 127.0.0.1 | 1080 |

2.情景模式 -> auto switch

> 规则列表设置 -> 添加规则列表

| 规则列表格式  | AutoProxy |
| :---------- | :-------- |
| 规则列表网址  | https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt |

> 切换规则

| 规则列表规则 | (按照规则列表匹配请求) | proxy |
| :--------- | :----------------- | :---- |
| 默认情景模式 |                    | 直接连接 |

3.`立即更新情景模式` -> `应用选项` 保存设置。

情景模式说明：

| proxy  | 所有URL都走代理模式 |
| :----- | :--------------- |
| auto switch | 自动根据URL判断是否走代理 |

### 终端代理

```bash
yaourt -S proxychains-ng
```

+ 编辑`proxychains.conf`文件，将`socks4 127.0.0.1 9095`修改为`socks5 127.0.0.1 1080`

```bash
sudo nano /etc/proxychains.conf
#找到最后一行，修改为
socks5 127.0.0.1 1080
```

+ 使用：

```bash
proxychains yourcommand
```

+ eg：

```bash
proxychains curl www.google.com
```

### 安装 VirtualBox

+ 查看当前的内核版本， `uname -r` ，比如输出了 `4.14.44-1-MANJARO` 内核版本为 `414`

```bash
yaourt -S virtualbox linux414-virtualbox-host-modules virtualbox-ext-oracle
```

+ `[kernel version]-virtualbox-host-modules`  根据内核版本选择，假如我的内核版本为 `3.7.4-1-MANJARO`，则安装 `linux37-virtualbox-host-modules`

+ 添加当前用户到vboxusers，如果不需要使用USB外设，可以不执行此操作。

```bash
	sudo gpasswd -a [username] vboxusers
```

+ eg

```bash
sudo gpasswd -a ocean vboxusers
```

重新启动系统或执行 `sudo vboxreload` ，[参考链接](https://wiki.manjaro.org/index.php?title=Virtualbox)

### 安装oh-my-zsh、powerline

+ Manjaro 自带zsh，`zsh --version` 查看，如果没有安装 执行 `yaourt -S zsh`

oh-my-zsh：http://ohmyz.sh/

```bash
#安装 oh-my-zsh
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
#安装powerline及字体
yaourt -S powerline powerline-fonts
```

+ 编辑 `nano .zshrc` 在最后添加

```bash
powerline-daemon -q
. /usr/lib/python3.6/site-packages/powerline/bindings/zsh/powerline.zsh
```

+ 安装 nvm(安装完成后需要重启终端)

Github：https://github.com/creationix/nvm

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | zsh
#或者
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | zsh

export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node
```

+ 安装node(最新LTS版本)

```bash
nvm install --lts
```

+ 安装hexo

```bash
#淘宝镜像加速
npm config set registry https://registry.npm.taobao.org
npm install hexo-cli -g
```

+ 临时切换 bash

```bash
bash
```

+ 临时切换 zsh

```bash
zsh
```

+ 修改 bash 为默认 shell

```bash
chsh -s /bin/bash
```

+ 修改 zsh 为默认 shell

```bash
chsh -s /bin/zsh
```

+ 免密登录

```bash
cat ~/.ssh/id_rsa.pub
#将公钥追加到远程服务器的 ~/.ssh/authorized_keys
```

+ 保持ssh连接(客户端配置)

```bash
sudo nano /etc/ssh/ssh_config
#添加下面两行
ServerAliveInterval 120
ServerAliveCountMax 60
```

[参考链接](http://einverne.github.io/post/2017/05/ssh-keep-alive.html)