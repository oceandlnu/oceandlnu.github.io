---
layout: post
title: Sublime Text 3 安装+注册码
date: 2017-01-18 13:47:23
tags:
 - sublime
categories:
 - 配置
description: Sublime Text 3 安装+注册码
copyright: true
---

首先，到官网下载且安装，博主是解压版的 https://www.sublimetext.com/3

接着，写入注册码。

![](/uploads/2017-01-18/2.png)

![](/uploads/2017-01-18/3.png)

点击help>About Sublime Text显示如下信息，注册成功

![](/uploads/2017-01-18/1.png)

### 注册码：

Sublime Text 3 3126
```
—– BEGIN LICENSE —–
Michael Barnes
Single User License
EA7E-821385
8A353C41 872A0D5C DF9B2950 AFF6F667
C458EA6D 8EA3C286 98D1D650 131A97AB
AA919AEC EF20E143 B361B1E7 4C8B7F04
B085E65E 2F5F5360 8489D422 FB8FC1AA
93F6323C FD7F7544 3F39C318 D95E6480
FCCC7561 8A4A1741 68FA4223 ADCEDE07
200C25BE DBBC4855 C4CFB774 C5EC138C
0FEC1CEF D9DCECEC D3A5DAD1 01316C36
—— END LICENSE ——
```
Sublime Text 3 3143
```
—– BEGIN LICENSE —–
TwitterInc
200 User License
EA7E-890007
1D77F72E 390CDD93 4DCBA022 FAF60790
61AA12C0 A37081C5 D0316412 4584D136
94D7F7D4 95BC8C1C 527DA828 560BB037
D1EDDD8C AE7B379F 50C9D69D B35179EF
2FE898C4 8E4277A8 555CE714 E1FB0E43
D5D52613 C3D12E98 BC49967F 7652EED2
9D2D2E61 67610860 6D338B72 5CF95C69
E36B85CC 84991F19 7575D828 470A92AB
—— END LICENSE ——
```

虽然vscode也还行但是sublime也还是不错的

### ubuntu中文支持

[zh_cn.gz](/uploads/2017-01-18/zh_cn.gz)

使用方法

下载后,用

    tar -xf zh_cn.gz 

会得到一个 sublime的文件夹,然后进入该文件夹内运行命令:

    sudo sh install.sh

即可完美支持中文

### 如何解决Sublime Text 3不能正确显示中文的问题

步骤：

在Sublime Text里，按 ctrl+` 或者 View > Show Console ，打开 Console 一次性输入如下代码：

```
import urllib.request,os,hashlib; h = '6f4c264a24d933ce70df5dedcf1dcaee' + 'ebe013ee18cced0ef93d5f746d80ef60'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

这样Sublime Text就会安装我们需要的Package Control。否则后面会找不到Package。
重启Sublime Text。
在Sublime Text中，按Ctrl+Shift+P打开命令行模式，输入Install Package关键字，然后点击自动出现的下拉菜单里的第一项：Package Control: Install Package。
此时你会看到左下角有个=号来回动，稍等一会，会再次在命令行下弹出一个下拉菜单。输入“ConvertToUTF8”或者“GBK Encoding Support”，选择匹配项。中文字符就可以正常显示了。

+ [Installation - Package Control](https://packagecontrol.io/installation)