---
layout: post
title: PHP解析post请求发送的json数据
date: 2018-04-12 17:56:08
tags:
 - Linux
 - PHP
 - json
categories:
 - PHP
description: PHP解析post请求发送的json数据
copyright: true
---

> * 前端请求json数据

```javascript
{
  cat_id:6,
  "live_cat":"is_hot"
}
```

> * 后台解析json数据转换为数组

```PHP
<?php
function receive_json_to_array()
{
    $json = file_get_contents('php://input');
    //加true转换为数组，不加转换为对象
    $arr =  json_decode($json, true);
    return $arr;
}
$arr = receive_json_to_array();
echo $arr['cat_id'];  //输出 6
echo $arr['live_cat'];  //输出 is_hot
```