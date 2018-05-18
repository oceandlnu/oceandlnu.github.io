---
layout: post
title: Ubuntu 18.04 LTS 初始安装
date: 2018-04-28 13:47:23
tags:
 - linux
 - ubuntu
categories:
 - 配置
description: Ubuntu 18.04 LTS 初始安装
copyright: true
---

### 更换阿里源

#### 查看系统版本及内核

首先查看自己的ubuntu系统的codename，这一步很重要，直接导致你更新的源是否对你的系统起效果，查看方法：

    lsb_release -a

```
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 18.04 LTS
Release:    18.04
Codename:   bionic
```

上面显示了一些ubuntu的版本信息，codename->xenial

#### 确认阿里源支持

登陆一下网页： http://mirrors.aliyun.com/ubuntu/dists/

该网页显示了阿里云支持的ubuntu系统下各个Codename版本，确保自己的Codename在该网页中存在（一般都会有的）

#### 更换源

##### 1.软件包管理中心（推荐）

系统设置 -> 软件和更新 选择下载服务器 -> "mirrors.aliyun.com"

##### 2.手动更改配置文件

先将原配置文件备份

    sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup

然后打开编辑sources.list,替换默认的 archive.ubuntu.com 为 mirrors.aliyun.com

    sudo vi /etc/apt/sources.list

最后的效果如下：

bionic(18.04)

```
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```

### 安装常用软件

更新源并且升级软件

```
sudo apt update
sudo apt upgrade
```

#### 安装vim、chromium、filezilla、git

    sudo apt install vim chromium-browser filezilla git -y
    #GIMP图像处理，Kdenlive视频处理，p7zip支持rar压缩
    sudo apt install gimp kdenlive p7zip-full -y

#### 安装nvm、node

通过curl:

    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
    
通过wget:

    wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash

__注意：安装完成后请重新打开终端环境， oh-my-zsh 也是一样。__


执行下面语句安装最新稳定版node（自带npm）：

    nvm install node

#### 安装搜狗输入法

下载：https://pinyin.sogou.com/linux/

双击安装下载的deb软件包，安装完成之后，进入语言支持->键盘输入法系统 选项旁边选择fcitx，重启电脑

使用快捷键 ctrl+space 或者 shift 切换输入法

#### 安装网易云音乐

下载：http://music.163.com/#/download

双击安装下载的deb软件包即可

#### Chromium创建桌面应用(例如：钉钉、微信)

该功能以前是chromium/chrome自带功能，后续版本把这个功能去掉了，但是却可以这样做得以恢复

在 ～/.local/share/applications/ 目录下创建各自的desktop文件(vim xxx.desktop)

钉钉

```
#!/usr/bin/env xdg-open
[Desktop Entry]
Version=1.0
Type=Application
Name=钉钉
Icon=/home/ocean/appicon/dd.png
Exec=/opt/google/chrome/google-chrome "--app=https://im.dingtalk.com/"
Terminal=false
StartupWMClass=im.dingtalk.com
```

微信

```
#!/usr/bin/env xdg-open
[Desktop Entry]
Version=1.0
Type=Application
Name=微信
Icon=/home/ocean/appicon/wx.png
Exec=/opt/google/chrome/google-chrome "--app=https://wx.qq.com/?lang=zh_CN"
Terminal=false
StartupWMClass=wx.qq.com
```

创建App快捷方式(如Pycharm)：

```
[Desktop Entry]
Version=1.0
Type=Application
Name=Pycharm
Icon=/home/ocean/develop/Pycharm/bin/pycharm.png
Exec="/home/ocean/develop/Pycharm/bin/pycharm.sh" %f
Terminal=false
```

图标路径可以自己改成对应的路径，不必按照案例来做，只要能访问到就是OK的

![](/uploads/2017-09-25/dd.png)

![](/uploads/2017-09-25/wx.png)

#### 安装Postman

官网：https://www.getpostman.com/

下载解压

    cd Postman/
    sudo apt install libgconf-2-4
    ./Postman &

#### 安装Workbench

下载地址：https://dev.mysql.com/downloads/workbench/

#### 安装VirtualBox VMware

VirtualBox下载地址：https://www.virtualbox.org/wiki/Downloads

VMware下载地址：https://www.vmware.com/cn/products/workstation-pro/workstation-pro-evaluation.html

安装Vmware:

    sudo ./VMware-Workstation-Full-14.1.1-7528167.x86_64.bundle

#### PHPStorm Pycharm

下载地址：http://www.jetbrains.com/

解压进入bin目录，启动

    ./phpstorm.sh &
    ./pycharm.sh &

#### Sublime Text

下载地址：http://www.sublimetext.com/

### Shadowsocksr Client

##### SSR 客户端一键安装配置脚本(推荐)

