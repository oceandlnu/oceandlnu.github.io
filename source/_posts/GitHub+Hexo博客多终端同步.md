---
layout: post
title: GitHub+Hexo博客多终端同步
date: 2017-04-05 19:23:08
tags:
 - GitHub
 - Git
 - Hexo
categories:
 - 基本操作
description: GitHub+Hexo博客多终端同步
copyright: true
password: 
---

我们发现用Github+Hexo搭建了自己的博客，但是回到宿舍打开电脑时遇到了一个问题，我想在不同的终端进行github+Hexo的博客发布更新该如何进行呢，在Google中搜了一些教程，并自身进行了简化与实践！

主体的思路是__将博文内容相关文件放在Github项目中master中，将Hexo配置写博客用的相关文件放在Github项目的hexo分支上__，这个是关键，多终端的同步只需要对分支hexo进行操作。下面是详细的步骤讲解：

1.准备条件

安装Node,Git,Hexo环境，完成Github与本地Hexo的对接。

配置好这些，就可以捋起袖子大干一场了！

2.在其中一个终端操作，push本地文件夹Hexo中的必要文件到yourname.github.io的hexo分支上

在利用Github+Hexo搭建自己的博客时，新建了一个Hexo的文件夹，并进行相关的配置，这部分主要是将这些配置的文件托管到Github项目的分支上，其中只托管部分用于多终端的同步的文件：

__注意：themes文件夹里面的主题是不会add的，所以可以先将主题文件压缩（如我的是next.zip），然后add，之后同步到本地之后再解压，目前还没有找到别的办法__

```bash
//初始化本地仓库
git init
//将目录下的所有文件添加到本地仓库
git add . 
git commit -m "Blog Source Hexo"
//新建hexo分支
git branch hexo
//切换到hexo分支上
git checkout hexo
//将本地与Github项目对接
git remote add origin git@github.com:oceandlnu/oceandlnu.github.io.git
//push到Github项目的hexo分支上
git push origin hexo
```

完成之后的效果图为：

![](/uploads/2017-04-05/1.png)

这样你的github项目中就会多出一个Hexo分支，这个就是用于多终端同步关键的部分。

3.另一终端完成clone和push更新

此时在另一终端更新博客，只需要将Github的hexo分支clone下来，进行初次的相关配置，详细配置点击[GitHub+Hexo博客多终端同步[准备工作]](https://oceandlnu.github.io/2017/03/06/GitHub+Hexo%E5%8D%9A%E5%AE%A2%E5%A4%9A%E7%BB%88%E7%AB%AF%E5%90%8C%E6%AD%A5[%E5%87%86%E5%A4%87%E5%B7%A5%E4%BD%9C]/)

```bash
//将Github中hexo分支clone到本地
git clone -b hexo git@github.com:oceandlnu/oceandlnu.github.io.git  
//切换到刚刚clone的文件夹内
cd  oceandlnu.github.io
//注意，这里一定要切换到刚刚clone的文件夹内执行，安装必要的所需组件，不用再hexo init
npm install
//新建一个.md文件，并编辑完成自己的博客内容
hexo new post "new blog name"
//经测试每次只要更新source中的文件到Github中即可，因为只是新建了一篇新博客
git add source
git commit -m "XX"
//推送到远程仓库，更新hexo分支
git push origin hexo
```

__注意，这里先将themes文件夹里面的主题压缩包（例如我的是next.zip）解压，不然直接hexo g -d页面为空白__


```
hexo g -d   //push更新完分支之后将自己写的博客对接到自己搭的博客网站上，同时同步了Github中的master
```

4.不同终端间愉快地玩耍

在不同的终端已经做完配置，就可以愉快的分享自己更新的博客，进入自己相应的文件夹

```bash
//先pull完成本地与远端的融合
git pull origin hexo
hexo new post " new blog name"
git add source
git commit -m "XX"
git push origin hexo
```

__注意，这里先将themes文件夹里面的主题压缩包（例如我的是next.zip）解压，不然直接hexo g -d页面为空白__


```
hexo g -d
```

关于Github不熟悉的强烈推荐张哥的Github系列教程[我的书籍出版了](http://stormzhang.com/2017/01/20/learn-github-from-zero-pdf/)

另外你可能会遇到一些其他的坑，在这里我没有遇到，大家可以参考一篇博文[搭建hexo博客并简单的实现多终端同步](https://righere.github.io/2016/10/10/install-hexo/)

对于于博文的图片托管问题感兴趣的可以我写的参考[如何利用Github在Markdown中优雅的插入图片](http://blog.csdn.net/monkey_lzl/article/details/57480599)