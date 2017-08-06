---
layout: post
title: 如何解决failed to push some refs to git
date: 2016-12-24 21:26:56
tags:
 - GitHub
 - Git
categories:
 - 基本操作
description: 如何解决failed to push some refs to git
---

在使用git 对源代码进行push到gitHub时可能会出错，信息如下

```bash
YANG@DESKTOP-VLST063 MINGW64 /d/hexo/oceandlnu.github.io (master|REBASE 1/2)
$ git push -u origin master
To github.com:oceandlnu/oceandlnu.github.io.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'git@github.com:oceandlnu/oceandlnu.github.io.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

此时很多人会尝试下面的命令把当前分支代码上传到master分支上。

	$ git push -u origin master

但依然没能解决问题

出现错误的主要原因是github中的README.md文件不在本地代码目录中

可以通过如下命令进行代码合并【注：pull=fetch+merge]

	$ git pull --rebase origin master
执行上面代码后可以看到本地代码库中多了README.md文件

此时再执行语句 git push -u origin master即可完成代码上传到github