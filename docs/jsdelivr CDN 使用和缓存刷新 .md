# jsdelivr CDN 使用和缓存刷新 

jsDelivr 提供的全球 CDN 加速，CDN的分流作用不仅减少了用户的访问延时，也减少的源站的负载。因为 jsDelivr 是开源的免费 cdn，所以我个人一直在使用他

## 1. jsdelivr的cdn使用规则

```bash
https://cdn.jsdelivr.net/gh/user/repo@version/file
```

例如：

```bash
https://cdn.jsdelivr.net/gh/Ashesttt/Emergency_Supplies_Management_System@Emergency_Material_Manage_System/docker-compose.yml
```

## 2. jsdelivr 缓存刷新方法

> [!NOTE]
> 有的时候在github上更新了某个文件，但是wget下来还是旧的，没更新，那么就需要缓存刷新

对于 jsDelivr，缓存刷新的方式也很简单，只需将想刷新的链接的开头的cdn 更改为 purge

```
https://cdn.jsdelivr.net/
```

切换为

```
https://purge.jsdelivr.net/
```

例子：

```bash
https://purge.jsdelivr.net/gh/Ashesttt/Emergency_Supplies_Management_System@Emergency_Material_Manage_System/docker-compose.yml
```

