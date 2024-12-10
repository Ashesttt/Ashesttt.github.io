# 配置Docker镜像源

## 1. 配置单次地址

1. **假如拉取原始镜像命令如下:**

```bash
docker pull jerryestt/backend-image:latest
```

2. **仅需在原命令前缀加入加速镜像地址 例如：**

```bash
docker pull dockerpull.com/jerryestt/backend-image:latest
```



## 2. 一键设置镜像加速

1. 修改文件 /etc/docker/daemon.json（如果不存在则创建）

```bash
vim /etc/docker/daemon.json
```

2. 修改JSON文件 更改为以下内容 然后保存(注意替换地址)

```
{
  "registry-mirrors": [
          "https://n1j279su.mirror.aliyuncs.com",
          "https://docker.1panel.live/",
          "https://hub.rat.dev/",
          "http://docker.registry.cyou/",
          "https://dockerpull.com/",
          "http://dockerproxy.cn/"
  ]
}
```

3. 保存好之后 执行以下两条命令

```
sudo systemctl daemon-reload #重载systemd管理守护进程配置文件
```

```
sudo systemctl restart docker #重启 Docker 服务
```



## 3. 列举一些docker镜像源

[目前国内可用Docker镜像源汇总（截至2024年12月） - CoderJia](https://www.coderjia.cn/archives/dba3f94c-a021-468a-8ac6-e840f85867ea#:~:text=国内经常使用Dock)

| Domain                 | Status |
| ----------------------- | ------ |
| docker.registry.cyou    | 正常     |
| docker-cf.registry.cyou | 正常     |
| dockerpull.com          | 正常     |
| dockerproxy.cn          | 正常     |
| docker.1panel.live      | 正常     |
| hub.rat.dev             | 正常     |
| dhub.kubesre.xyz        | 正常     |
| docker.hlyun.org        | 正常     |
| docker.kejilion.pro     | 正常     |
| registry.dockermirror.com | 正常     |
| docker.mrxn.net         | 正常     |
| docker.chenby.cn        | 正常     |
| ccr.ccs.tencentyun.com  | 正常     |
| hub.littlediary.cn      | 正常     |
| hub.firefly.store       | 正常     |
| docker.nat.tf           | 正常     |
| hub.yuzuha.cc           | 正常     |
| hub.crdz.gq             | 正常     |
| noohub.ru               | 异常 ([WinError 10061] 由于目标计算机积极拒绝，无法连接。) |
| docker.nastool.de       | 正常     |
| hub.docker-ttc.xyz      | 正常     |
| freeno.xyz              | 正常     |
| docker.hpcloud.cloud    | 正常     |
| dislabaiot.xyz          | 正常     |
| docker.wget.at          | 正常     |
| ginger20240704.asia     | 正常     |
| lynn520.xyz             | 正常     |
| doublezonline.cloud     | 正常     |
| dockerproxy.com         | 异常 (连接超时) |

