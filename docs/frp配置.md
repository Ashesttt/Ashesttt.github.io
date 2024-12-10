# frp配置

## 前情提要：

1. 路由器是通过宽带拨号联网的，服务商是中国电信，我向客服申请了公网 ip（==虽然有公网ip了，但是无法直接使用这个公网ip，而且还是动态的==）

2. 我家的路由器是 tp-link 的，可以通过 TP-LINK 物联 APP 进行管理（==虽然无法直接使用这个公网ip，但是可以通过路由器的DDNS功能免费使用TP-LINK的域名）==
3. TP-LINK物联APP还可以设置虚拟服务器（即端口转发）（==虽然可以使用TP-LINK的域名加上端口转发，可以实现在公网随时访问得到自己部署的项目，但是端口转发名额只有16个）==

4. 我有一个家里的服务器（其实有三个，两个黑豹 x2(刷机教程：[PantherX2(黑豹 X2)刷机-如普·Blog](https://rupu.net/archives/pantherx2#wifi%E7%9B%B8%E5%85%B3))（咸鱼100米左右一个），一个玩客云，但是主要是玩黑豹 x2）（==虽然CPU是4核的，内存是4G的，配置还挺高的，可以部署很多小项目，但是路由器的限制，最多只能端口转发13个端口（因为要除去就家里服务器的三个22端口）==），先把这个黑豹 x2 服务器称作：==**服务器 A**==
5. 阿里云上也有一个服务器（上一年有活动，99米用两年）（==虽然有公网ip，但是CPU，内存分别只有可怜的2核，2G==），把这个阿里云服务器称作：**==服务器B==**

###### 那么总结一下我的两个服务器情况：

* **服务器A**：虽然配置较好，适合部署很多项目，但受限于家庭网络环境和路由器的端口转发限制，在同时运行多个需要公网访问的服务时遇到瓶颈。
* **服务器B**：虽然配置较低，不适合部署很多项目，但由于直接拥有公网 IP，对于需要稳定公网访问的服务来说是一个更直接、灵活的选择。

‍

**总结：能不能有一个方法，可以让我不再被TP-LINK的这16个端口限制呢？**

‍

## 什么是内网穿透？

内网穿透是指通过特定的网络技术或工具，突破内网的防火墙和路由器，允许外部设备访问内网的服务。常见的应用场景包括：

* **远程控制内网设备**：开发者需要在外部访问处于内网中的服务器。
* **网站和API的暴露**：开发中的Web应用、数据库等需要暴露给外部进行测试。
* **IoT设备接入**：物联网设备通过内网穿透与外部服务通信。

内网穿透工具通过“隧道”或“代理”方式实现外部设备和内网设备之间的直接连接，而无需修改路由器或防火墙配置。

‍

## 挑选内网穿透工具

[推荐10个内网穿透工具，有免费且开源的，也有国产的，太香了！](https://mp.weixin.qq.com/s/xFLPm4aPLLEvoot2sEw3Sw)

[常见的内网穿透工具对比_frp和nps哪个速度快-CSDN博客](https://blog.csdn.net/P2LINKniu/article/details/143228200)

1. 由于我喜欢用docker，就找有没有docker版的frp，找到了这个[VaalaCat/frp-panel](https://github.com/VaalaCat/frp-panel)，但是我的服务器A比较特殊，我无法控制公网ip的端口，端口只能通过转发的方式开放，折腾了挺久，最终放弃用这个docker版的frp
2. 最终我选择了**frp**

‍

## FRP(Fast Reverse Proxy)

FRP 是一个高性能的反向代理应用，旨在帮助用户穿透防火墙，支持 TCP、UDP、HTTP、HTTPS 等协议的穿透。FRP 采用客户端-服务端架构，允许内网服务通过外网代理服务器公开访问。

```
https://github.com/fatedier/frp
```

### 特点

* **支持多种协议**：FRP 支持 TCP、UDP、HTTP、HTTPS 等多种协议，适用于 Web 服务、数据库、SSH 等应用。
* **性能优越**：FRP 的设计目标之一是高效的传输速度，能够在有限的带宽条件下提供稳定的连接。
* **易于配置**：FRP 提供了简单易用的配置文件和命令行参数，支持快速部署。
* **加密与安全**：FRP 使用 TLS 加密协议，保证传输过程中的数据安全。

### 使用场景

* **Web 应用远程访问**：开发者可以将本地开发的 Web 服务暴露到外网，进行远程调试和测试。
* **数据库服务暴露**：在不暴露真实 IP 地址的情况下，安全地访问内网数据库。
* **远程 SSH 访问**：通过 FRP 实现远程 SSH 登录内网服务器。

### 优缺点

* **优点**：免费、开源、易于配置、支持多种协议。
* **缺点**：需要有一个公网上的中转服务器作为代理。

### 安装与配置

1. 下载 FRP 安装包，您可以从 GitHub 的 [Release](https://github.com/fatedier/frp/releases) 页面中下载最新版本的客户端和服务器二进制文件。
2. 配置服务器端（frps.ini）和客户端（frpc.ini）。
3. 启动 FRP 服务：
4. 1. 使用以下命令启动服务器：`./frps -c ./frps.toml`​。
    2. 使用以下命令启动客户端：`./frpc -c ./frpc.toml`​。
    3. 如果需要在后台长期运行，建议结合其他工具，如 [systemd](https://gofrp.org/zh-cn/docs/setup/systemd/)

‍

### Web 界面

目前 frpc 和 frps 分别内置了相应的 Web 界面方便用户使用。

#### 服务端 Dashboard

服务端 Dashboard 使用户可以通过浏览器查看 frp 的状态以及代理统计信息。

需要在 frps.toml 中指定 dashboard 服务使用的端口，即可开启此功能：

```toml
# 默认为 127.0.0.1，如果需要公网访问，需要修改为 0.0.0.0。
webServer.addr = "0.0.0.0"
webServer.port = 7500
# dashboard 用户名密码，可选，默认为空
webServer.user = "admin"
webServer.password = "admin"
```

## 客户端管理界面 Admin UI

frpc 内置的 Admin UI 可以帮助用户通过浏览器来查询和管理客户端的 proxy 状态和配置。

需要在 frpc.toml 中指定 admin 服务使用的端口，即可开启此功能：

```toml
# frpc.toml
webServer.addr = "127.0.0.1"
webServer.port = 7400
webServer.user = "admin"
webServer.password = "admin"
```

打开浏览器通过 `http://127.0.0.1:7400`​ 访问 Admin UI。

如果想要在外网环境访问 Admin UI，可以将 7400 端口通过 frp 映射出去即可，但需要重视安全风险。

```toml
# frpc.toml
[[proxies]]
name = "admin_ui"
type = "tcp"
localPort = 7400
remotePort = 7400
```

‍

## 我遇到的问题

### 服务端 Dashboard

* 由于我喜欢在web上设置，部署frpc和frps，这样就方便很多，但是我试了很多次，很多种方法，都无法通过编辑frps.toml来启动dashboard 服务
* 输入命令`./frps -h`​查看帮助，最终通过命令行的形式把dashboard启动了
* ```bash
  ./frps \
      --bind-port 7000 \
      --token your_token\
      --log-file /your/path/log/frps.log \
      --log-level info \
      --log-max-days 3 \
      --disable-log-color \
      --dashboard-port 7500 \
      --dashboard-user admin \
      --dashboard-pwd admin
  ```

‍

### 客户端管理界面 Admin UI

* 也试过很多方法都无法通过编辑frpc.toml来启动Admin UI（根本没有启动，我curl 127.0.0:7400返回refuse，根本启动不了）
* 命令行也启动不了，`./frpc -h`​帮助里根本没有dashboard也没有什么AdminUI，试了很久也启动不了，放弃了。

‍

### 插件

#### [frps-panel: ](https://github.com/yhl452493373/frps-panel)

* frps-panel 是 frp 的一个服务端插件，用于支持多用户鉴权，同时用于展示服务器信息。
* 但是，是基于服务端的Dashboard的api的，基本功能也相差不大。

#### [frpc-panel: ](https://github.com/yhl452493373/frpc-panel)

* frpc-panel 是 frp 的一个客户端工具，用于更好的展示客户端信息，以及管理客户端代理信息。
* 但是，frpc-panel是基于客户端的AdminUI的，所以我也无法使用

‍
