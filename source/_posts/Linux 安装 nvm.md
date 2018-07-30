---
layout: post
title: Linux 安装 nvm
date: 2017-09-16 21:26:56
tags:
 - Linux
 - nvm
categories:
 - linux
description: Linux 安装 nvm
copyright: true
---

> nvm是专门的node版本管理工具，可以在同一台机器上管理不同node版本。

github地址: https://github.com/creationix/nvm

#### 卸载已安装到全局的 node/npm

如果之前是在官网下载的 node 安装包，运行后会自动安装在全局目录，其中

node 命令在 /usr/local/bin/node ，npm 命令在全局 node_modules 目录中，具体路径为 /usr/local/lib/node_modules/npm

安装 nvm 之后最好先删除下已安装的 node 和全局 node 模块：

```bash
#查看已经安装在全局的模块，以便删除这些全局模块后再按照不同的 node 版本重新进行全局安装
npm ls -g --depth=0
#删除全局 node_modules 目录
sudo rm -rf /usr/local/lib/node_modules
#删除 node
sudo rm /usr/local/bin/node
#删除全局 node 模块注册的软链
cd /usr/local/bin && ls -l | grep "../lib/node_modules/" | awk '{print $9}'| xargs rm
```

#### 安装 nvm(安装完成后需要重启终端)

Github：https://github.com/creationix/nvm

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | zsh
# 或者
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | zsh
# 更换 nvm 淘宝源
export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node
```

+ 安装 node

```bash
# 最新 lts 版本
nvm install --lts
# 更换 npm 淘宝源
npm config set registry https://registry.npm.taobao.org
# 查看当前源
npm config get registry
# 补充(yarn安装，查看源，更换源)
npm install -g yarn
yarn config get registry
yarn config set registry https://registry.npm.taobao.org
```

#### 安装切换各版本 node/npm

```bash
# 最新 LTS 版本
nvm install --lts
#安装 8.11.3 版本
nvm install 8.11.3
#安装 4.2.2 版本
nvm install 4.2.2
# 特别说明：以下模块安装仅供演示说明，并非必须安装模块

#切换至 8.11.3 版本
nvm use 8
#安装 mz-fis 模块至全局目录，安装完成的路径是 /Users/<你的用户名>/.nvm/versions/node/v8.11.3/lib/mz-fis
npm install -g mz-fis
#切换至 4.2.2 版本
nvm use 4
#安装 react-native-cli 模块至全局目录，安装完成的路径是 /Users/<你的用户名>/.nvm/versions/node/v4.2.2/lib/react-native-cli
npm install -g react-native-cli
#设置默认 node 版本为 8.11.3
nvm alias default 8.11.3
```

使用 `nvm --help` 查看是否安装成功。

使用 `nvm ls` 查看已经安装的版本。

使用 `nvm ls-remote` 查看所有远端版本。

使用 `nvm install` 安装某个版本，如 `nvm install v8.11.3` 。

使用 `nvm use` 切换到某个版本，如 `nvm use v8.11.3` 使用 `8.11.3` ， `nvm use system` 使用系统版本。

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