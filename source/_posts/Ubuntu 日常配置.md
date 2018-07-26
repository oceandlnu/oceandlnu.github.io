---
layout: post
title: Ubuntu 日常配置
date: 2018-05-26 13:47:23
tags:
 - linux
 - ubuntu
categories:
 - 配置
description: Ubuntu 日常配置
copyright: true
---

### 更换国内源

系统设置 -> 软件和更新 选择下载服务器 -> "mirrors.aliyun.com"

### 安装常用软件

更新源并且升级软件

```bash
sudo apt update
sudo apt upgrade
```

#### 安装vim、chromium、filezilla

```bash
sudo apt install git vim chromium-browser filezilla -y
#GIMP图像处理，Kdenlive视频处理，p7zip支持rar压缩
sudo apt install gimp kdenlive p7zip-full -y
```

#### 安装nvm

[nvm](https://github.com/creationix/nvm)
[n](https://github.com/tj/n)

+ ps:类似的工具也有n命令，n 命令是作为一个 node 的模块而存在，而 nvm 是一个独立于 node/npm 的外部 shell 脚本， 因此 n 命令相比 nvm 更加局限。由于 npm 安装的模块路径均为 /usr/local/lib/node_modules ， 当使用 n 切换不同的 node 版本时，实际上会共用全局的 node/npm 目录。所以还是推荐使用nvm。

通过curl:

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
```

通过wget:

```bash
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
```

> 安装完成后请重新打开终端

安装node（LTS）：

```bash
export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node
nvm install --lts
#淘宝镜像加速
npm config set registry https://registry.npm.taobao.org
npm install hexo-cli -g
```

#### 安装搜狗输入法

下载：https://pinyin.sogou.com/linux/

双击安装下载的deb软件包，安装完成之后，进入语言支持->键盘输入法系统 选项旁边选择fcitx，重启电脑

使用快捷键 ctrl+space 或者 shift 切换输入法

#### 安装网易云音乐

下载：http://music.163.com/#/download

双击安装下载的deb软件包即可

#### Chromium创建桌面应用(例如：钉钉、微信)

在 ～/.local/share/applications/ 目录下创建各自的desktop文件(vim xxx.desktop)

钉钉

```bash
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

```bash
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

图标路径可以自己改成对应的路径，不必按照案例来做，只要能访问到就是OK的

![](/uploads/2017-09-25/dd.png)

![](/uploads/2017-09-25/wx.png)

创建App快捷方式：

charles
```bash
[Desktop Entry]
Version=1.0
Type=Application
Name=charles
Icon=/home/ocean/develop/charles/icon/64x64/apps/charles-proxy.png
Exec="/home/ocean/develop/charles/bin/charles" %f
Comment=The Drive to Develop
Categories=Development;IDE;
Terminal=false
```

postman
```bash
[Desktop Entry]
Version=1.0
Type=Application
Name=Postman
Icon=/home/ocean/develop/Postman/resources/app/assets/icon.png
Exec="/home/ocean/develop/Postman/Postman" %f
Comment=The Drive to Develop
Categories=Development;IDE;
Terminal=false
```

pycharm
```bash
[Desktop Entry]
Version=1.0
Type=Application
Name=Pycharm
Icon=/home/ocean/develop/pycharm-community-2018.1.2/bin/pycharm.png
Exec="/home/ocean/develop/pycharm-community-2018.1.2/bin/pycharm.sh" %f
Comment=The Drive to Develop
Categories=Development;IDE;
Terminal=false
```

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

#### 安装Postman

官网：https://www.getpostman.com/

下载解压

```bash
cd Postman/
sudo apt install libgconf-2-4 -y
./Postman &
```

#### 安装Workbench

下载地址：https://dev.mysql.com/downloads/workbench/

#### 安装VirtualBox VMware

VirtualBox下载地址：https://www.virtualbox.org/wiki/Downloads

VMware下载地址：https://www.vmware.com/cn/products/workstation-pro/workstation-pro-evaluation.html

安装Vmware:

```bash
sudo ./VMware-Workstation-Full-14.1.1-7528167.x86_64.bundle
```

#### PHPStorm Pycharm

下载地址：http://www.jetbrains.com/

解压进入bin目录，启动

```bash
./phpstorm.sh &
./pycharm.sh &
```

> 激活

修改hosts文件，添加下面一行

```bash
0.0.0.0 account.jetbrains.com
```

获取激活码，注册码激活：

http://idea.iteblog.com/
http://idea.lanyus.com/

#### Sublime Text

下载地址：http://www.sublimetext.com/

### Shadowsocksr Client

##### SSR 客户端安装配置脚本(推荐)

```bash
# 下载
curl https://raw.githubusercontent.com/the0demiurge/CharlesScripts/master/charles/bin/ssr -o "ssr"
# 或者
wget https://raw.githubusercontent.com/the0demiurge/CharlesScripts/master/charles/bin/ssr -O "ssr"
# 添加执行权限
chmod a+x ssr
sudo ln -s /home/ocean/develop/ssr /usr/bin/ssr
# 安装依赖
sudo apt install curl jq tsocks -y
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

##### SSR GUI 客户端

[ssr GUI 客户端](https://github.com/erguotou520/electron-ssr/releases)

直接下载安装就可以，不多做介绍，对于有些Linux客户端无效，不推荐。

安装ssr之后，还不能翻墙，因为没有设置代理，下面进行设置。

#### 代理配置(Pac/SwitchyOmega 二选一)

##### Pac 自动代理配置

1.genpac 生成pac(推荐)

> [genpac](https://github.com/JinnLynn/genpac)

```bash
# 安装pip
sudo apt install python-pip -y
# 安装genpac
pip install genpac
# 更新genpac
pip install --upgrade genpac
# 卸载genpac
pip uninstall genpac
# 在当前目录(比如：/home/ocean/develop)下生成autoproxy.pac
genpac --format=pac --pac-proxy="SOCKS5 127.0.0.1:1080" --pac-precise --output="autoproxy.pac"
```

> 注意：如果执行时出现无法找到命令 genpac 错误，可能是因为genpac命令没有被安装到系统路径，genpac执行入口文件被安装到了`~/.local/bin`，解决方法

> 方案一：将`~/.local/bin`添加到系统路径

```bash
sudo ln -s ~/.local/bin/genpac /usr/bin/genpac
```

> 方案二：卸载重新使用sudo安装genpac

```bash
pip uninstall genpac
sudo pip install genpac
sudo pip install --upgrade genpac
```

2.pac_get.sh 生成pac

> [pac_get](https://github.com/ToyoDAdoubi/doubi/blob/master/pac_get.sh)

```bash
# 下载脚本到当前目录
curl https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/pac_get.sh -o "pac_get"
# 或者
wget https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/pac_get.sh -O "pac_get"
chmod a+x pac_get
vim pac_get
# "__PROXY__"修改为自己的代理地址，比如"SOCKS5 127.0.0.1:1080"，否则无法翻墙
# Output_URL=生成pac文件路径，可自由修改，比如"autoproxy.pac"
# 修改保存后，执行脚本，在当前目录(比如：/home/ocean/develop)下生成autoproxy.pac
./pac_get
```

3.打开系统设置->网络->网络代理->自动，配置 URL”file://pac文件路径”，比如"file:///home/ocean/develop/autoproxy.pac"

##### SwitchyOmega 代理配置

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

4.干货分享

> [自由上网](https://github.com/Alvin9999/new-pac/wiki/ss%E5%85%8D%E8%B4%B9%E8%B4%A6%E5%8F%B7)

> [逗比根据地](https://doub.io/sszhfx/)

> [SSR 账号](http://ss.pythonic.life/)

> [老D博客](https://laod.cn/)