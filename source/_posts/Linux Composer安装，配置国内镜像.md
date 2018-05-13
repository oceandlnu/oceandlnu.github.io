---
layout: post
title: Linux Composer安装，配置国内镜像
date: 2018-03-07 17:56:08
tags:
 - Linux
 - PHP
 - Composer
categories:
 - Composer
description: Linux Composer安装，配置国内镜像
copyright: true
---

> * 安装 Composer

```
# wget https://dl.laravel-china.org/composer.phar -O /usr/local/bin/composer
# chmod a+x /usr/local/bin/composer
```

> * Composer 镜像加速


> * 一、全局配置

```
//推荐
# composer config -g repo.packagist composer https://packagist.laravel-china.org
//备用
# composer config -g repo.packagist composer https://packagist.phpcomposer.com
```

> * 二、局部配置(仅限当前工程使用镜像，去掉 -g 即可)

```
//推荐
composer config repo.packagist composer https://packagist.laravel-china.org
//备用
composer config repo.packagist composer https://packagist.phpcomposer.com
```
> * 全局配置信息(查看[repositories.packagist.org.url]表示当前镜像地址)

```
composer config -gl
```

> * 取消镜像

```
composer config -g --unset repos.packagist
```

> * 查看当前版本

```
composer -v
```

> * 升级版本

```
composer selfupdate
```