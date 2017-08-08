---
layout: post
title: Git的简单使用
date: 2016-06-22 21:26:56
tags:
 - GitHub
 - Git
categories:
 - 基本操作
description: Git的简单使用
copyright: true
---
第一步 下载[Git](https://git-scm.com/)
1. 在官网点击Download，下载对应的exe文件，注意你的操作系统是32位还是64位。
2. 双击安装，中间不用做任何改动，一直下一步就行。如果你想修改安装位置，请放在纯英文路径下。
3. 安装成功，你现在就可以使用git命令行工具了。在你想要下载代码的路径，点击鼠标右键，选择Git Bash here。注意，你的代码路径也应是纯英文的。
4. Git Bash使用的是MinGW，其界面如下图所示：

![](/uploads/2016-06-22/1.png)

第二步 创建一个本地hello-world仓库
1. 在命令行输入 mkdir hello-word，创建一个新文件夹。你可以使用ls命令来查看当前目录下有哪些文件和文件夹。
2. 输入cd hello-world进入新文件夹，注意在输入命令时，你可以用Tab键来自动补全。
3. 输入git init初始化Git仓库。此时用ls -a查看当前目录，可以看到多了一个.git/的文件夹，此文件夹保存了版本控制的所有相关信息。

![](/uploads/2016-06-22/2.png)

>注意，在此处使用的所有命令，如果你是在linux环境下开发，用法都是完全一样的。所以对于完全没有Linux使用经验的学员，这也是一个开始接触Linux工作方式的好方法。

接下来，让我们创建一份简单的说明文件，并提交到版本库中。

4. 输入echo "This is a simple practise" > readme.txt，创建一个readme.txt文件。
5. 输入git status查看当前版本库状态，在Untracked files(未跟踪文件)下，会出现红色的readme.txt，代表此文件还未被Git所管理。

![](/uploads/2016-06-22/3.png)

6. 使用git add readme.txt，将该文件加入缓冲区，如果你确定所有的修改都需要提交，可以使用git add .来加入所有修改。现在用git status查看，将看到文件名变为绿色。
7. 使用git commit -m "This is my first commit via Git!"来提交修改，-m后面所带的参数是本次提交信息，一般用来记录本次提交的主要意图。

![](/uploads/2016-06-22/4.png)

8. 提交成功后，可以用git log查看历史提交记录。每个记录都会有提交id，作者和提交日期。
9. 你可以用git branch查看当前有哪些分支，当然，因为我们没有创建任何分支，目前只会有一个master分支。
10. 使用git checkout -b feature创建一个名为feature的分支，再用git branch查看一下。
```bash
$ git checkout -b iss53
# 它是下面两条命令的简写：
$ git branch iss53
$ git checkout iss53
```

![](/uploads/2016-06-22/5.png)

以上是最最基本的Git操作，大家可以在此hello-world项目中随意尝试各种其他git命令，最好的参考资料是[Pro Git book](https://git-scm.com/book/zh/v2)。

>注意：学会Git的唯一方式是在实际使用中学习，切记不要尝试先记住一大堆理论知识或者Git命令。

项目的下载，查看和修改
第一步. 从GitHub上下载我们的项目代码。
1. 以Hello-World项目为例，点击绿色按钮Clone or download，然后在弹出窗口中点击剪切板图标，复制仓库的URL。

![](/uploads/2016-06-22/6.png)

2. 在git bash中输入git clone git@github.com:oceandlnu/hello-world.git，下载项目源码。
第二步. 查看版本历史
1. cd到项目文件夹下，使用git log能看到我们的历史提交记录。
2. 要回到某一历史版本，可以使用git checkout commitId，看完后要回到最新代码，使用git checkout master。
第三步. 本地修改代码
你可以在我们的代码基线上任意修改，但为了下载新代码时不出现冲突，请遵循以下步骤：

1. 下载新代码：git pull。
2. 从master出捡出一个新的分支：git checkout -b feature。feature是分支名称，你可以随意取名，但请用英文。
3. 在feature分支上随意修改，改完后你可以提交你的修改：git commit -m "some message"。
4. 此时要同步代码，请先切回主分支：git checkout master，然后更新git pull。
5. 如果想删除自己建立的分支，使用git branch -D feature，注意执行此命令后分支被强制删除，无法恢复。