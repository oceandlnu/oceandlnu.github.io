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

### 压缩包安装

首先，到官网下载压缩包 https://www.sublimetext.com/3

将下载的压缩包解压到/opt/目录下，将目录名改为sublime_text

### 在线安装

安装说明 http://www.sublimetext.com/docs/3/linux_repositories.html

```
#安装GPG密钥
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -

#确保apt被设置为与https源一起工作
sudo apt-get install apt-transport-https

#选择要安装的版本：
#稳定版
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
#开发版
echo "deb https://download.sublimetext.com/ apt/dev/" | sudo tee /etc/apt/sources.list.d/sublime-text.list

#更新apt源并安装Sublime Text
sudo apt-get update
sudo apt-get install sublime-text
```

接着，写入注册码。

![](/uploads/2017-01-18/2.png)

![](/uploads/2017-01-18/3.png)

点击help>About Sublime Text显示如下信息，注册成功

![](/uploads/2017-01-18/1.png)

### 注册码：

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