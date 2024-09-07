## 构建docker

### 构建docker的方法：

#### Dockerfile.frontend

```bash
docker build -f Dockerfile.frontend -t jerryestt/frontend-image:latest .
```



#### Dockerfile.backend

```bash
docker build -f Dockerfile.backend -t jerryestt/backend-image:latest .
```



# 文件：Dockerfile.frontend

### 1.

> 1.33GB

```dockerfile
FROM node:16.19
WORKDIR /app
COPY vue/package*.json /app/
RUN npm install
COPY vue /app
CMD ["npm", "run", "serve"]
```



### 2.

> 42.9MB

```dockerfile
FROM node:16.19 AS build
WORKDIR /app
COPY vue/package*.json ./
RUN npm install
COPY vue/ ./
RUN npm run build
FROM nginx:alpine
COPY --from=build /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

启动：

```bash
docker run -d -p 80:80 jerryestt/frontend-image
```



# 文件：Dockerfile.backend

### 1.

> 282MB

```dockerfile
FROM openjdk:11-jre-slim
WORKDIR /app
COPY target/SpringBoot-0.0.1-SNAPSHOT.jar /app/app.jar
CMD ["java", "-jar", "/app/app.jar"]
```

### 2.

> 188MB

```dockerfile
# 使用更小的基础镜像
FROM eclipse-temurin:11-jre-alpine
WORKDIR /app
# 复制Spring Boot应用程序
COPY target/SpringBoot-0.0.1-SNAPSHOT.jar /app/app.jar
# 运行应用程序
CMD ["java", "-jar", "/app/app.jar"]

```



## 启动docker

```bash
docker-compose up --build
```



若多次尝试启动，没有成功达到您的目的，可以尝试重新启动容器

```bash
docker-compose down -v
```



## dive：一款按层分析docker镜像的工具

### 安装方法：

```bash
wget https://github.com/wagoodman/dive/releases/download/v0.12.0/dive_0.12.0_linux_amd64.deb
sudo apt install ./dive_0.12.0_linux_amd64.deb
```



### 使用方法：

要分析Docker镜像，只需使用image tag/id/digest运行：

```
dive <your-image-tag>
```



### 键盘绑定：

|   键盘绑定   |                 描述                 |
| :----------: | :----------------------------------: |
| Ctrl + C或Q  |                 退出                 |
|     Tab      |    在图层视图和文件树视图之间切换    |
|   Ctrl + F   |               筛选文件               |
|    PageUp    |             向上滚动页面             |
|   PageDown   |             向下滚动页面             |
|   Ctrl + A   |      图层视图：查看聚合图像修改      |
|   Ctrl + L   |      图层视图：查看当前图层修改      |
|    Space     |     Filetree 视图：折叠/展开目录     |
| Ctrl + Space | Filetree 视图：折叠/取消折叠所有目录 |
|   Ctrl + A   |  Filetree 视图：显示/隐藏添加的文件  |
|   Ctrl + R   | Filetree 视图：显示/隐藏已删除的文件 |
|   Ctrl + M   |  文件树视图：显示/隐藏已修改的文件   |

