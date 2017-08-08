---
layout: post
title: Wamp集成开发环境常见问题（windows）
date: 2016-09-10 21:26:56
tags:
 - Apache
 - MySQL
 - PHP
categories:
 - Wamp
description: Wamp集成开发环境常见问题（windows）
copyright: true
---
1.wamp打不开localhost下的文件

打开www目录下的index.php文件，搜索if (is_dir($file) && !in_array($file,$projectsListIgnore))判断逻辑里面的a标签就是目录，在http://
后面加上localhost/即可：

```PHP
if (is_dir($file) && !in_array($file,$projectsListIgnore))
	{
		$projectContents .= '<li><a href="';
		if($suppress_localhost)
			$projectContents .= 'http://'.$file.$UrlPort.'/"';
		else
			$projectContents .= 'http://localhost'.$UrlPort.'/'.$file.'/"';
		$projectContents .= '>'.$file.'</a></li>';
	}
```

将第5行改成这样

```PHP
if (is_dir($file) && !in_array($file,$projectsListIgnore))
	{
		$projectContents .= '<li><a href="';
		if($suppress_localhost)
			$projectContents .= 'http://localhost/'.$file.$UrlPort.'/"';
		else
			$projectContents .= 'http://localhost'.$UrlPort.'/'.$file.'/"';
		$projectContents .= '>'.$file.'</a></li>';
	}
```

2.设置wamp->localhost的默认打开浏览器

现象：wamp托盘里面的菜单如：localhost、phpmyadmin点击默认用IE浏览器打开，想改成firefox打开怎么改？
解析：安装时有1步是选择默认浏览器的,默认的是选用ie浏览器
解决方法：打开wamp安装目录下的wampmanager.conf文件修改其中的navigator=”E:\ProgramFiles\MozillaFirefox\firefox.exe”这是我所设置的火狐浏览器，你可以更改你所需要的浏览器。如果没有就增加这句。
再在wampmanager.ini文件下修改[Menu.Left]

```
Type:separator;Caption:”WAMP5”
Type:item;Caption:”Localhost”;Action:run;FileName:”E:\ProgramFiles\MozillaFirefox\firefox.exe”;
Parameters:”http://localhost/";Glyph:5
Type:item;Caption:”phpMyAdmin”;Action:run;FileName:”E:\ProgramFiles\MozillaFirefox\firefox.exe”;
Parameters:”http://localhost/phpmyadmin/";Glyph:5
Type:item;Caption:”SQLiteManager”;Action:run;FileName:”E:\ProgramFiles\MozillaFirefox\firefox.exe”;
Parameters:”http://localhost/sqlitemanager/";Glyph:5
```

小技巧：直接在文件下查找“C:\ProgramFiles\InternetExplorer\iexplore.exe”批量替换成你的浏览器安装路径就可以！