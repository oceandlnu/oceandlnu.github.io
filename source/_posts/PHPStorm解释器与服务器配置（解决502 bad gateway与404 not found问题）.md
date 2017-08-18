---
layout: post
title: PHPStorm解释器与服务器配置（解决502 bad gateway与404 not found问题）
date: 2017-07-18 13:47:23
tags:
 - PHP
 - IDE
 - PHPStorm
categories:
 - 配置
description: PHPStorm解释器与服务器配置（解决502 bad gateway与404 not found问题）
copyright: true
password: 
---

phpstorm是一个非常强大的全栈开发工具，但是作为刚入手的我发现它并不是安装之后就可以正常使用的，还需要相关的配置，否则会出现网页打开错误。下面记录我在使用中遇到的一些问题与解决方法。
首先，在phpstorm中是直接可以运行PHP程序而不需要手动启动apache服务器，这为我编写与调试代码提供了很大便捷，不需要每次手动启动wampware相关环境。前提是需要配置php解释器，如果没有配置，在运行时会在右下角弹出提示，需要配置解释器interpreter。也可以自己手动配置：File>Settings>Languages&Frameworks>PHP目录下打开配置界面，右面绿色的“+”按钮，添加你的php程序路径，并选择相关CLI interpreter，点击ok配置完成

![](/uploads/2017-07-18/4.png)

![](/uploads/2017-07-18/5.png)

![](/uploads/2017-07-18/6.png)


__但是，要注意运行的php文件需要放在apache的网站根目录下，如果运行不在该目录下的文件就会显示502 bad gateway。__

其次在运行相关表单提交或者php页面跳转时会提示404 not found，即找不到服务器。这是因为phpstorm的页面默认在localhost：63342端口下运行，而我们的apache服务器一般默认为80端口，所以在提交表单到服务器时它会找不到相关php程序，尽管你的路径是正确的，因此需要配置phpstorm的服务器环境：
在file->settings->build,excution,deployment->Deployment页面栏下选择左上角绿色的“+”按钮新建，起个名字，type选择inplace（本地调试的意思），然后设置web sever root url为：http://localhost ，在mappings标签页下填写localpath，即你的apache网站根目录：

![](/uploads/2017-07-18/1.png)

![](/uploads/2017-07-18/2.png)

点击ok配置完成，这样你点击运行后页面就是在80端口下运行相关了，这时候提交或者跳转就不会显示404not found了。

进行上面的配置后，发现在进行表单传值学习的时候，post不能得到值而get可行，最后发现是因为phpstorm打开的页面默认是localhost:63342/......，从而使用post得不到值。解决方法，新建一个Nginx的项，只是设置了下面的root URL，再在phpstorm中打开浏览器时，就可以了。

![](/uploads/2017-07-18/3.png)
