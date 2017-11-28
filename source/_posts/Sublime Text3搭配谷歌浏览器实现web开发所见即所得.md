---
layout: post
title: Sublime Text3搭配谷歌浏览器实现web开发所见即所得
date: 2016-10-03 21:26:56
tags:
 - web
 - sublime
categories:
 - web
description: Sublime Text3搭配谷歌浏览器实现web开发所见即所得
copyright: true
---
1.选择LiveReload for Sublime text3.zip，将解压的文件夹放在Packages文件夹（Preference>Browse Packags）重启ST3。
2.Preference>Package Settings>LiveReload>Settings User

```
{
“enabled_plugins”: [
“SimpleReloadPlugin”,
“SimpleRefresh”
]
}
```

3.快捷键推荐设置 Preference>Keymap User

```
{
//复制当前行
	"keys": ["ctrl+d"],
	"command": "duplicate_line" 
},{
//格式化代码
	"keys": ["ctrl+alt+/"],
	"command":"reindent",
	"args":{"single_line": false}
},
```

4.将 chromein.com_ext_11631.crx 托放到谷歌浏览器扩展程序处安装，谷歌浏览器右上角刷新按钮点击变为实心即和ST3同步。

下载工具包：[LiveReload for Sublime text3.tar](/uploads/2016-10-03/LiveReload-for-Sublime-text3.tar.gz)