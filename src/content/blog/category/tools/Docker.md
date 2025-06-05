---
title: "docker 笔记"
description: "docker基本概念与常见命令，以及一些使用过程中遇到的问题"
pubDate: "Jul 05 2025"
image:/image/category/tools/docker.jpg
categories:
  - tools
tags:
  - Makrdown
badge: Pin
---

参考文档：[docker 教程](https://www.runoob.com/docker/docker-tutorial.html)



# 一句话解释 docker 的作用

通过 docker构建镜像过后，只需要通过启动镜像（启动后的镜像叫做容器），就能快速启动程序。在不同的机器上，都可以利用该镜像来部署服务，而不需要考虑环境的兼容性，这就是 docker 的作用。

# Docker 的架构介绍

Docker是基于 C/S 架构，使用远程 API 来管理和创建 Docker容器

Docker 容器通过镜像来创建，容器的镜像的关系：镜像-->类，容器-->对象

![Docker 架构示意图](/image/category/tools/docker-architecture.png)

Docker 架构的工作流程：

**构建镜像**：使用`Dockerfile`来构建镜像

**推送镜像到仓库**：将镜像上传到 Docker Hub 或私有注册表中

**拉取镜像**：从仓库拉取镜像

**运行容器**：使用镜像创建并启动容器

**管理容器**：使用 docker 客户端命令管理容器（查看日志、停止容器、查看资源使用）

**网络与存储**：容器之间通过 docker 网络连接；数据通过 docker 卷或绑定挂载进行持久



# Docker 的一些核心组件

## Docker Client

Docker 客户端其实就是命令行界面（CLI），用户通过客户端与 Docker 守护进程进行交互（用户通过 Docker CLI 向守护进程发送命令，由守护进程执行相应操作）

## Docker Daemon

Docker Daemon 是实际执行操作的组件。它的主要功能如下：

- 启动和停止容器
- 构建、拉取、推送镜像
- 管理容器的网络和存储
- 查看日志



## Docker Containers

容器是从 Docker 镜像启动的，它**包含了运行某个应用程序所需的一切**，不同容器之间的文件系统和进程相互**隔离**

容器不依赖环境配置，**所有运行时需要的依赖已经封装在了镜像中**。



## Docker Images

将应用程序运行时需要的一切打包在一起（包括代码、依赖的环境），构成了镜像。镜像是只读的，同一个镜像可以实例化不同的容器。

镜像可以**从仓库拉取**（docker hub 或者私有仓库），也可以**由 Dockerfile 构建**。



## Docker Registries

Docker 仓库就是用来**存储镜像的地方**，最常用的公共仓库是Docker Hub。也可以部署自己的私有仓库来管理企业内部的镜像



## Docker Compose

docker compose 用于定义和运行**多个容器**。通过 compose，用户可以使用配置文件定义多个容器（服务），并通过一个命令启动这些容器。

注意：通过docker-compose.yml 来进行配置



## Docker Networks

docker 网络能够允许容器之间通信，并与外部网络连接。



## Docker Volumes

docker 卷是一种持久化机制，将数据落盘于硬盘上，不会因为容器停止后数据被删除。



# Docker 常用命令

## 容器

`docker run`创建并启动容器

`docker start`启动容器

`docker restart`重新启动容器

`docker ps` 查看容器

`docker stop` 终止容器

`docker attach`进入容器

`docker exec`进入容器

`docker export`导出容器

`docker import`导出容器快照

`docker rm`删除容器

`docker logs`查看日志

## 镜像

`docker search`检索镜像

`docker pull`拉取镜像

`docker images`列出镜像

`docker image rm`删除镜像

`docker save`导出镜像

`docker load`导入镜像

`docker run`运行镜像

## Dockerfile 相关

### docker命令

`docker build`构建镜像

### 参数

`FROM`：指定基础镜像。每个 Dockerfile 的第一条指令必须是 `FROM`

> `FROM python:3.9`



`RUN`：在镜像构建过程中执行命令。常用于安装软件包、更新系统等

> `RUN apt-get update && apt-get install ffmpeg inetutils-ping sshpass nano vim -y`



`COPY`：将文件或目录从构建上下文（通常是 Dockerfile 所在的目录）**复制到镜像的文件系统中**

> `COPY ./requirements.txt /opt/requirements.txt`



`ADD`：与 `COPY` 类似，但它还可以处理远程 URL 和解压本地压缩包

> ```dockerfile
> ADD https://example.com/somefile.txt /app/somefile.txt
> ADD myapp.tar.gz /app/
> ```



`CMD`容器启动命令



`ENV`：设置环境变量。这些环境变量在构建过程中以及容器运行时都可用

> ```dockerfile
> ENV http_proxy "http://dev.datamatrixai.com:30130"
> ENV https_proxy "http://dev.datamatrixai.com:30130"
> ```



`VOLUME`：创建一个可以从本地主机或其他容器挂载的挂载点。用于持久化数据或共享数据

> ```dockerfile
> VOLUME /data
> ```



`EXPOSE`：**声明**容器**运行**时监听的网络**端口**。这只是一**个文档性的声明**，并**不会实际发布端口**。实际发布端口需要在 `docker run` 时使用 `-p` 或 `-P` 选项。

> ```dockerfile
> EXPOSE 80
> ```

## docker服务

`docker version`查看 docker 详细版本信息

`docker -v`查看docker简要信息

`ststemctl start docker`启用docker

`ststemctl stop docker`关闭 docker

`ststemctl enable docker` 设置开机启动

`service docker restart` 重启 docker 服务

`service docker stop`关闭 docker 服务（这个命令和上方的 ststemctl stop 的效果一样，不过它是历史遗留命令）

## docker-compose

> `docker-compose` 是一个**用于定义和运行多容器** Docker 应用程序的工具。

### 常用命令

**`docker-compose up`**：构建（如果需要）、（重新）创建、启动并附加到服务的容器

常用选项：

- `-d`：在后台（分离模式）运行容器。
- `--build`：在启动容器前构建镜像。
- `--force-recreate`：即使容器配置没有改变，也强制重新创建容器。
- `--no-deps`：不启动链接的服务。
- `<service_name>`：只启动指定的服务及其依赖。



**`docker-compose build`**：构建或重新构建服务中定义的镜像。

常用选项：

- `--no-cache`：构建镜像时不使用缓存。
- `--pull`：始终尝试拉取较新版本的镜像。
- `<service_name>`：只构建指定服务的镜像。



**`docker-compose down`**：停止并移除由 `up` 命令创建的容器、网络、卷和镜像

常用选项：

- `-v` 或 `--volumes`：移除在 `volumes` 部分定义的命名卷以及附加到容器的匿名卷。
- `--rmi <type>`：移除镜像。`type` 可以是 `all`（移除所有服务使用的镜像）或 `local`（只移除没有自定义标签的镜像）。
- `--remove-orphans`：移除未在 Compose 文件中定义但属于此项目的容器。



**`docker-compose ps`**：列出项目中的容器

常用选项：

- `-q`：只显示容器 ID



**`docker-compose logs`**：显示服务的日志输出。

常用选项：

- `-f` 或 `--follow`：持续跟踪日志输出。
- `--tail="N"`：显示最后 N 行日志。
- `<service_name>`：只显示指定服务的日志。



**`docker-compose pull`**：拉取服务所需的镜像。

常用选项：

- `--ignore-pull-failures`：如果拉取失败，则继续。
- `<service_name>`：只拉取指定服务的镜像。



**`docker-compose start <service_name...>`**：启动一个或多个已存在的、已停止的服务容器



**`docker-compose stop <service_name...>`**：停止一个或多个正在运行的服务容器，但不移除它们。

常用选项：

- `-t, --timeout TIMEOUT`：指定停止超时时间（默认为 10 秒）。



**`docker-compose restart <service_name...>`**：重启一个或多个服务容器。



**`docker-compose exec <service_name> <command>`**：在指定服务的正在运行的容器中执行一个命令。

常用选项：

- `-d`：分离模式，在后台运行命令。
- `-u, --user USER`：以指定用户运行命令。
- `-T`：禁用伪 TTY 分配。当 `stdin` 不是 TTY 时很有用。

- **示例**：`docker-compose exec web bash` (进入 web 服务的容器)



**`docker-compose config`**：验证并查看 Compose 文件的配置。

常用选项：

- `--quiet` 或 `-q`：只检查配置是否有效，不输出任何内容。

- `--services`：打印服务名称。

- `--volumes`：打印卷名称。

  

**`docker-compose rm <service_name...>`**：移除已停止的服务容器。

常用选项：

- `-f, --force`：强制移除。
- `-s, --stop`：在移除前先停止容器。
- `-v`：同时移除附加到容器的匿名卷。

- **示例**：`docker-compose rm web`



### 常用配置参数

#### **顶层键：**

**`version`**：

- **用途**：定义 Compose 文件格式的版本

- **示例**：`version: '3.8'`



**`services`**

- **用途**：**定义应用程序的各个服务**（容器）。这是 Compose 文件中最核心的部分。

- **示例**：

```
services:
  back_app:
    # ... 后端服务的配置 ...
  pgsql:
    # ... postgresql服务的配置 ...
```



**`networks`**

- **用途**：定义自定义网络，供服务连接。

- **示例**：

```yaml
networks:
  my_app_net:
    driver: bridge
```



**`volumes`**

- **用途**：定义命名卷，用于数据持久化。

- 示例

  ```yaml
  volumes:
    db_data:
      driver: local
  ```



#### **服务 (`services`) 下的常用配置**

**`image`**

- **用途**：指定用于创建容器的镜像。可以是 Docker Hub 或者私有仓库的镜像，也可以是使用Dockerfile构建的镜像
- **示例**：`image: postgres:16.1-alpine`、`image: wjkz:1.0`(这个是自己构建的镜像，要和 `build` 参数结合使用)



**`build`**

- **用途**：指定 Dockerfile 的路径或构建上下文，用于在 `docker-compose up --build` 或 `docker-compose build` 时构建镜像。
- **示例 (字符串)**：`build: .` (Dockerfile 在当前目录下)



**`container_name`**

- **用途**：指定一个自定义的容器名称，而不是 Compose 生成的默认名称。通常不推荐，因为可能导致名称冲突。
- **示例**：`container_name: my_custom_web_container`



**`working_dir`**

- **用途**：设置容器内的工作目录。
- **示例**：`working_dir: /app`



**`ports`**

- **用途**：将**容器端口映射到主机端口**。格式为 `"HOST_PORT:CONTAINER_PORT"` 或只指定容器端口（主机端口随机分配）。

- **示例**：

  ```YAML
  ports:
    - "8080:80"    # 将主机的 8080 端口映射到容器的 80 端口
    - "9000"       # 将容器的 9000 端口映射到主机的一个随机端口
    - "127.0.0.1:3000:3000" # 只在本地主机上映射
  ```



**`volumes`**

- **用途**：挂载卷，用于数据持久化或共享文件。可以是主机路径映射，也可以是命名卷。

- **格式**：

  - 主机路径：`HOST_PATH:CONTAINER_PATH` 或 `HOST_PATH:CONTAINER_PATH:ro` (只读)
  - 命名卷：`VOLUME_NAME:CONTAINER_PATH`

- **示例**：

  ```yaml
  volumes:
    - ./html:/usr/share/nginx/html  # 挂载主机当前目录下的 html 文件夹
    - db_data:/var/lib/mysql        # 挂载名为 db_data 的命名卷
    - /tmp/app_logs:/app/logs:ro    # 只读挂载
  ```



**`environment`**

- **用途**：设置环境变量。可以是一个列表或一个字典。

- **示例 (列表)：**

  ```YAML
  environment:
    - DEBUG=true
    - API_KEY=abcdef12345
  ```

- **示例 (字典)：**

  ```yaml
  environment:
    NODE_ENV: development
    POSTGRES_USER: myuser
  ```



**`env_file`**

- **用途**：从一个或多个文件加载环境变量。

- **示例**：

  ```YAML
  env_file:
    - ./common.env
    - ./api.env
  ```



**`depends_on`**

- **用途**：定义服务间的启动依赖关系。Compose 会按照依赖顺序启动服务。

- **注意**：这只保证依赖服务先启动，不保证依赖服务内部的应用程序已准备好接受连接。对于应用级别的就绪检查，通常需要使用 `healthcheck` 或其他机制。

- **示例**：

  ```yaml
  services:
      back_app:
  				# 	... 其他配置
          depends_on:
              - pgsql
              - minio
  ```



**`networks`**

- **用途**：**将服务连接到指定的网络**。如果未指定，服务会连接到默认网络。

- **示例**：

  ```YAML
  services:
      back_app:
         # ...其他配置
          networks:
              - network-backend-wjkz
  networks:
    network-backend-wjkz:
      ipam:
        driver: default
        config:
          - subnet: 172.40.0.0/16
  ```



**`command`**

- **用途**：覆盖容器镜像中定义的默认命令 (`CMD`)。

- **示例**：

  ```yaml
  services:
      back_app:
         # ...其他配置
  			command: python3 app.py
  ```



**`restart`**

- **用途**：定义容器的重启策略。
- **可选值**：
  - `no`：默认值，不重启。
  - `always`：总是重启。
  - `on-failure`：当容器以非零退出码退出时重启。可以指定最大重启次数，如 `on-failure:5`。
  - `unless-stopped`：总是重启，除非容器被手动停止。
- **示例**：`restart: always`



**`privileged`**

- **用途**：当一个服务的 `privileged` 参数设置为 `true` 时，它会给予该容器几乎所有宿主机的 root 权限。
