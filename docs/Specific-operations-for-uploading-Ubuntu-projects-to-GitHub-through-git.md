---
layout: '../../layouts/MarkdownPost.astro'
title: 'Ubuntu的项目通过git上传到GitHub的具体操作'
pubDate: 2023-03-20
description: 'Ubuntu的项目通过git上传到GitHub的具体操作'
author: 'hjh'
cover:
    url: 'https://cdnjson.com/images/2023/04/02/CB7D407C232A13EFD6874878E4AC2AAD---.jpg'
    square: 'https://cdnjson.com/images/2023/04/02/CB7D407C232A13EFD6874878E4AC2AAD---.jpg'
    alt: 'cover'
tags: ["git", "GitHub", "Ubuntu"]
theme: 'dark'
featured: true
---
# Ubuntu的项目通过git上传到GitHub的具体操作

![IMG_0467.jpeg](https://s2.loli.net/2023/04/02/j59QbIeHtJc3TSP.jpg)



工作区：
开发程序的文件夹
暂存区：
修改后的文件临时暂存到内存中
本地版本库：
将写好的项目放在本地版本库中保存
服务器版本库：
将写好的项目放在服务器版本库中保存

# 首先创造一个文本：

```bash
vim test.txt
```

![IMG_0464.jpeg](https://s2.loli.net/2023/04/02/gdIBheAVl5mcGsJ.jpg)

# 查看当前仓库状态：

```bash
git status
```

![IMG_0464.jpeg](https://s2.loli.net/2023/04/02/FWDHMPt3TRbZq1v.jpg)

# 把文件的变更提交到暂存区：(并看看当前仓库状态）

```bash
git add <文件名>

#也可以将当前路径下的所有变更提交到暂存区

git add .
```

![IMG_0465.jpeg](https://s2.loli.net/2023/04/02/R37pKirdNqEZmMX.jpg)

# *将暂存区的内容提交到版本库：（在在看看当前仓库状态）*

```bash
git commit -m 注释
```

成功的话会出现：

![IMG_0466.jpeg](https://s2.loli.net/2023/04/02/XxTD7ij2m58KbBk.jpg)





会发现已经没有叫你 git add test.txt了

# 最后推送到github：

```bash
git push
```

![IMG_0466.jpeg](https://s2.loli.net/2023/04/02/VNmw7YCR1Hul9gI.jpg)

我已经用 github 的 api 的 token 密钥绑定了我的 ubuntu 的 git push 配置中了

