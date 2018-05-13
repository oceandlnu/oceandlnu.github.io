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

#### 安装vim、Chromium

    sudo apt install vim chromium-browser filezilla -y
    sudo apt install gimp kdenlive p7zip-full -y

#### 安装搜狗输入法

下载：https://pinyin.sogou.com/linux/?r=pinyin

双击安装下载的deb软件包，安装完成之后，进入语言支持->键盘输入法系统 选项旁边选择fcitx，重启电脑

使用快捷键ctrl+space或者shift切换输入法

#### 安装网易云音乐

下载：http://music.163.com/#/download

双击安装下载的deb软件包即可

#### Chromium浏览器创建应用(钉钉,微信)

该功能以前是谷歌自带功能，后续版本把这个功能去掉了，但是却可以这样做得以恢复

下面开始说方法
在这个目录下 ～/.local/share/applications/ 创建各自的desktop文件(vim xxx.desktop)

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

附上创建应用快捷方式(例如Phpstorm)：

```
[Desktop Entry]
Version=1.0
Type=Application
Name=PhpStorm
Icon=/home/ocean/develop/PhpStorm-181.4668.78/bin/phpstorm.png
Exec="/home/ocean/develop/PhpStorm-181.4668.78/bin/phpstorm.sh" %f
Comment=The Drive to Develop
Categories=Development;IDE;
Terminal=false
StartupWMClass=jetbrains-phpstorm
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

### shadowsocksr客户端(ssr)

#### 安装配置

##### ssr下载

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

出现下面的提示,说明运行成功

```
2017-10-17 12:30:49 INFO     local.py:50 local start with protocol[auth_chain_a] password [Ck6295iFwq] method [none] obfs [tls1.2_ticket_auth] obfs_param []
2017-10-17 12:30:49 INFO     local.py:54 starting local at 127.0.0.1:1080
2017-10-17 12:30:49 INFO     asyncdns.py:324 dns server: [('127.0.1.1', 53)]
2017-10-17 12:30:57 INFO     util.py:85 loading libcrypto from libcrypto.so.1.0.0
```

如果失败，根据提示信息安装依赖

    sudo apt install curl jq tsocks -y
    //再次运行
    python local.py -c /etc/shadowsocks.json

##### ssr GUI 客户端

[ssr GUI 客户端](https://github.com/erguotou520/electron-ssr/releases)

直接下载安装就可以，不多做介绍，对于有些Linux客户端无效，不推荐。

安装ssr之后，还不能翻墙，因为没有开启代理端口，下面进行设置。

##### SwitchyOmega 配置代理

https://switchyomega.com/index.html

[Github](https://github.com/FelisCatus/SwitchyOmega/releases)

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

SwitchyOmega情景模式选择"proxy"（都走代理模式）或者"auto switch"（自动根据URL来决定是否使用代理）就可以科学上网，推荐使用"auto switch"。

#### pac代理配置

1.安装genpac

```
sudo apt-get install python-pip
sudo pip install genpac
```

2.接下来使用genpac生成autoproxy.pac

    genpac -p "SOCKS5 127.0.0.1:1080" --output="autoproxy.pac"

该命令会在/home/xxx/下生成autoproxy.pac（其中xxx是用户名，比如我的是/home/ocean/）

3.打开系统设置->网络->网络代理，将 方法 改为 自动，配置 Url填”file:///home/ocean/autoproxy.pac”

4.干货分享 ([逗比根据地](https://doub.bid/sszhfx/),[自由上网](https://github.com/Alvin9999/new-pac/wiki/ss%E5%85%8D%E8%B4%B9%E8%B4%A6%E5%8F%B7))。

#### 点击应用程序 Launcher 图标放大缩小功能

```
gsettings set org.compiz.unityshell:/org/compiz/profiles/unity/plugins/unityshell/ launcher-minimize-window true
```