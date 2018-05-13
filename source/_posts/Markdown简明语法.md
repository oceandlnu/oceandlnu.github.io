---
layout: post
title: Markdown简明语法
date: 2016-07-05 21:26:56
tags:
 - Markdown
 - Git
categories:
 - 基本操作
description: Markdown简明语法
copyright: true
---
Markdown是一种极简的『标记语言』，将文本转为HTML，通常为我大码农所用。其不追求大而全，简洁至上，正所谓不求最贵，只求最好！

本文介绍Markdown基本语法，内容很少，一行语法一行示例，学会后可轻松写出高大上的文档，再也不需要各种编辑器去调文章格式。另外，网上有各平台下的Markdown工具可用，也有在线的，我直接使用sublime搞定，Markdown本来就是为了追求简洁，弄个工具岂不多此一举。

强调

星号与下划线都可以，单是斜体，双是粗体，符号可跨行，符号可加空格

```bash
**粗体**
__粗体__
*斜体*
_斜体_
```
**粗体**
__粗体__
*斜体*
_斜体_

分割线

三个或更多\-\_\*，必须单独一行，可含空格

	---
引用

	翻译成html就是<blockquote></blockquote>符号后的空格可不要
	> 引用

>引用

```bash
内层符号前的空格必须要
>引用
 >>引用中的引用
```

>引用
 >>引用中的引用

标题：Setext方式

```bash
三个或更多
大标题
===
小标题
---
```
大标题
===
小标题
---

标题：Atx方式

```bash
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题

无序列表

```md
符号之后的空格不能少，-+*效果一样，但不能混合使用，因混合是嵌套列表，内容可超长
- 无序列表
- 无序列表
- 无序列表
```

- 无序列表
- 无序列表
- 无序列表

```md
符号之后的空格不能少，-+*效果一样，但不能混合使用，因混合是嵌套列表
+ 无序列表
+ 无序列表
+ 无序列表
```

+ 无序列表
+ 无序列表
+ 无序列表

```bash
符号之后的空格不能少，-+*效果一样，但不能混合使用，因混合是嵌套列表
* 无序列表
* 无序列表
* 无序列表
```

* 无序列表
* 无序列表
* 无序列表

```bash
数字不能省略但可无序，点号之后的空格不能少
1. 有序列表
2. 有序列表
3. 有序列表
8. 有序列表
```

1. 有序列表
2. 有序列表
3. 有序列表
8. 有序列表

```bash
-+*可循环使用，但符号之后的空格不能少，符号之前的空格也不能少
- 嵌套列表
 + 嵌套列表
 + 嵌套列表
  - 嵌套列表
   * 嵌套列表
