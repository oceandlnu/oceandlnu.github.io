---
layout: post
title: Ubuntu安装git、nvm
date: 2017-09-16 21:26:56
tags:
 - Linux
 - nvm
 - Git
categories:
 - linux
description: Ubuntu安装git、nvm
copyright: true
---

### Git
    
    sudo apt install git
    
验证是否安装成功，查看git版本
    
    git --version
    
卸载Git

    sudo apt remove git
    sudo apt autoremove
 
### nvm

nvm是专门的node版本管理工具，可以在同一台机器上管理不同node版本。

github地址: https://github.com/creationix/nvm

#### 卸载已安装到全局的 node/npm

如果之前是在官网下载的 node 安装包，运行后会自动安装在全局目录，其中

node 命令在 /usr/local/bin/node ，npm 命令在全局 node_modules 目录中，具体路径为 /usr/local/lib/node_modules/npm

安装 nvm 之后最好先删除下已安装的 node 和全局 node 模块：

```
npm ls -g --depth=0 #查看已经安装在全局的模块，以便删除这些全局模块后再按照不同的 node 版本重新进行全局安装

sudo rm -rf /usr/local/lib/node_modules #删除全局 node_modules 目录
sudo rm /usr/local/bin/node #删除 node
cd  /usr/local/bin && ls -l | grep "../lib/node_modules/" | awk '{print $9}'| xargs rm #删除全局 node 模块注册的软链
```

#### 安装 nvm

通过curl:

    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash
    
通过wget:

    wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash


__注意：安装完成后请重新打开终端环境， oh-my-zsh 也是一样。__


执行下面语句安装最新稳定版node（自带npm）：

    nvm install node

#### 安装切换各版本 node/npm

```
nvm install node

nvm install 4.2.2
#安装 4.2.2 版本

nvm install 0.12.7
#安装 0.12.7 版本

# 特别说明：以下模块安装仅供演示说明，并非必须安装模块

nvm use 0
#切换至 0.12.7 版本

npm install -g mz-fis
#安装 mz-fis 模块至全局目录，安装完成的路径是 /Users/<你的用户名>/.nvm/versions/node/v0.12.7/lib/mz-fis

nvm use 4
#切换至 4.2.2 版本

npm install -g react-native-cli
#安装 react-native-cli 模块至全局目录，安装完成的路径是 /Users/<你的用户名>/.nvm/versions/node/v4.2.2/lib/react-native-cli

nvm alias default 0.12.7
#设置默认 node 版本为 0.12.7
```

使用nvm --help查看是否安装成功。

使用nvm ls查看已经安装的版本。

使用nvm ls-remote查看所有远端版本。

使用nvm install安装某个版本，如nvm install v5.3.0。

使用nvm use切换到某个版本，如nvm use v5.3.0使用5.3.0，nvm use system使用系统版本。

#### 使用 .nvmrc 文件配置项目所使用的 node 版本
如果你的默认 node 版本（通过 nvm alias 命令设置的）与项目所需的版本不同，则可在项目根目录或其任意父级目录中创建 .nvmrc 文件，在文件中指定使用的 node 版本号，例如：

```
cd <项目根目录>  #进入项目根目录
echo 4 > .nvmrc #添加 .nvmrc 文件
nvm use #无需指定版本号，会自动使用 .nvmrc 文件中配置的版本
node -v #查看 node 是否切换为对应版本
```

#### Ps:

类似的工具也有n命令，n 命令是作为一个 node 的模块而存在，而 nvm 是一个独立于 node/npm 的外部 shell 脚本， 因此 n 命令相比 nvm 更加局限。由于 npm 安装的模块路径均为 /usr/local/lib/node_modules ， 当使用 n 切换不同的 node 版本时，实际上会共用全局的 node/npm 目录。所以还是推荐使用nvm。

github地址：https://github.com/tj/n