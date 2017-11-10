---
layout: post
title: linux安装Git、Nodejs
date: 2017-09-16 21:26:56
tags:
 - Linux
 - Nodejs
 - Git
categories:
 - linux
description: linux安装Git、Nodejs
copyright: true
---

### nvm安装

nvm是专门的node版本管理工具，可以在同一台机器上管理不同node版本。

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

    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.0/install.sh | bash  #具体最新版见github

安装完成后请重新打开终端环境， oh-my-zsh 也是一样。

#### 安装切换各版本 node/npm

```
nvm install stable #安装最新稳定版 node，现在是 5.0.0
nvm install 4.2.2 #安装 4.2.2 版本
nvm install 0.12.7 #安装 0.12.7 版本

# 特别说明：以下模块安装仅供演示说明，并非必须安装模块
nvm use 0 #切换至 0.12.7 版本
npm install -g mz-fis #安装 mz-fis 模块至全局目录，安装完成的路径是 /Users/<你的用户名>/.nvm/versions/node/v0.12.7/lib/mz-fis
nvm use 4 #切换至 4.2.2 版本
npm install -g react-native-cli #安装 react-native-cli 模块至全局目录，安装完成的路径是 /Users/<你的用户名>/.nvm/versions/node/v4.2.2/lib/react-native-cli

nvm alias default 0.12.7 #设置默认 node 版本为 0.12.7
```

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

### Node.js 编译安装(不推荐)

#### 安装、源码下载

下载最新版本node的源代码：下载 https://nodejs.org/en/download/

解压下载文件：

    tar -xzvf node-v4.2.1.tar.gz

切换工作目录至源代码目录：

    cd node-v4.2.1

configure配置安装文件：

    ./configure

make编译码代码： (编译Node源码时间较长，我编译用了大约40分左右。)

    make

make install安装：

    sudo make install

查看安装：(显示版本号)

    node -v

#### 卸载

保留编译完成的源码包，或者自己再重新编译生成个

干掉make install命令时装进去的文件,需要管理员身份

    sudo make uninstall

只删除make时产生的临时文件：

    make clean

同时删除configure和make产生的临时文件

    make distclean

升级Node版本

直接下载源码重新编译，安装。（覆盖了旧的版本）

#### Ps:

也同样适用与其他linux平台编译安装软件。