---
layout: post
title: GitHub+Hexo博客多终端同步[准备工作]
date: 2017-03-06 16:59:16
tags:
 - Hexo
 - Git
categories:
 - 基本操作
description: GitHub+Hexo博客多终端同步[准备工作]
copyright: true
password: 
---

1. 搭建 Git 环境 
2. 搭建 Node.js 环境 
3. 安装配置 Hexo 
4. GitHub 注册和配置 
5. 关联 Hexo 与 GitHub Pages 
6. GitHub Pages 地址解析到个人域名 

### 搭建 Git 环境

为什么要搭建 git 环境？ - 因为需要把本地的网页和文章等提交到 GitHub 上。
Git 是一款免费、开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。

```bash
sudo apt install git
git --version
```

出现版本号则说明 Git 环境配置成功，第一步完成！！！

### 搭建 Node.js 环境

为什么要搭建 node.js 环境？ - 因为 Hexo 博客系统是基于 Node.js 编写的
Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境，可以在非浏览器环境下，解释运行 JS 代码。

#### nvm 安装(安装完成后需要重启终端)

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | zsh
# 或者
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | zsh
# 更换 nvm 淘宝源
echo "export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node" >> ~/.zshrc
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

#### 官方安装

Node.js 官网：https://nodejs.org/en/ 下载安装包 LTS，根据官方说明文档安装

```bash
node -v
npm -v
```

出现版本号则说明 Node.js 环境配置成功，第二步完成！！！

### 安装配置 Hexo

Hexo 是一个快速、简洁且高效的博客框架，使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

强烈建议你花20分钟区读一读 Hexo 的官方文档：https://hexo.io/zh-cn/

使用 npm 安装 Hexo：在命令行中输入

```bash
npm install hexo-cli -g
```

然后你将会看到下图，可能你会看到一个WARN，但是不用担心，这不会影响你的正常使用。

查看Hexo的版本

```bash
hexo -v
```

到此搭建 Hexo 博客的相关环境配置已经完成，下面开始讲解 Hexo 的相关操作

__注意__：

- 如果是多终端hexo博客同步，下面的步骤不用执行了，到这里就结束

- 如果clone失败，可能是密钥没有配好，可以查看：关联 Hexo 与 GitHub Pages里面的SSH Key配置步骤

---

### GitHub 注册和配置

GitHub 是一个代码托管平台，因为只支持 Git 作为唯一的版本库格式进行托管，故名 GitHub。

Github注册：https://github.com/

创建仓库：Repository name 使用自己的用户名，仓库名规则：

注意：yourname 必须是你的用户名。

```bash
yourname/yourname.github.io
```

访问 yourname.github.io，如果可以正常访问，那么 Github 的配置已经结束了。

安装 Hexo 完成后，请执行下列命令来初始化 Hexo，用户名改成你的，Hexo 将会在指定文件夹中新建所需要的文件。

```bash
hexo init oceandlnu.github.io
cd oceandlnu.github.io
npm install
```

新建完成后，指定文件夹的目录如下：

```json
.
├── .deploy         #需要部署的文件
├── node_modules    #Hexo插件
├── public          #生成的静态网页文件
├── scaffolds       #模板
├── source          #博客正文和其他源文件，404、favicon、CNAME 都应该放在这里
| ├── \_drafts       #草稿
| └── \_posts        #文章
├── themes          #主题
├── \_config.yml     #全局配置文件
└── package.json    #npm 依赖等
```

运行本地 Hexo 服务
```bash
hexo server
或者
hexo s
# 如果报错，执行
hexo generate
```

您的网站会在 `http://localhost:4000` 下启动。如果 `http://localhost:4000` 能够正常访问，则说明 Hexo 本地博客已经搭建起来了，只是本地哦，别人看不到的。

__注意：如果 http://localhost:4000 不能访问，那有可能是端口被占用了，上面的命令加上端口号就行了，如下：__

```bash
#5000是你想改成的端口号，不加的话，默认是4000
hexo s -p 5000
```

### 关联 Hexo 与 GitHub Pages

我们如何让本地git项目与远程的github建立联系呢？用 SSH keys

检查是否已生成密钥， `cd ~/.ssh`，`ls` 如果有3个文件，则密钥已经生成，`id_rsa.pub` 就是公钥

```bash
YANG@DESKTOP-VLST063 MINGW64 /d/hexo/oceandlnu.github.io (master|REBASE 1/2)
$ cd ~/.ssh
YANG@DESKTOP-VLST063 MINGW64 ~/.ssh
$ ls
id_rsa  id_rsa.pub  known_hosts
```
生成SSH keys，输入你自己的邮箱地址

```bash
ssh-keygen -t rsa -C "ocean"
```

在回车中会提示你输入一个密码，这个密码会在你提交项目时使用，如果为空的话提交项目时则不用输入，我们按回车不设置密码。

添加 SSH Key 到 GitHub

打开 `~/.ssh/id_rsa.pub`，此文件里面内容为刚才生成的密钥，准确的复制这个文件的内容，粘贴到 https://github.com/settings/keys 的 `new SSH key` 中

