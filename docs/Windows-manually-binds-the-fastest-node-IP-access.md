---
layout: '../../layouts/MarkdownPost.astro'
title: 'Windows手动绑定访问最快的节点ip'
pubDate: 2023-10-2
description: 'Windows手动绑定访问最快的节点ip'
author: 'hjh'
cover:
    url: 'https://cdnjson.com/images/2023/10/02/wallhaven-wqzkyp_2560x1080.png'
    square: 'https://cdnjson.com/images/2023/10/02/wallhaven-wqzkyp_2560x1080.png'
    alt: 'cover'
tags: ["Windows","ip" ,"代理", "cmd"]
theme: 'light'
featured: false
---

# Windows手动绑定访问最快的节点ip


在Windows操作系统中，你可以使用命令提示符（cmd）来手动绑定访问最快的节点IP地址。下面是具体的步骤（以：github.com为例子）：

1. 打开命令提示符（cmd）。你可以通过在开始菜单中搜索“cmd”来找到它。
2. 用这个网站查找哪个IP响应时间段短，哪个TTL越大，哪个IP就越快越好。

   [多个地点Ping服务器,网站测速 - 站长工具](https://ping.chinaz.com/)

3. 打开C:\Windows\System32\drivers\etc
4. 输入命令dir查看etc文件夹有哪些文件。
5. 输入命令notepad networks，弹出文本编辑器。
6. 以以下这种格式编写。

    ```c
    # GitHub
    20.205.243.166 github.com
    
    #20.27.177.116 api.github.com
    #useless
    
    # Copilot
    140.82.113.18 copilot.github.com
    
    # chatgpt
    #172.64.150.28 chat.openai.com
    #useless
    ```

7. 最后ping一下。

    ```c
    ping github.com
    ```


至此，不用clash就可以访问到github，举一反三，需要代理的网站或软件可以通过这种方式配置。

通过使用命令提示符手动绑定访问最快的节点IP地址，你可以提高网络连接速度并改善网络体验。

注意：在进行这些更改之前，请确保你已经获取了最快节点的IP地址，并确保你了解网络设置的影响。
