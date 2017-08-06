---
layout: post
title: Hexo的常用操作
date: 2016-08-05 21:26:56
tags:
 - Hexo
 - Git
categories:
 - 基本操作
description: Hexo的常用操作
---

新建一个网站

  //新建一个网站。如果没有设置 folder ，$ hexo 默认在目前的文件夹建立网站。
  $ hexo init [foledr]

发表一篇文章

  //新建一篇文章。如果没有设置 layout 的话，默认使用 _config.yml 中的 default_layout 参数代替。如果标题包含空格的话，请使用引号括起来。
  $ hexo new [layout] "文章标题"

在本地博客文件夹 source_posts 文件夹下看到我们新建的 markdown 文件。

当然，我们也可以手动添加Markdown文件在source->_deploy文件夹下，其效果同样可以媲美$ hexo new

文章编辑好之后，运行生成、部署命令：

```bash
// 清除缓存文件 (db.json) 和已生成的静态文件 (public)。
//在某些情况（尤其是更换主题后），如果发现您对站点的更改无论如何也不生效，您可能需要运行该命令。
$ hexo clean
// 生成新的静态文件（public）
$ hexo generate
或者
$ hexo g
// 开始部署
$ hexo deploy
或者
$ hexo d
```

也可以用下面的命令，是一样的效果，让 $ hexo 在生成完毕后自动部署网站

  $ hexo clean
  $ hexo g -d

文章如何添加多个标签

有两种多标签格式

```bash
tags: [a, b, c]
或
tags:
  - a
  - b
  - c
```

显示部分文章内容

```
//如果在博客文章列表中，不想全文显示，可以增加 <!-- more -->, 后面的内容就不会显示在列表。
<!--more-->
```

添加插件

添加 sitemap 和 feed 插件

切换到你本地的 $ hexo 目录 CIA ，在git窗口，输入以下命令

  npm install $ hexo-generator-feed -save
  npm install $ hexo-generator-sitemap -save

修改 _config.yml，增加以下内容

```bash
# Extensions
Plugins:
- $ hexo-generator-feed
- $ hexo-generator-sitemap
#Feed Atom
feed:
  type: atom
  path: atom.xml
  limit: 20
#sitemap
sitemap:
  path: sitemap.xml
```

再执行以下命令，部署服务端

  $ hexo g -d

配完之后，就可以访问 https://oceandlnu.github.io/atom.xml 和 https://oceandlnu.github.io/sitemap.xml ，发现这两个文件已经成功生成了。

添加 404 页面

GitHub Pages 自定义404页面非常容易，直接在根目录下创建自己的404.html就可以。但是自定义404页面仅对绑定顶级域名的项目才起作用，GitHub默认分配的二级域名是不起作用的，使用$ hexo server在本机调试也是不起作用的。

其实，404页面可以做更多有意义的事，来做个404公益项目吧。

推荐使用腾讯公益404 http://www.qq.com/404/ ：

```html
<script type="text/javascript"
        src="//qzonestyle.gtimg.cn/qzone/hybrid/app/404/search_children.js"
        charset="utf-8"
        homePageUrl="http://www.lovebxm.com/"
        homePageName="回到我的主页">
</script>
```

复制上面代码，贴粘到目录下新建的404.html即可！

多PC同步管理博客

很多人可能家里一台笔记本，公司一个台式机，想两个同时管理博客，同时达到备份的博客主题、文章、配置的目的。下面就介绍一下用github来备份博客并同步博客。

A电脑备份博客内容到github

配置.gitignore文件。进入博客根目录下的\themes\next文件夹下，找到此文件，用sublime text 打开，在最后增加两行内容/.deploy_git和/public

初始化仓库。

在博客根目录下，在git bash下依次执行git init和git remote add origin seivice，service为远程仓库地址。

同步到远程仓库。

gitbash下依次执行以下命令

```bash
git add . #添加目录下所有文件
git commit -m "更新说明" #提交并添加更新说明
git push -u origin master #推送更新到远程仓库
```

B电脑拉下远程仓库文件

在B电脑上同样先安装好node、git、ssh、hexo，然后建好hexo文件夹，安装好插件，（然后选做：将备份到远程仓库的文件及文件夹删除），然后执行以下命令：

```bash
git init
git remote add origin <server>
git fetch --all
git reset --hard origin/master
发布博客后同步
```

在B电脑发布完博客之后，记得将博客备份同步到远程仓库
执行以下命令：

```bash
git add .
#可以用git master 查看更改内容
git commit -m "更新信息"
git push -u origin master  #以后每次提交可以直接git push
```

平时同步管理

每次想写博客时，先执行：git pull进行同步更新。发布完文章后同样按照上面的 发布博客后同步 同步到远程仓库。

平时常用命令整理

```bash
git pull #同步更新
hexo new post "新建文章" #简写形式 hexo n "新建文章"
hexo clean #清除旧的public文件夹
hexo generate #生成静态文件 简写形式 hexo g
hexo deploy #发布到github上 简写形式 hexo d
git add . #添加更改文件到缓存区
git commit -m "更新说明" #提交到本地仓库
git push -u origin master #推送到远程仓库进行备份
```

中文乱码

在 md 文件中写中文内容，发布出来后为乱码，原因是 md 的编码不对，将 md 文件另存为UTF-8编码的文件即可解决问题。

结束语

建站的系统有很多，如：

+ [Hexo + GitHub Pages](https://hexo.io/zh-cn/)
+ [Jekyll + GitHub Pages](http://jekyll.com.cn/)
+ [WordPress + 服务器 + 域名](https://cn.wordpress.org/)
+ [DeDeCMS + 服务器 + 域名](http://www.dedecms.com/)
+ …

使用 Hexo + GitHub Pages 建站，有优点也有缺点：

GitHub Pages 不支持数据库管理，所以你只能做静态页面的博客，不能像其他博客（如 WordPress）那样通过数据库管理自己的博客内容。
但是，GitHub Pages 无需购置服务器，免服务器费的同时还能做负载均衡，github pages有300M免费空间。
个人博客真的有必要用数据库吗？答案是否定的。博客静态化，评论记录使用第三方的 网易云跟帖就可以了。静态的博客更有利于搜索引擎蜘蛛爬取，轻量化的感觉真的很好。
通过 Hexo 你可以轻松地使用 Markdown 编写文章，非常符合我的口味。Markdown 真的是专门针对程序员开发的语言啊，现在感觉没有 Markdown什么都不想写。什么富文本编辑器，什么word，太麻烦了！而且样式都好丑！效率太低！
推荐几个很好用的在线 Markdown 编辑器：

作业部落：https://www.zybuluo.com/mdeditor
马克飞象：https://maxiang.io
推荐图床：

极简图床 + chrome 插件 + 七牛空间，七牛云储存提供10G的免费空间,以及每月10G的流量，存放个人博客外链图片最好不过了，七牛云储存还有各种图形处理功能、缩略图、视频存放速度也给力。