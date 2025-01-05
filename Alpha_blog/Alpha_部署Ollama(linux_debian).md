# Alpha_部署Ollama(linux_debian)

## 1. 我们先看看官网怎么说的：

#### 安装

要安装 Ollama，请运行以下命令：

```
curl -fsSL https://ollama.com/install.sh | sh
```

#### 总结：好容易部署啊，就cv一个命令就可以部署Ollama，太好啦

## 2. 可是现实就爱踢你一脚

#### 无论是直连，还是科学上网，都下载的好慢，大概要下载个2，3天，于是果断放弃，果断上网搜索：ollama linux 下载慢，搜出来大概有这些解决方法：

##### 2.1修改脚本的url地址

##### 在url地址前加上加速网址https://github.moeyy.xyz或者https://gh.api.99988866.xyz等等

首先，下载安装脚本：

```sh
curl -fsSL https://ollama.com/install.sh -o ollama_install.sh
```

然后，给脚本添加执行权限：

```sh
chmod +x ollama_install.sh
```

步骤 2: 修改下载源

打开 `ollama_install.sh`，找到以下两个下载地址：

- `https://ollama.com/download/ollama-linux-${ARCH}${VER_PARAM}`
- `https://ollama.com/download/ollama-linux-amd64-rocm.tgz${VER_PARAM}`

最终，把下载地址改为：

- `https://github.moeyy.xyz/https://github.com/ollama/ollama/releases/download/v0.3.4/ollama-linux-amd64`
- `https://github.moeyy.xyz/https://github.com/ollama/ollama/releases/download/v0.3.4/ollama-linux-amd64-rocm.tgz`

最后，运行修改后的脚本：

```sh
sudo ./ollama_install.sh
```

##### 2.2 离线安装+然后修改install.sh脚本啥的

详细请看：[Linux内网离线安装Ollama_ollama离线安装-CSDN博客](https://blog.csdn.net/u010197332/article/details/137604798)

果断放弃，太复杂了



## 3. 我的解决方法：

我是结合了官网和加速网址的方法

### 1. 下载 `ollama-linux-arm64.tgz`

首先，使用 `curl` 下载 `ollama-linux-arm64.tgz` 文件：

```bash
curl -L https://gh.api.99988866.xyz/https://github.com/ollama/ollama/releases/download/v0.4.6/ollama-linux-arm64.tgz -o ollama-linux-arm64.tgz
```

### 2. 创建目标目录

确保目标目录 `/jerry_2/play/Ollama` 存在：

```bash
sudo mkdir -p /jerry_2/play/Ollama
```

### 3. 解压文件到指定目录

使用 `tar` 命令将文件解压到 `/jerry_2/play/Ollama`：

```bash
sudo tar -C /jerry_2/play/Ollama -xzf ollama-linux-arm64.tgz
```

### 4. 设置权限

确保解压后的 `ollama` 文件有适当的执行权限：

```bash
sudo chmod +x /jerry_2/play/Ollama/bin/ollama
```

### 5. 添加到 `PATH`

为了能够直接运行 `ollama` 命令，你需要将 `/jerry_2/play/Ollama/bin` 添加到 `PATH` 环境变量中。

编辑你的 shell 配置文件（例如 `.bashrc` 或 `.zshrc`）：

```bash
echo 'export PATH=$PATH:/jerry_2/play/Ollama/bin' >> ~/.bashrc
```

或者，如果你使用的是 `zsh`：

```bash
echo 'export PATH=$PATH:/jerry_2/play/Ollama/bin' >> ~/.zshrc
```

然后，重新加载配置文件：

```bash
source ~/.bashrc
```

或者：

```bash
source ~/.zshrc
```

### 6. 启动 `ollama`

现在，你应该能够直接运行 `ollama` 命令了。启动 `ollama` 服务：

```bash
ollama serve
```

### 7. 验证安装

在另一个终端中，验证 `ollama` 是否正在运行：

```bash
ollama -v
```

### 8. 配置服务（可选）

#### 创建 `ollama` 用户和组

首先，创建 `ollama` 用户和组：

```bash
sudo useradd -r -s /bin/false -U -m -d /usr/share/ollama ollama
```

- `-r`: 创建系统用户
- `-s /bin/false`: 设置用户的默认 shell 为 `/bin/false`，表示此用户不能登录系统
- `-U`: 创建与用户同名的组
- `-m`: 创建用户的家目录
- `-d /usr/share/ollama`: 设置用户的家目录为 `/usr/share/ollama`

#### 将当前用户添加到 `ollama` 组

然后，将当前用户添加到 `ollama` 组中：

```bash
sudo usermod -a -G ollama $(whoami)
```

- `-a`: 追加用户到指定的组
- `-G ollama`: 指定要加入的组
- `$(whoami)`: 获取当前登录用户的用户名

#### 更新用户组

为了使当前会话立即生效，你可能需要重新登录或使用以下命令更新用户组：

```bash
newgrp ollama
```

如果你希望 `ollama` 作为系统服务运行，你可能需要手动创建一个 systemd 服务文件。由于你将 `ollama` 安装在非标准位置，你需要修改服务文件中的路径。以下是一个示例：

创建服务文件：

```bash
sudo vim /etc/systemd/system/ollama.service
```

在文件中添加以下内容，注意 `ExecStart` 路径：

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
Environment="OLLAMA_HOST=0.0.0.0:11434"
[Install]
WantedBy=default.target
```

其中`Environment="OLLAMA_HOST=0.0.0.0:11434"`将允许你通过服务器的公共IP访问API

# Ollama模型下载路径替换

先创建一个新文件夹

```bash
sudo mkdir /jerry_2/play/Ollama/models
```

将目标路径的所属用户和组改为`ollama`

```bash
sudo chown -R ollama:ollama /jerry_2/play/Ollama/models
```

将其文件权限更换为`777`

```bash
sudo chmod -R 775 /jerry_2/play/Ollama/models
```

1. **更改目录**

用`vim`[编译器](https://marketing.csdn.net/p/3127db09a98e0723b83b2914d9256174?pId=2782?utm_source=glcblog&spm=1001.2101.3001.7020)打开`ollama.service`

```bash
sudo vim /etc/systemd/system/ollama.service
```

在`[Service]`下面加入一行新的`Environment`，新一行！ 如下图：

```bash
Environment="OLLAMA_MODELS=/jerry_2/play/Ollama/models" # 记得替换路径！！！
```



保存并退出编辑器。之后，启用并启动服务：

```bash
sudo systemctl daemon-reload
sudo systemctl enable ollama
sudo systemctl start ollama
sudo systemctl status ollama
```