测试，输入下面的命令，看看设置是否成功，git@github.com的部分不要修改：

```bash
ssh -T git@github.com
```

如果是下面的反馈：
```
The authenticity of host ‘github.com (207.97.227.239)’ can’t be established. 
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48. 
Are you sure you want to continue connecting (yes/no)?
```
不要紧张，输入yes就好，然后会看到：

```bash
Hi oceandlnu! You've successfully authenticated, but GitHub does not provide shell access.
```

配置Git个人信息

现在你已经可以通过 SSH 链接到 GitHub 了，还有一些个人信息需要完善的。 
Git 会根据用户的名字和邮箱来记录提交。GitHub 也是用这些信息来做权限的处理，输入下面的代码进行个人信息的设置，把名称和邮箱替换成你自己的。

```bash
git config --global user.name "oceandlnu"
git config --global user.email "136494666@qq.com"
```

配置 Deployment

在_config.yml文件中，找到Deployment，然后按照如下修改，用户名改成你的：

需要注意的是：冒号后面记得空一格！
```bash
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@github.com:oceandlnu/oceandlnu.github.io.git
  branch: master
```
本地文件提交到 GitHub Pages

```bash
// 删除旧的 public 文件
hexo clean

// 生成新的 public 文件
hexo generate
或者
hexo g

// 开始部署
hexo deploye
或者
hexo d
```

在浏览器中输入 https://oceandlnu.github.io （用户名改成你的）看到了 Hexo 与 GitHub Pages 已经成功关联了。

注意1：若上面操作失败，则需要提前安装一个扩展：

	npm install hexo-deployer-git --save

注意2：如果在执行 hexo d 后,出现 error deployer not found:github 的错误（如下），则是因为没有设置好 public key 所致，重新详细设置即可。

```bash
Permission denied (publickey). 
fatal: Could not read from remote repository. 
Please make sure you have the correct access rights 
and the repository exists.
```

注意3：怎么避免 .md 文件被解析？

请查看[Hexo 怎么避免 .md 文件被解析？](https://oceandlnu.github.io/2017/02/10/Hexo%20%E6%80%8E%E4%B9%88%E9%81%BF%E5%85%8D%20.md%20%E6%96%87%E4%BB%B6%E8%A2%AB%E8%A7%A3%E6%9E%90%EF%BC%9F/)

### GitHub Pages 地址解析到个人域名

Github Pages 是面向用户、组织和项目开放的公共静态页面搭建托管服 务，站点可以被免费托管在 Github 上，你可以选择使用 Github Pages 默 认提供的域名 github.io 或者自定义域名来发布站点。
看着博客的域名是二级域名，总有一种寄人篱下的感觉，为了让这个小窝看起来更加正式，我在阿里云上买了一个域名，打算将博客绑定自己的域名。

进行该绑定过程，其实就是一个重定向的过程。

在 GitHub 仓库的根目录下建立一个 CNAME 的文本文件(注意：没有扩展名)，文件里面只能输入一个你的域名，不能加http://

	www.xxxxxx.com

注意：CNAME 一定是在你 Github 项目的 master 根目录下

进入阿里云域名解析地址，添加解析：

1.记录类型选择CNAME
2.主机记录填www
3.解析线路选择默认
4.记录值填yourname.github.io
5.TTL值为10分钟
6.再添加一个解析，记录类型A
7.主机记录填www
8.解析线路选择默认
9.记录值填你GitHub 的ip地址（在cmd中ping：）

	ping oceandlnu.github.com

点击保存，等 1 分钟，访问下你自己的域名，一切就ok了。

域名绑定成功，域名解析成功，因此你在浏览中输入 www.xxxxxx.com，或 xxxxxx.com 就可以访问到博客了，输入 oceandlnu.github.io 会重定向到 www.xxxxxx.com。过程：www 的方式，会先解析成 http://xxxx.github.io，然后根据 CNAME 再变成 www

__注意：CNAME文件在下次 hexo deploy的时候就消失了，需要重新创建，这样就很繁琐__

方法一：每次 hexo d 之后，就去 GitHub 仓库根目录新建 CNAME文件

方法二：在 hexo g 之后， hexo d 之前，把CNAME文件复制到 “\public\” 目录下面，里面写入你要绑定的域名。

方法三（推荐）：将需要上传至github的内容放在source文件夹，例如CNAME、favicon.ico、images等，这样在 hexo d 之后就不会被删除了。

方法四：通过安装插件实现永久保留

	$ npm install hexo-generator-cname --save

之后在_config.yml中添加一条

	plugins:
	- hexo-generator-cname

需要注意的是：如果是在github上建立的CNAME文件，需要先clone到本地，然后安装插件，在deploy上去即可。CNAME只允许一个域名地址。

注意1：每次生成的 CNAME 都是 yoursite.com 怎么解决？

修改 _config.yml

```bash
url: http://www.xxxxxx.com
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:
```