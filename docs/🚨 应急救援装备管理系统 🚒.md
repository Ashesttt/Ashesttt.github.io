# 🚨 应急救援装备管理系统 🚒

<div align="center">

[![GitHub Release](https://img.shields.io/github/v/release/Ashesttt/Emergency_Supplies_Management_System)](https://img.shields.io/github/v/release/Ashesttt/Emergency_Supplies_Management_System)
[![GitHub License](https://img.shields.io/github/license/Ashesttt/Emergency_Supplies_Management_System)](https://img.shields.io/github/license/Ashesttt/Emergency_Supplies_Management_System)
![Welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg?style=flat)

</div>

## 📖 介绍

一个基于 SpringBoot，Vue，Element 的应急救援装备管理系统 🧰

![show_emergency_system](https://github.com/Ashesttt/Ashesttt.github.io/blob/main/images/show_emergency_system.png?raw=true)

## 🌐 在线体验

[🚑 应急救援装备管理系统](http://47.92.99.199)
> [!IMPORTANT]
> 账号：**admin** 
> 密码：**admin**

## 🛠️ 系统需求

- 🐬 **MySQL** == 8.0.36
- 🧰 **Maven** == 3.6.3
- 🌳 **Node** == 16.19.0

## 🚀 快速开始

**Emergency_Supplies_Management_System** 支持  [🐳 Docker Compose 部署](#docker-compose-部署) 和 [📄 本地部署](./docs/development.md#本地部署)

> [!TIP]
> 推荐使用 Docker-compose 部署。

### 🐳 Docker Compose 部署 

1. 📂 创建文件夹并下载 `docker-compose.yml` 和 `emergency_material_manage.sql`

```bash
mkdir Emergency_Supplies_Management_System && cd Emergency_Supplies_Management_System && curl -O https://cdn.jsdelivr.net/gh/Ashesttt/Emergency_Supplies_Management_System@Emergency_Material_Manage_System/docker-compose.yml -O https://cdn.jsdelivr.net/gh/Ashesttt/Emergency_Supplies_Management_System@Emergency_Material_Manage_System/emergency_material_manage.sql
```

2. 📝 可编辑端口

> [!NOTE]
> 默认后端端口为：9091
> 前端端口为：80

```bash
vim docker-compose.yml
```

3. 🏃 运行 Docker Compose

```bash
docker-compose up --build
```
> [!TIP]
> 若多次尝试启动，没有成功达到您的目的，可以尝试停止启动容器

```bash
docker-compose down -v
```

## 🧩 支持的功能

### 1️⃣ 系统管理

|   功能   |    子项    |                             解释                             |
| :------: | :--------: | :----------------------------------------------------------: |
| 系统管理 |            |                                                              |
|          | 👤 用户管理 |        对用户进行管理，可新增，删除，编辑用户全部信息        |
|          | 📂 文件管理 | 对上传到服务器的文件进行管理，有文件的详细介绍，可下载，删除，批量删除文件 |
|          | 🔐 角色管理 |      对权限与菜单进行分配，可根据部门设置角色的数据权限      |
|          | 🗂️ 菜单管理 |        已实现菜单动态路由，后端可配置化，支持多级菜单        |

### 2️⃣ 应急物资管理

|     功能     |    子项    |                             解释                             |
| :----------: | :--------: | :----------------------------------------------------------: |
| 应急物资管理 |            |                                                              |
|              | 🚚 应急物资 | 用于管理和展示物资信息，支持筛选、批量操作、单个物资的增删改查以及物资照片上传等功能 |

### 3️⃣ 设备跟踪模块

|     功能     |       子项       |                             解释                             |
| :----------: | :--------------: | :----------------------------------------------------------: |
| 设备跟踪模块 |                  |                                                              |
|              | 📝 仓库的出库记录 | 仓库的出库记录包括:仓库出库记录id,出库时间,申请人,申请原因,出库物资名称,出库前仓库总量,出库数量,运输单号 |
|              | 🗃️ 用户的使用记录 | 用户的使用记录包括：用户使用记录id，出库时间，使用人，使用原因，使用物资名称，使用前拥有物资，总量使用数量 |

### 4️⃣ 用户物资信息

|     功能     |      子项      |                             解释                             |
| :----------: | :------------: | :----------------------------------------------------------: |
| 用户物资信息 |                |                                                              |
|              | 👥 用户物资管理 |              用于管理员查看每个用户拥有多少物资              |
|              | 🔧 个人物资管理 |            用户对物资进行管理，可以申请，使用物资            |
|              | 📝 物资申请审批 | 管理员对用户申请物资进行审批，可以通过，拒绝申请（若通过，则生成运输订单，发给司机，若拒绝，则填写拒绝原因，发送给申请人） |

### 5️⃣ 物资运输信息

|     功能     |       子项       |                       解释                       |
| :----------: | :--------------: | :----------------------------------------------: |
| 物资运输信息 |                  |                                                  |
|              |  🚚 物资运输管理  | 用于管理员对已通过的申请的物资分配司机，进行运输 |
|              | 🚛 司机拉货小程序 |       用于司机配送物资，给系统提供配送详情       |

### 6️⃣ 信息接收中心

|     功能     |    子项    |                             解释                             |
| :----------: | :--------: | :----------------------------------------------------------: |
| 信息接收中心 |            |                                                              |
|              | 📨 申请信息 |                      所有用户的申请信息                      |
|              | 📋 系统日志 | 所有信息（包括：每日库存警告报告，申请信息，用户使用物资信息，司机运输信息） |
|              | 📄 个人日志 |                    基于用户收到的个人信息                    |

## 🤝 贡献者

***Thanks to the following people who have contributed to this project:***

[![Contributors](https://contrib.rocks/image?repo=Ashesttt/Emergency_Supplies_Management_System)](https://github.com/Ashesttt/Emergency_Supplies_Management_System/graphs/contributors)