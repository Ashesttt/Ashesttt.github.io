---
layout: '../../layouts/MarkdownPost.astro'
title: 'node和nginx部署网站的初步认识'
pubDate: 2023-03-29
description: 'node和nginx部署网站的初步认识'
author: 'hjh'
cover:
    url: 'https://cdnjson.com/images/2023/04/02/v2-ac85dfe7ff45d09896dc6078dc51971f_r.jpg'
    square: 'https://cdnjson.com/images/2023/04/02/v2-ac85dfe7ff45d09896dc6078dc51971f_r.jpg'
    alt: 'cover'
tags: ["Node.js", "Nginx", "Linux"]  
theme: 'dark'   
featured: true
---

# node和nginx部署网站的初步认识

## nginx开启和关闭

开启：nginx

关闭：nginx -s stop

## nginx重新加载和重新启动

重新加载：**nginx -s reload**

重新启动：`nginx -s reopen`

## linxu查看端口占用：netstat -tunlp | grep 端口或者软件名称


nginx在本地端口888，443，80端口，如图：

![nginx在本地端口888，443，80端口](https://s2.loli.net/2023/04/03/fnY5sPITlQtm9aK.png)

## node.js把网站部署到本地端口3000

不知道为什么在外网访问的时候，会显示在外网的443端口  82.157.238.98:443

应该是因为nginx的设置问题

因为把nginx暂停后，外网直接不可以访问，没有页面

但是把nginx开启后，外网就可以访问，有页面，而且在

## node在网站部署在127.0.0.1:3000,把nginx启动了就可以访问网站

## nginx反向代理：

```c
server {
    listen 82;
    server_name localhost;
    location / {
        proxy_pass http://192.168.200.201:8080;     #反向代理配置，将请求转发到指定服务
    }
}
 
//上述配置的含义为: 当我们访问nginx的82端口时，根据反向代理配置，
会将请求转发到 http://192.168.200.201:8080 对应的服务上。
```



[用node和nginx](https://www.notion.so/node-nginx-df015bac2bcb45fdb9798329bbc9f7b6)

[部署网站](https://www.notion.so/node-nginx-df015bac2bcb45fdb9798329bbc9f7b6)
