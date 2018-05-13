---
layout: post
title: VirtualBox CentOS7 Mini 安装增强工具
date: 2018-04-12 17:56:08
tags:
 - Linux
 - VirtualBox
categories:
 - VirtualBox
description: VirtualBox CentOS7 Mini 安装增强工具
copyright: true
---

>* 宿主机：Ubuntu18.04
   虚拟机：CentOS7 Mini
   教程适用系统：CentOS6/CentOS7(亲测)


>* 安装相关依赖

```
# yum install vim gcc kernel kernel-devel bzip2 -y
# reboot
```

>* 点击虚拟机菜单栏 => 设备 => 安装增强功能

```
# mount /dev/cdrom /mnt
# cd /mnt
# ./VBoxLinuxAdditions.run
```

>* 打开 VirtualBox 界面，选择对应的虚拟系统进行"设置"，选中设置窗口中的最后一项"共享文件夹"，再选中"固定分配"，右键单击并确定共享文件夹的路径，下面的复选框一个都不用勾选，最后"确定"。启动虚拟系统，进入系统以后，执行以下命令来挂载共享文件夹：mount -t vboxsf code /web，其中 code 为共享文件夹的名字，/web 表示当前挂载到 web 目录下，如果没有web目录请自行创建 mkdir /web。如果需要取消挂载，可以直接使用命令：umount -f /web。


>* 实现开机自动挂载

1.适用于CentOS7/6

```
# vim ~/.bashrc
// 在最后添加一行
# mount -t vboxsf code /web
```
2.只适用于CentOS6

```
# vim /etc/rc.d/rc.local
// 在文件的最后面加上挂载命令
# mount -t vboxsf code /web
```

### VirtualBox Win7虚拟机无法识别U盘问题解决方法

1.首先需要一个USB用户组，可以用vboxusers这个在安装VirtualBox的时候产生的用户组，把你使用的这个用户加到vboxusers组中，确保该用户是否有权限去读写usbfs这个文件系统

```
$ cat /etc/group | grep vboxusers
vboxusers:x:127:
$ whoami
ocean
```

2.把ocean用户加到vboxusers组中，后面的ocean就是你自己的用户名

```
$ sudo adduser ocean vboxusers
```

3.重启电脑，启动Virtualbox，确认在virtualbox管理器中，USB设备的enable usb controler 、enable usb2.0 controler打勾。Win7启动后，右击右下角USB设备符号，在Setting中，选择USB Massage相关的选项，就会自动安装驱动，驱动安装成功后，win7下成功挂载U盘。

__注意：U盘插入宿主机usb接口必须是usb2.0，如果为3.0虚拟机无法识别__