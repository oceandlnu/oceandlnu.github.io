---
layout: post
title: PHPStorm 在 laradock 下进行 Xdebug 断点调试
date: 2018-06-26 10:26:23
tags:
 - linux
 - docker
categories:
 - Xdebug
description: PHPStorm 在 laradock 下进行 Xdebug 断点调试
copyright: true
---

### `laradock` 配置

> 编辑 `laradock/.env` 文件

```
WORKSPACE_INSTALL_XDEBUG=true
PHP_FPM_INSTALL_XDEBUG=true
```

> 重新构建容器

```bash
docker-compose build workspace php-fpm
# 启动
docker-compose up -d nginx mysql redis
```

### PHPStorm 配置

> 打开 `PHPStorm`， `File -> Settings` 进入 `Languages & Frameworks -> PHP -> Servers`。新建一个 `Servers`，如下图

```bash
Name 填写 laradock，必须和 laradock/.env 文件里面 serverName 一致，如果你修改了，改为你设置的 serverName
注意目录映射别忘记设置，主机代码对应 server 的映射位置(/var/www/xxx)
### Remote Interpreter ####################################

# Choose a Remote Interpreter entry matching name. Default is `laradock`
PHP_IDE_CONFIG=serverName=laradock
```

![](/uploads/2018-06-26/1.png)

> 设置断点，点击电话按钮启动监听就可以进行断点调试了

![](/uploads/2018-06-26/2.png)

![](/uploads/2018-06-26/3.png)