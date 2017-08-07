---
layout: post
title: GitHub+Hexo博客多终端同步
date: 2017-04-05 16:59:16
tags:
 - Hexo
 - Git
categories:
 - 基本操作
description: GitHub+Hexo博客多终端同步
---

```bash
1. 搭建 Node.js 环境 
2. 搭建 Git 环境 
3. 安装配置 Hexo 
4. 关联 Hexo 与 GitHub Pages 
```

### 搭建 Node.js 环境

为什么要搭建 node.js 环境？ - 因为 Hexo 博客系统是基于 Node.js 编写的
Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境，可以在非浏览器环境下，解释运行 JS 代码。

在 Node.js 官网：https://nodejs.org/en/ 下载安装包 v6.10.3 LTS

保持默认设置即可，一路Next，安装很快就结束了。

然后打开命令提示符，输入 node -v、npm -v，出现版本号则说明 Node.js 环境配置成功，第一步完成！！！

### 搭建 Git 环境

为什么要搭建 git 环境？ - 因为需要把本地的网页和文章等提交到 GitHub 上。
Git 是一款免费、开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。

在 Git 官网：https://git-scm.com/ 下载安装包 Git-2.13.0-64-bit.exe



桌面右键，打开 Git Bush Here，输入 git --version，出现版本号则说明 Git 环境配置成功，第二步完成！！！

### 安装配置 Hexo

Hexo 是一个快速、简洁且高效的博客框架，使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

强烈建议你花20分钟区读一读 Hexo 的官方文档：https://hexo.io/zh-cn/



使用 npm 安装 Hexo：在命令行中输入

	npm install hexo-cli -g

然后你将会看到下图，可能你会看到一个WARN，但是不用担心，这不会影响你的正常使用。



查看Hexo的版本

	hexo -v

安装 Hexo 完成后，请执行下列命令来初始化 Hexo，用户名改成你的，Hexo 将会在指定文件夹中新建所需要的文件。

```bash
hexo init oceandlnu.github.io

cd oceandlnu.github.io

npm install
```

新建完成后，指定文件夹的目录如下：

.
├── .deploy         #需要部署的文件
├── node_modules    #Hexo插件
├── public          #生成的静态网页文件
├── scaffolds       #模板
├── source          #博客正文和其他源文件，404、favicon、CNAME 都应该放在这里
| ├── _drafts       #草稿
| └── _posts        #文章
├── themes          #主题
├── _config.yml     #全局配置文件
└── package.json    #npm 依赖等

运行本地 Hexo 服务
```bash
hexo server
或者
hexo s -p 5000 #5000改成你想改成的端口号，默认是4000，可能被占用
```
您的网站会在 http://localhost:4000 下启动。如果 http://localhost:4000 能够正常访问，则说明 Hexo 本地博客已经搭建起来了，只是本地哦，别人看不到的。下面，我们要部署到Github。

### 关联 Hexo 与 GitHub Pages

我们如何让本地git项目与远程的github建立联系呢？用 SSH keys

生成SSH keys

输入你自己的邮箱地址

	ssh-keygen -t rsa -C "136494666@qq.com"

在回车中会提示你输入一个密码，这个密码会在你提交项目时使用，如果为空的话提交项目时则不用输入，我们按回车不设置密码。

添加 SSH Key 到 GitHub

打开 C:\Users\bxm09\.ssh\id_rsa.pub，此文件里面内容为刚才生成的密钥，准确的复制这个文件的内容，粘贴到 https://github.com/settings/ssh 的 new SSH key 中

测试

可以输入下面的命令，看看设置是否成功，git@github.com的部分不要修改：

	ssh -T git@github.com

如果是下面的反馈：

The authenticity of host ‘github.com (207.97.227.239)’ can’t be established. 
RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48. 
Are you sure you want to continue connecting (yes/no)?

不要紧张，输入yes就好，然后会看到：

Hi oceandlnu! You've successfully authenticated, but GitHub does not provide shell access.

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

在浏览器中输入 https://oceandlnu.github.io （用户名改成你的）看到了 Hexo 与 GitHub Pages 已经成功关联了，哇哇哇哇哇哇，开心死你了，不要忘了回来给我点赞哟 ~

注意1：若上面 hexo d 操作失败，则需要提前安装一个扩展：

	npm install hexo-deployer-git --save

注意2：如果在执行 hexo d 后,出现 error deployer not found:github 的错误（如下），则是因为没有设置好 public key 所致，重新详细设置即可。
```
Permission denied (publickey). 
fatal: Could not read from remote repository. 
Please make sure you have the correct access rights 
and the repository exists.
```