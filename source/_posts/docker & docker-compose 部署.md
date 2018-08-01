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

```c
tar xzvf docker-18.03.1-ce.tgz
sudo cp docker/* /usr/bin/
```

> 启动

```c
sudo dockerd &
```

> 验证是否安装成功

```sh
docker -v
docker info
```
> 为当前用户增加执行权限

```sh
sudo groupadd docker
sudo usermod -aG docker $USER
```

### docker-compose 安装


```bash
#如果shell是zsh，切换到bash安装
bash
sudo curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

### 替换国内源

> 编辑 `sudo nano /etc/docker/daemon.json` 如果不存在则创建。如果文件为空，添加以下内容。

[参考链接](http://guide.daocloud.io/dcs/daocloud-9153151.html)

```json
{
    "registry-mirrors": [
        "http://0ed8bcb8.m.daocloud.io"
    ],
    "insecure-registries": []
}
```


> 验证

```bash
docker-compose --version
```

### 基本命令

> 停止所有正在运行的容器

```bash
docker-compose stop
```

> 删除所有已停止运行的容器

```bash
docker-compose down
docker container prune
```

> 删除所有镜像

```bash
docker rmi $(docker images -q)
docker image rm $(docker image ls -q)
```

> 进入 `workspace` 容器

```bash
# 以laradock身份登录 workspace 容器，如果省略 --user=laradock，则以root身份登录
docker-compose exec --user=laradock workspace bash
```

> 进入 `mysql` 容器连接 `mysql`

```bash
docker-compose exec mysql mysql -uroot -proot
```

> 进入 `redis` 容器连接 `redis`，默认端口为 `6379`，如果你更改了端口请在后面加上 `-p [端口号]`

```bash
docker-compose exec redis redis-cli -h redis
```

+ [laradock 设置代理](https://github.com/laradock/laradock/issues/1315#issuecomment-380492758)

+ 参考资料：
+ https://docs.docker.com/install/linux/docker-ce/binaries/
+ https://docs.docker.com/install/linux/linux-postinstall/
+ https://docs.docker.com/compose/install/#install-compose
+ https://github.com/yeasy/docker_practice/blob/master/SUMMARY.md