---
layout: post
title: GitHub+Hexo博客多终端同步[准备工作]
date: 2017-04-05 16:59:16
tags:
 - Hexo
 - Git
categories:
 - 基本操作
description: GitHub+Hexo博客多终端同步[准备工作]
---

```bash
1. 搭建 Node.js 环境 
2. 搭建 Git 环境 
3. 安装配置 Hexo 
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

---

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