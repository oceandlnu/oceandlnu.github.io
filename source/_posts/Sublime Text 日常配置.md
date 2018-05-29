---
layout: post
title: Sublime Text 日常配置
date: 2017-01-18 13:47:23
tags:
 - sublime
categories:
 - 配置
description: Sublime Text 日常配置
copyright: true
---

### 安装

官网下载压缩包 https://www.sublimetext.com/3

解压缩，将目录名改为sublime_text，移动到 /opt/ 目录下

	sudo mv sublime_text /opt/

### 注册码：

```
--- BEGIN LICENSE -----
ZYNGA INC.
50 User License
EA7E-811825
927BA117 84C9300F 4A0CCBC4 34A56B44
985E4562 59F2B63B CCCFF92F 0E646B83
0FD6487D 1507AE29 9CC4F9F5 0A6F32E3
0343D868 C18E2CD5 27641A71 25475648
309705B3 E468DDC4 1B766A18 7952D28C
E627DDBA 960A2153 69A2D98A C87C0607
45DC6049 8C04EC29 D18DFA40 442C680B
1342224D 44D90641 33A3B9F2 46AADB8F
------ END LICENSE ------
```

### 如何禁用自动更新：

```
“Preferences -> Settings-User/Distraction Free“
#增加一行
“update_check”: false,
```

__如果上面方法无效，修改hosts文件，添加下面几行：__

| OS | path |
| :--- | :-- |
|Windows | C:\Windows\System32\drivers\etc\hosts |
|Mac | /Private/etc/hosts |
|Linux | /etc/hosts |

```
127.0.0.1 www.sublimetext.com
127.0.0.1 license.sublimehq.com
127.0.0.1 45.55.255.55
127.0.0.1 45.55.41.223
```

### linux不能输入中文解决方法

下载 [zh_cn.tar.gz](/uploads/2017-01-18/zh_cn.tar.gz)

使用方法

下载后，执行

    tar -zxvf zh_cn.tar.gz 
	cd sublime/
    sudo ./install.sh

即可完美支持中文

### 导入插件

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

### 安装汉化插件

打开Sublime Text，按 ctrl + \` 或者 View > Show Console ，打开 Console 一次性输入如下代码，回车：

```
import urllib.request,os,hashlib; h = '6f4c264a24d933ce70df5dedcf1dcaee' + 'ebe013ee18cced0ef93d5f746d80ef60'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

接着Ctrl+Shift+P打开命令行模式，输入pci，然后回车选择：Package Control: Install Package。

稍等一会，在弹出的下拉菜单输入"ChineseLocalization"。

### Sublime Text 3中文显示乱码

安装"ConvertToUTF8" 或者 "ConvertChineseCharacters" 或者 "GBK Support"，中文字符就可以正常显示了。

__参考资料：__

+ [Installation - Package Control](https://packagecontrol.io/installation)