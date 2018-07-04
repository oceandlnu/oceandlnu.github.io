---
layout: post
title: docker & docker-compose 部署
date: 2018-06-29 10:26:23
tags:
 - linux
 - docker
categories:
 - 配置
description: docker & docker-compose 部署
copyright: true
---

### docker 安装

进入 [下载地址](https://download.docker.com/linux/static/stable/x86_64/) 选择最新的包下载

```
tar xzvf docker-18.03.1-ce.tgz
sudo cp docker/* /usr/bin/
```

> 启动

    sudo dockerd &

> 验证是否安装成功

    docker -v
    docker info

> 为当前用户增加执行权限

    sudo groupadd docker
    sudo usermod -aG docker $USER

### docker-compose 安装


```
#如果shell是zsh，切换到bash安装
bash
sudo curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

> 验证

    docker-compose --version

> 参考资料：
> https://docs.docker.com/install/linux/docker-ce/binaries/
> https://docs.docker.com/install/linux/linux-postinstall/
> https://docs.docker.com/compose/install/#install-compose
> https://github.com/yeasy/docker_practice/blob/master/SUMMARY.md