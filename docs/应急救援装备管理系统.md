# 应急救援装备管理系统教程

本教程将指导您如何设置和运行应急救援装备管理系统。


## 介绍

一个基于SpringBoot，Vue，Element的应急救援装备管理系统

![show_emergency_system](https://github.com/Ashesttt/Ashesttt.github.io/blob/main/images/show_emergency_system.png?raw=true)



## 在线体验

[应急救援装备管理系统](http://47.92.99.199:8080/login)



## 系统需求

- MySQL == 8.0.36
- Maven == 3.6.3
- Node ==16.19.0



## 快速开始

>目前仅支持mysql

1.首先，创建一个新的数据库 `emergency_material_manage` 并使用它。

```sql
create database emergency_material_manage；
use emergency_material_manage；
```

2.将`emergency_material_manage.sql`文件导入到数据库中。请确保您已将该文件放置在正确的路径中。

```bash
source /Emergency_Supplies_Management_System/emergency_material_manage.sql;
```

3.进入项目，复制后端示例配置文件，并编辑配置文件，填写数据库密码。

>修改以下内容：
>spring.datasource.password（数据库的密码）
>
>api.base-url(您的后端地址，即服务器公网ip+后端端口，默认是9091)

```bash
cd Emergency_Supplies_Management_System/src/main/resources
cp application.yaml.example application.yaml
vim application.yaml
```

4.编辑环境文件，换成您的服务器公网ip+后端端口，默认是9091

>VUE_APP_API_BASE_URL
>
>其中上边的api.base-url和这个一样

```bash
vim Emergency_Supplies_Management_System/vue/.env
```

5.使用 Maven 构建后端 Spring Boot 项目。

```bash
mvn -X clean install
mvn package
```

6.使用以下命令启动后端项目。

```bash
cd target
java -jar SpringBoot-0.0.1-SNAPSHOT.jar
```

7.进入前端项目的目录，并安装所需的依赖项。

```bash
cd /Emergency_Supplies_Management_System/vue
npm install
```

8.启动前端项目

```bash
npm run serve
```





## 支持的功能

### 1.系统管理

|   功能   |   子项   |                             解释                             |
| :------: | :------: | :----------------------------------------------------------: |
| 系统管理 |          |                                                              |
|          | 用户管理 |        对用户进行管理，可新增，删除，编辑用户全部信息        |
|          | 文件管理 | 对上传到服务器的文件进行管理，有文件的详细介绍，可下载，删除，批量删除文件 |
|          | 角色管理 |      对权限与菜单进行分配，可根据部门设置角色的数据权限      |
|          | 菜单管理 |        已实现菜单动态路由，后端可配置化，支持多级菜单        |



### 2.应急物资管理

|     功能     |   子项   |                             解释                             |
| :----------: | :------: | :----------------------------------------------------------: |
| 应急物资管理 |          |                                                              |
|              | 应急物资 | 用于管理和展示物资信息，支持筛选、批量操作、单个物资的增删改查以及物资照片上传等功能 |



### 3.设备跟踪模块

|     功能     |      子项      |                             解释                             |
| :----------: | :------------: | :----------------------------------------------------------: |
| 设备跟踪模块 |                |                                                              |
|              | 仓库的出库记录 | 仓库的出库记录包括:仓库出库记录id,出库时间,申请人,申请原因,出库物资名称,出库前仓库总量,出库数星,运输单号 |
|              | 用户的使用记录 | 用户的使用记录包括：用户使用记录id，出库时间，使用人，使用原因，使用物资名称，使用前拥有物资，总量使用数量 |



### 4.用户物资信息

|     功能     |     子项     |                             解释                             |
| :----------: | :----------: | :----------------------------------------------------------: |
| 用户物资信息 |              |                                                              |
|              | 用户物资管理 |              用于管理员查看每个用户拥有多少物资              |
|              | 个人物资管理 |            用户对物资进行管理，可以申请，使用物资            |
|              | 物资申请审批 | 管理员对用户申请物资进行审批，可以通过，拒绝申请（若通过，则生成运输订单，发给司机，若拒绝，则填写拒绝原因，发送给申请人） |





### 5.物资运输信息

|     功能     |      子项      |                       解释                       |
| :----------: | :------------: | :----------------------------------------------: |
| 物资运输信息 |                |                                                  |
|              |  物资运输管理  | 用于管理员对已通过的申请的物资分配司机，进行运输 |
|              | 司机拉货小程序 |       用于司机配送物资，给系统提供配送详情       |



### 6.信息接收中心

|     功能     |   子项   |                             解释                             |
| :----------: | :------: | :----------------------------------------------------------: |
| 信息接收中心 |          |                                                              |
|              | 申请信息 |                      所有用户的申请信息                      |
|              | 系统日志 | 所有信息（包括：每日库存警告报告，申请信息，用户使用物资信息，司机运输信息） |
|              | 个人日志 |                    基于用户收到的个人信息                    |






## 贡献者
***Thanks to the following people who have contributed to this project:***

[![Contributors](https://contrib.rocks/image?repo=Ashesttt/Emergency_Supplies_Management_System)](https://github.com/Ashesttt/Emergency_Supplies_Management_System/graphs/contributors)
