---
layout: '../../layouts/MarkdownPost.astro'
title: '前后端分离的ruoyi在如何放在服务器上'
pubDate: 2023-03-29
description: '前后端分离的ruoyi在如何放在服务器上'
author: 'hjh'
cover:
    url: 'https://cdnjson.com/images/2023/04/02/wallhaven-57rmj5_2560x1440.png'
    square: 'https://cdnjson.com/images/2023/04/02/wallhaven-57rmj5_2560x1440.png'
    alt: 'cover'
tags: ["前后端分离", "若依", "服务器", "Node.js", "Nginx", "Maven", "JDK", "MySQL", "Redis"]
theme: 'light'
featured: true
---

# 前后端分离的ruoyi在如何放在服务器上



本文将介绍如何将前后端分离的ruoyi项目部署在服务器上。



## 准备工作

在部署之前，需要先准备以下工作：

- 服务器，可以选择云服务器或者本地服务器。
- 确保服务器已安装JDK环境，可以通过运行`java -version`命令来检查。
- 确保服务器已安装Maven环境，可以通过运行`mvn -v`命令来检查。
- [node.](http://node.is)js 最好v16 不然出现各种问题



# 后端：

1.**用idea 修改配置：mysql、redis**

2.**在服务器的mysql 导入sql；**



- 首先创建和使用 数据库

```bash
create database'ruo-vue';

use ruo-vue;
```



- 然后 导入sql

```bash
source /nigproject/ruoyi/RuoYi-Vue-nigge/RuoYi-Vue-master/sql/quartz.sql

source /nigproject/ruoyi/RuoYi-Vue-nigge/RuoYi-Vue-master/sql/ry_20220822.sql
```

- 收工！



3.**在idea中 然后找到源代码的bin/package.bat 用maven 运行package 打包**



- 打包完会在 以下地址 生成一个jar包 ：

```bash
D:\RuoYi-Vue-nigge\RuoYi-Vue-master\ruoyi-admin\target\ruoyi-admin.jar
```

- 然后用xshell 复制到服务器中的以下地址：

```bash
cd /nigproject/ruoyi/RuoYi-Vue-nigge/RuoYi-Vue-master
```

- 然后运行

```bash
java -jar ruoyi-admin.jar
```

- 后端就运行起来了

![后端就运行起来了](https://s2.loli.net/2023/04/03/yCTBzM5P6emhIA9.jpg)



4.**启动 redis**

```bash
redis-server
```

![启动 redis](https://s2.loli.net/2023/04/03/GNoh3xC4euYvPBT.jpg)



- 检验redis 是否启动了

```bash
redis-cli
```

- 然后进入到redis的 客户端 再输入

```bash
ping
```

- 他就会返回 pong



5.**打包前端ruoyi-ui**

```bash
npm run build:prod
```

- 然后就在该文件夹中 生成打包好的前端 dist 文件

![然后就在该文件夹中 生成打包好的前端 dist 文件](https://s2.loli.net/2023/04/03/sQOMvREHKyIFzVN.jpg)



- 去到nginx 的前端文件夹

```bash
/usr/share/nginx/ruoyiui
```

- 再复制dist里的文件复制到ruoyiui中

```bash
cp -r /nigproject/ruoyi/RuoYi-Vue-nigge/RuoYi-Vue-master/ruoyi-ui/dist/* .
```

![复制dist里的文件复制到ruoyiui中](https://s2.loli.net/2023/04/03/wHLC46MGaDKe9Oy.jpg)



6.**最重要的配置**



### 后端

的两个配置文件：

[application.yml](https://github.com/Ashesttt/RuoYi-Vue-nigge/blob/master/RuoYi-Vue-master/ruoyi-admin/src/main/resources/application.yml)

[application-druid.yml](https://github.com/Ashesttt/RuoYi-Vue-nigge/blob/master/RuoYi-Vue-master/ruoyi-admin/src/main/resources/application-druid.yml)

- 它们分别在本机的：

```jsx
D:/RuoYi-Vue-nigge/RuoYi-Vue-master/ruoyi-admin/src/main/resources/application.yml

D:/RuoYi-Vue-nigge/RuoYi-Vue-master/ruoyi-admin/src/main/resources/application-druid.yml
```

### application.yml主要配置是：

```jsx
# 项目相关配置
# 文件路径 示例( Windows配置D:/ruoyi/uploadPath Linux配置 /home/ruoyi/uploadPath )
  profile: /nigproject/ruoyi/RuoYi-Vue-nigge

# redis 配置
  redis:
    # 地址
    host: 127.0.0.1
```

### application-druid.yml主要配置是：

```jsx
# 主库数据源
            master:
                url: jdbc:mysql://localhost:3306/ry-vue?useUnicode=true&characterEncoding=utf8&zeroDateTimeBehavior=convertToNull&useSSL=true&serverTimezone=GMT%2B8
                username: root
                password: 485010
```

### Nginx

[default.conf](https://github.com/Ashesttt/picture/blob/main/default.conf)

- 它在服务器的

```jsx
cat /etc/nginx/conf.d/default.conf
```

### default.conf主要配置是：

```c
server {
                listen    4000;#监听端口
                server_name  82.157.238.98;    #服务器名称
                location / {#匹配客户端请求url
                root /usr/share/nginx/ruoyiui;#指定静态资源根目录
                index index.html index.htm;      #指定默认首页
    }
		
	location /prod-api/ {  # 反向代理到后端工程
            proxy_set_header Host $http_host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header REMOTE-HOST $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_pass http://localhost:8080/;
        }

}
```

### 前端

[vue.config.txt](https://github.com/Ashesttt/picture/blob/main/vue.config.txt)

- 它在本机的：

```jsx
D:/RuoYi-Vue-nigge/RuoYi-Vue-master/ruoyi-ui/vue.config.js
```

### vue.config.js主要配置是：

```jsx
const CompressionPlugin = require('compression-webpack-plugin')

const name = process.env.VUE_APP_TITLE || '若依管理系统' // 网页标题

const port = process.env.port || process.env.npm_config_port || 4000 // 端口

// webpack-dev-server 相关配置
  devServer: {
    host: '0.0.0.0',
    port: port,
    open: true,
    proxy: {
      // detail: https://cli.vuejs.org/config/#devserver-proxy
      [process.env.VUE_APP_BASE_API]: {
        target: `http://localhost:8080`,
```

7.**访问**

[http://82.157.238.98:4000/](http://82.157.238.98:4000/)

[](http://jerryestt.top:4000/)
