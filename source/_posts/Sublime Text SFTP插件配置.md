---
layout: post
title: Sublime Text SFTP插件配置
date: 2017-02-18 17:56:08
tags:
 - Linux
 - sublime
categories:
 - sublime
description: Sublime Text SFTP插件配置
copyright: true
---

>* SFTP 插件。主要功能就是通过 FTP/SFTP 连接远程服务器并获取文件列表，可以选择下载编辑、重命名、删除等等操作，点下载编辑之后，可以打开这个文件进行修改。修改完成之后，保存一下会自动上传到远程的服务器上面，使用这个插件之后，工作效率可以大大提高，下面就来记录一下具体的配置方法。

```
{
    // The tab key will cycle through the settings when first created
    // Visit http://wbond.net/sublime_packages/sftp/settings for help
    
    // sftp, ftp or ftps
    "type": "sftp",

    "save_before_upload": true,
    // 保存后自动上传
    "upload_on_save": true,
    // 开启时同步远端到本地
    "sync_down_on_open": false,
    // 同步时跳过删除的文件
    "sync_skip_deletes": false,
    "sync_same_age": true,
    //开启「下载确认」
    "confirm_downloads": false,
    //开启「同步确认」
    "confirm_sync": true,
    //开启「覆盖确认」
    "confirm_overwrite_newer": false,
    
    "host": "ip",
    "user": "username",
    "password": "password",
    //"port": "22",
    
    // 远程服务器文件夹路径
    "remote_path": "/web/alexa-master/",
    "ignore_regexes": [
        "\\.sublime-(project|workspace)", "sftp-config(-alt\\d?)?\\.json",
        "sftp-settings\\.json", "/venv/", "\\.svn/", "\\.hg/", "\\.git/",
        "\\.bzr", "_darcs", "CVS", "\\.DS_Store", "Thumbs\\.db", "desktop\\.ini"
    ],
    //"file_permissions": "664",
    //"dir_permissions": "775",
    
    // 开启设置并发连接数为10，提高同步速度
    "extra_list_connections": 10,

    "connect_timeout": 30,
    //"keepalive": 120,
    //"ftp_passive_mode": true,
    //"ftp_obey_passive_host": false,
    //"ssh_key_file": "~/.ssh/id_rsa",
    //"sftp_flags": ["-F", "/path/to/ssh_config"],
    
    //"preserve_modification_times": false,
    //"remote_time_offset_in_hours": 0,
    //"remote_encoding": "utf-8",
    //"remote_locale": "C",
    //"allow_config_upload": false,
}
```