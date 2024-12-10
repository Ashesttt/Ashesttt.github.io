# 使用LVM重新挂载黑豹_x2-tf卡

在使用LVM（逻辑卷管理）来管理你的磁盘空间之前，确保系统已安装LVM工具。如果尚未安装，请执行以下命令：

```bash
sudo apt-get update
sudo apt-get install lvm2
```

### 1. 卸载分区

首先，确认你的TF卡分区已经卸载：

```bash
sudo umount /mnt/tfcard
```

### 2. 编辑`/etc/fstab`

使用`vim`编辑`/etc/fstab`文件，移除任何与`/dev/mmcblk0p1`相关的条目：

```bash
sudo vim /etc/fstab
```

### 3. 创建物理卷（PV）

将你的TF卡分区转换为LVM的物理卷：

```bash
sudo pvcreate /dev/mmcblk0p1
```

### 4. 创建卷组（VG）

创建一个新的卷组，例如名为`jerryvg`：

```bash
sudo vgcreate jerryvg /dev/mmcblk0p1
```

### 5. 创建逻辑卷（LV）

在`jerryvg`卷组中创建一个逻辑卷，可以指定大小或使用所有可用空间：

```bash
# 创建一个名为'jerrylv'的逻辑卷，大小为20G
sudo lvcreate -L 20G -n jerrylv jerryvg

# 或者使用全部可用空间
sudo lvcreate -l 100%FREE -n jerrylv jerryvg
```

### 6. 格式化逻辑卷

为逻辑卷创建文件系统，这里使用ext4：

```bash
sudo mkfs.ext4 /dev/jerryvg/jerrylv
```

### 7. 挂载逻辑卷

创建挂载点并挂载逻辑卷：

```bash
sudo mkdir /jerry_2
sudo mount /dev/jerryvg/jerrylv /jerry_2
```

### 8. 更新`/etc/fstab`

编辑`/etc/fstab`以确保系统启动时自动挂载该逻辑卷：

```bash
sudo vim /etc/fstab
```

添加以下行：

```plaintext
/dev/jerryvg/jerrylv  /jerry_2  ext4  defaults  0  2
```

### 9. 验证挂载

使用`df -h`命令检查逻辑卷是否已正确挂载：

```bash
df -h | grep /jerry_2
```

预期输出：

```plaintext
/dev/mapper/jerryvg-jerrylv   59G   24K   56G   1% /jerry_2
```

### 10. 设置权限

- 如果需要调整`/jerry_2`目录的权限或所有者：

```bash
sudo chown username:username /jerry_2
sudo chmod 755 /jerry_2
```

替换`username`为你的用户名。

- 作为root用户，你已经对`/jerry_2`拥有完全权限。如果需要限制访问：

```bash
sudo chmod 755 /jerry_2
```

这将允许所有用户读取和执行目录内容，但只有root可以写入。