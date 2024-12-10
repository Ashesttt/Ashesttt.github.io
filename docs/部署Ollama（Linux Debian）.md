# 部署Ollama（Linux Debian）

## 1. 官方安装指南

### 安装

按照官方网站的指示，安装Ollama非常简单：

```sh
curl -fsSL https://ollama.com/install.sh | sh
```

### 总结

官方方法简单明了，只需执行一个命令即可完成部署。

## 2. 现实中的挑战

### 下载速度问题

无论是通过直连还是使用代理，安装Ollama的下载速度都可能非常慢，导致安装时间过长。

#### 解决方法

##### 2.1 修改安装脚本的下载地址

为了加速下载，可以修改安装脚本中的URL地址：

1. **下载并修改安装脚本**：

   ```sh
   curl -fsSL https://ollama.com/install.sh -o ollama_install.sh
   chmod +x ollama_install.sh
   ```

2. **修改下载源**：

   打开 `ollama_install.sh`，找到并替换以下URL：

   - 原始URL：`https://ollama.com/download/ollama-linux-${ARCH}${VER_PARAM}`
   - 加速URL：`https://github.moeyy.xyz/https://github.com/ollama/ollama/releases/download/v0.3.4/ollama-linux-amd64`

   对于ROCM版本也同样处理。

3. **运行修改后的脚本**：

   ```sh
   sudo ./ollama_install.sh
   ```

##### 2.2 离线安装

对于复杂的离线安装方法，可以参考以下链接，但由于步骤繁琐，这里不再赘述：

- [Linux内网离线安装Ollama](https://blog.csdn.net/u010197332/article/details/137604798)

## 3. 优化安装方法

结合官方方法和加速下载地址，我采用了以下步骤：

### 1. 下载 `ollama-linux-arm64.tgz`

使用加速镜像下载安装包：

```sh
curl -L https://gh.api.99988866.xyz/https://github.com/ollama/ollama/releases/download/v0.4.6/ollama-linux-arm64.tgz -o ollama-linux-arm64.tgz
```

### 2. 准备安装目录

创建并确保安装目录存在：

```sh
sudo mkdir -p /jerry_2/play/Ollama
```

### 3. 解压安装包

将下载的文件解压到指定目录：

```sh
sudo tar -C /jerry_2/play/Ollama -xzf ollama-linux-arm64.tgz
```

### 4. 设置执行权限

为 `ollama` 文件设置执行权限：

```sh
sudo chmod +x /jerry_2/play/Ollama/bin/ollama
```

### 5. 添加到系统PATH

将 `ollama` 的安装路径添加到系统的 `PATH` 环境变量中：（还有设置默认端口）

```sh
echo 'export PATH=$PATH:/jerry_2/play/Ollama/bin' >> ~/.bashrc
echo export OLLAMA_HOST="0.0.0.0:11434">>~/.bashrc
source ~/.bashrc

echo 'export PATH=$PATH:/jerry_2/play/Ollama/bin' >> ~/.zshrc
echo export OLLAMA_HOST="0.0.0.0:11434">>~/.zshrc
source ~/.zshrc
```

### 6. 启动 Ollama

现在可以直接运行 `ollama` 命令来启动服务：

```sh
ollama serve
```

### 7. 验证安装

在新终端中运行 `ollama -v` 来检查安装是否成功。

### 8. 配置服务（可选）

#### 创建 `ollama` 用户和组

为安全起见，创建专门的用户和组来运行 `ollama`：

```sh
sudo useradd -r -s /bin/false -U -m -d /usr/share/ollama ollama
```

#### 添加当前用户到 `ollama` 组

将当前用户添加到新创建的 `ollama` 组中：

```sh
sudo usermod -a -G ollama $(whoami)
newgrp ollama
```

#### 创建并配置 `systemd` 服务

创建 `ollama` 的 `systemd` 服务文件：

```sh
sudo vim /etc/systemd/system/ollama.service
```

添加以下内容，确保路径正确：

```ini
[Unit]
Description=Ollama Service
After=network-online.target

[Service]
ExecStart=/jerry_2/play/Ollama/bin/ollama serve
User=ollama
Group=ollama
Restart=always
RestartSec=3
Environment="PATH=$PATH"
Environment="OLLAMA_HOST=0.0.0.0:11434
[Install]
WantedBy=default.target
```

其中`Environment="OLLAMA_HOST=0.0.0.0:11434"`将允许你通过服务器的公共IP访问API

## 4. Ollama模型下载路径配置

### 1. 创建模型存储目录

为Ollama模型创建一个新目录：

```sh
sudo mkdir /jerry_2/play/Ollama/models
```

### 2. 设置目录权限

更改目录的用户和组为 `ollama`，并设置适当的权限：

```sh
sudo chown -R ollama:ollama /jerry_2/play/Ollama/models
sudo chmod -R 775 /jerry_2/play/Ollama/models
```

### 3. 配置 `ollama.service`

编辑 `ollama.service` 文件，添加环境变量以指定模型存储路径：

```sh
sudo vim /etc/systemd/system/ollama.service
```

在 `[Service]` 部分添加：

```ini
Environment="OLLAMA_MODELS=/jerry_2/play/Ollama/models"
```

保存并退出编辑器，然后重新加载 `systemd` 配置并启动服务：

```sh
sudo systemctl daemon-reload
sudo systemctl enable ollama
sudo systemctl start ollama
sudo systemctl status ollama
```

通过这些步骤，您应该能够更高效地在Linux Debian系统上部署和配置Ollama服务。