- 嵌套列表
```

- 嵌套列表
 + 嵌套列表
 + 嵌套列表
  - 嵌套列表
   * 嵌套列表
- 嵌套列表

文字超链：Inline方式

```bash
Tooltips可省略
[Ocean's](https://oceandlnu.github.io/ "Ocean's Blog")
```
[Ocean's](https://oceandlnu.github.io/ "Ocean's Blog")

图片超链

```bash
多个感叹号，Tooltips可省略，要设置大小只能借助HTML标记
![GitHub Mark](http://github.global.ssl.fastly.net/images/modules/logos_page/GitHub-Mark.png "GitHub Mark")
```
![GitHub Mark](http://github.global.ssl.fastly.net/images/modules/logos_page/GitHub-Mark.png "GitHub Mark")


索引超链：Reference方式

```
索引，1 2可以是任意字符
[Ocean's Blog][1]
![GitHub Octocat][2]
[1]:https://oceandlnu.github.io/
[2]:http://github.global.ssl.fastly.net/images/modules/logos_page/Octocat.png
```
[Ocean's Blog][1]
![GitHub Octocat][2]
[1]:https://oceandlnu.github.io/
[2]:http://github.global.ssl.fastly.net/images/modules/logos_page/Octocat.png

自动链接

```bash
尖括号
<https://oceandlnu.github.io/>
https://oceandlnu.github.io/
```

代码：行内代码

```
每行文字前加4个空格或者1个Tab
	val s = "hello Markdown";
```
	val s = "hello Markdown"

代码：段落代码

```
内容包含在```bash和```之间，bash换成指定编程语言，也可以省略
val s = "hello Markdown"
println( s )
```

代码：hexo

```
可指定编程语言，代表左右大括号
『% codeblock [title] [lang:language] [url] [link text] %』
  code snippet
『% endcodeblock %』
```

注释
```
<!-- 注释 -->
```
用html的注释，好像只有这样？


转义字符

```
Markdown中的转义字符为\，转义的有：
\\ 反斜杠
\` 反引号
\* 星号
\_ 下划线
\{\} 大括号
\[\] 中括号
\(\) 小括号
\# 井号
\+ 加号
\- 减号
\. 英文句号
\! 感叹号
其它
```

文本中可直接用html标签，但是要前后加上空行。

表格

目前编辑器不支持表格，以往是通过截图，呈现的效果并不好，Markdown支持html，所以我们可以用html来写表格。但是……用html写表格，实在太麻烦了，这里有个简单的转换方法，供大家参考：

举例，假设有这样一个表格，内容如下：

时间 地点 人物
3月5日 北京 姚明
3月7日 上海 韩寒

处理方法如下：

1.从word或excel中复制表格

2.打开此[链接](http://pressbin.com/tools/excel_to_html_table/index.html)

3.贴上复制的文字，然后按convert，就会得到这个表格的代码

4.简要说明表格设定如下：

```
将第一个 <table>变成<table class="table table-bordered table-striped table-condensed">。
为什么要加它们呢？这三个是什么意思呢？解释如下：
它们都会给表格带上某种样式，如果没有的话，比较不好看：
1.table-bordered：带圆角边框和竖线
2.table-striped：奇偶行颜色不同
3.table-condensed：压缩行距
以上三个可以进行不同的组合，如果是很长的表格，建议只用后两个。
另外，如果需要表头跟内容不一样，可以将<td>表头内容</td>换成<th>表头内容</th>。
如果表格内文需要换行，可以在要换行的内容后加入<br>，后面的内容就会跑到下一行。
如果内文中有代码，需要特别显示，可使用：<code>代码</code>。
如果表格中有需要设为斜体的内容，可使用：<I>要设为斜体的内容</I>。
如果有跨行或者跨列的单元格，可用<th colspan="跨列数">内容</th>。
跨行则是用rowspan="跨行数"。
如果要调整某一列的宽度，可使用：<th width="宽度值或百分比">表头内容</th>。
```

如果要调整整个表格的宽度，可以参考berlinix的这篇文章：http://www.ituring.com.cn/article/details/8367

5.把最后得到的代码复制到文中，下面就是结果啦：

<table class="table table-bordered table-striped table-condensed">
   <tr>
      <td>时间 地点 人物</td>
   </tr>
   <tr>
      <td>3月5日 北京 姚明</td>
   </tr>
   <tr>
      <td>3月7日 上海 韩寒</td>
   </tr>
</table>

如果大家对此感兴趣，这里有一个进阶资料：http://twitter.github.com/bootstrap/base-css.html#tables
结束语

以上基本够用，更详尽的请参考文献10，另外Markdown+R可以干大事，请参考文献6。

参考文献

[1.献给写作者的 Markdown 新手指南](http://www.jianshu.com/p/q81RER)
[2.Cmd Markdown 编辑阅读器](https://www.zybuluo.com/mdeditor)
[3.Markdown之表格的处理](http://www.ituring.com.cn/article/3452)
[4.Markdown语法说明（详解版）](http://www.ituring.com.cn/article/504)
[5.怎样使用Markdown](http://www.ituring.com.cn/article/23)
[__6.Markdown写作浅谈__](http://www.jianshu.com/p/PpDNMG)
[7.Markdown语法示例](http://equation85.github.io/blog/markdown-examples/)
[8.HTML转义字符对照表](http://tool.oschina.net/commons?type=2)
[9.Markdown: Basics （快速入门）](http://wowubuntu.com/markdown/basic.html)
[__10.Markdown 语法说明 (简体中文版)__](http://wowubuntu.com/markdown/index.html)