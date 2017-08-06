---
layout: post
title: Hexo 怎么避免 .md 文件被解析？
date: 2017-02-10 21:26:56
tags:
 - GitHub
 - Git
 - Hexo
categories:
 - 基本操作
description: Hexo 怎么避免 .md 文件被解析？
---
Hexo原理就是hexo在执行hexo generate时会在本地先把博客生成的一套静态站点放到public文件夹中，在执行hexo deploy时将其复制到.deploy文件夹中。Github的版本库通常建议同时附上README.md说明文件，但是hexo默认情况下会把所有md文件解析成html文件，所以即使你在线生成了 README. md，它也会在你下一次部署时被删去。怎么解决呢？

在Hexo目录下的source根目录下添加一个README.md。修改Hexo目录下的_config.yml。将skip_render参数的值设置上：
```bash
skip_render:
 - README.md
```
保存退出即可。使用hexo d 命令就不会在渲染 README.md 这个文件了。