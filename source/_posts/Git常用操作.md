---
layout: post
title: Git常用操作
date: 2016-07-03 21:26:56
tags:
 - GitHub
 - Git
categories:
 - 基本操作
description: Git常用操作
---
把这个目录变成Git可以管理的仓库

	$ git init
文件添加到仓库（如果有）

	$ git add README.md
不但可以跟单一文件，还可以跟通配符，更可以跟目录。一个点就把当前目录下所有未追踪的文件全部add了，注意add和.之间有一个空格

	$ git add .
把文件提交到仓库

	$ git commit -m "提交的描述"
关联远程仓库

	$ git remote add origin git@github.com:oceandlnu/oceandlnu.github.io.git
把本地库的所有内容推送到远程库上

	$ git push -u origin master
提交修改，只需要下面三个命令

```bash
git add .
git commit -m "描述"
$ git push -u origin master
```