---
layout: post
title: Git 常见问题
date: 2016-12-24 21:26:56
tags:
 - Git
categories:
 - 基本操作
description: Git 常见问题
copyright: true
---

### 错误：failed to push some refs to

> 在使用git 对源代码进行push到gitHub时可能会出错，如下

```bash
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

出现错误的主要原因是github中的README.md文件不在本地代码目录中

可以通过如下命令进行代码合并【注：pull=fetch+merge]

	$ git pull --rebase origin master

执行上面代码后可以看到本地代码库中多了README.md文件

此时再执行语句 git push -u origin master即可完成代码上传到github

### 错误：Git Push Error: insufficient permission for adding an object to repository database

> 进入项目 `根目录/.git/objects` ，yourname 替换为你的用户名，yourgroup 替换为你的用户所属组

```
cd .git/objects
sudo chown -R yourname:yourgroup *
```