```
# 下载
wget https://raw.githubusercontent.com/the0demiurge/CharlesScripts/master/charles/bin/ssr -O "ssr"
# 或者
curl https://raw.githubusercontent.com/the0demiurge/CharlesScripts/master/charles/bin/ssr -o "ssr"
chmod a+x ssr
sudo ln -s /home/xxx/ssr /usr/bin/ssr
# 安装依赖
sudo apt install curl jq tsocks -y
# 首次使用先安装ssr client
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

##### SSR 客户端

[ssr releases](https://github.com/shadowsocksrr/shadowsocksr/releases)

下载之后，解压zip文件，更改文件夹名称为shadowsocksr

解压的文件夹， 有一个文件 config.json ，这个就是配置文件的模板，我们可以复制一份到/etc/shadowsocks.json，然后对这个文件进行配置：

    sudo cp config.json /etc/shadowsocks.json
    sudo vim config.json

```
{
    "server": "0.0.0.0",
    "server_ipv6": "::",
    "server_port": 8388,
    "local_address": "127.0.0.1",
    "local_port": 1080,

    "password": "m",
    "method": "aes-128-ctr",
    "protocol": "auth_aes128_md5",
    "protocol_param": "",
    "obfs": "tls1.2_ticket_auth_compatible",
    "obfs_param": "",
    "speed_limit_per_con": 0,
    "speed_limit_per_user": 0,

    "additional_ports" : {}, // only works under multi-user mode
    "additional_ports_only" : false, // only works under multi-user mode
    "timeout": 120,
    "udp_timeout": 60,
    "dns_ipv6": false,
    "connect_verbose_info": 0,
    "redirect": "",
    "fast_open": false
}
```

主要用到的配置是下面的这几个选项：

```
"server": "0.0.0.0",       //服务器
"server_port":888,         //端口
"password":"password",     //密码
"method":"aes-256-cfb",    //加密方式
"protocol":"origin",       //协议
"obfs":"http_simple",      //混淆
 ```

运行程序

进入shadowsocksr/shadowsocks/目录里面，执行：

    python local.py -c /etc/shadowsocks.json

##### SSR GUI 客户端

[ssr GUI 客户端](https://github.com/erguotou520/electron-ssr/releases)

直接下载安装就可以，不多做介绍，对于有些Linux客户端无效，不推荐。

安装ssr之后，还不能翻墙，因为没有设置代理，下面进行设置。

#### 代理配置(Pac/SwitchyOmega 二选一)

##### Pac 自动代理配置

1.genpac 生成pac(推荐)

> [genpac](https://github.com/JinnLynn/genpac)

```
# 安装pip
sudo apt install python-pip
# 安装genpac
pip install genpac
# 更新genpac
pip install --upgrade genpac
# 卸载genpac
pip uninstall genpac
# 在当前目录(比如：/home/xxx/)下生成autoproxy.pac
genpac --format=pac --pac-proxy="SOCKS5 127.0.0.1:1080" --pac-precise --output="autoproxy.pac"
```

> 注意：如果执行时出现无法找到命令的错误，可能是因为genpac命令没有被安装到系统路径，genpac执行入口文件被安装到了~/.local/bin，解决方法

> 方案一：将~/.local/bin添加到系统路径

```
sudo ln -s ~/.local/bin/genpac /usr/bin/genpac
```

> 方案二：卸载重新使用sudo安装genpac

```
pip uninstall genpac
sudo pip install genpac
sudo pip install --upgrade genpac
```

2.pac_get.sh 生成pac

> [pac_get](https://github.com/ToyoDAdoubi/doubi/blob/master/pac_get.sh)

```
# 下载脚本到当前目录
wget https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/pac_get.sh -O "pac_get"
# 或者
curl https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/pac_get.sh -o "pac_get"
chmod a+x pac_get
vim pac_get
# "__PROXY__"修改为自己的代理地址，比如"SOCKS5 127.0.0.1:1080"，否则无法翻墙
# Output_URL=生成pac文件路径，可自由修改，比如"autoproxy.pac"
# 修改保存后，执行脚本，在当前目录(比如：/home/xxx/)下生成autoproxy.pac
./pac_get
```

3.打开系统设置->网络->网络代理->自动，配置 URL”file://pac文件路径”，比如"file:///home/ocean/autoproxy.pac"

4.干货分享

> [自由上网](https://github.com/Alvin9999/new-pac/wiki/ss%E5%85%8D%E8%B4%B9%E8%B4%A6%E5%8F%B7)

> [逗比根据地](https://doub.bid/sszhfx/)

##### SwitchyOmega 代理配置

[SwitchyOmega](https://switchyomega.com/index.html)

[SwitchyOmega Github](https://github.com/FelisCatus/SwitchyOmega/releases)

安装完成后，拓展程序->选项

进入：情景模式->proxy 设置

| 代理协议  | 代理服务器 | 代理端口 |
| :--- | :-- | :-- |
| SOCKS5| 127.0.0.1 | 1080 |

进入：情景模式->auto switch 

| 规则列表规则(按照规则列表匹配请求) | 默认情景模式 |
| :--- | :-- |
| proxy | 直接连接 |

点击 添加规则列表

| 规则列表格式  | 规则列表网址 |
| :--- | :-- |
| AutoProxy | https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt |

点击"立即更新情景模式"，然后点击左边的"应用选项"应用更改。

情景模式说明：

| proxy  | auto switch |
| :----- | :---------- |
| 所有URL都走代理模式| 自动根据URL判断是否走代理 |