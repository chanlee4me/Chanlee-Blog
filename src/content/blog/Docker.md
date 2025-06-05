参考文档：[docker 教程](https://www.runoob.com/docker/docker-tutorial.html)

<h1 id="zouJW">前言</h1>
<h2 id="MxBPU">Docker 的应用场景</h2>
1. web 应用的**自动化打包**、**发布**
2. <font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">自动化测试和持续集成、发布</font>
3. <font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">在服务型环境中部署和调整数据库或其他的后台应用。</font>

<h2 id="N7HoK">Docker 的优点</h2>
快速、一致地交付应用程序

+ <font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">您的开发人员在本地编写代码，并使用 Docker 容器与同事共享他们的工作。</font>
+ <font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">他们使用 Docker 将其应用程序推送到测试环境中，并执行自动或手动测试。</font>
+ <font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">当开发人员发现错误时，他们可以在开发环境中对其进行修复，然后将其重新部署到测试环境中，以进行测试和验证。</font>
+ <font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">测试完成后，将修补程序推送给生产环境，就像将更新的镜像推送到生产环境一样简单</font>



<h1 id="p9Vje">Docker 的架构</h1>
<h2 id="lYgpr">架构简介</h2>
Docker是基于 C/S 架构，使用远程 API 来管理和创建 Docker容器

Docker 容器通过镜像来创建，容器的镜像的关系：镜像-->类，容器-->对象

![Docker 架构示意图](https://cdn.nlark.com/yuque/0/2025/png/51312682/1748241603799-36587c96-f4f3-4e7f-b63b-bcccd2d409d2.png)

Docker 架构的工作流程：

**构建镜像：**使用`Dockerfile`来构建镜像

**推送镜像到注册表**：<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">将镜像上传到 Docker Hub 或私有注册表中</font>

**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">拉取镜像</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">：通过 </font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">docker pull </font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">从注册表中拉取镜像  </font>

**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">运行容器</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">：使用镜像创建并启动容器</font>

**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">管理容器</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">：使用 Docker 客户端命令管理正在运行的容器（例如查看日志、停止容器、查看资源使用情况等）。</font>

**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">网络与存储</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">：容器之间通过 Docker 网络连接，数据通过 Docker 卷或绑定挂载进行持久化。</font>

<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">  
</font>

<h2 id="t3IcK">核心组件及工作机制</h2>
<h3 id="0f535055">**<font style="color:rgb(51, 51, 51);">Docker 客户端（Docker Client）</font>**</h3>
<font style="color:rgb(51, 51, 51);">Docker 客户端是用户与 Docker 守护进程</font>**<font style="color:rgb(51, 51, 51);">交互</font>**<font style="color:rgb(51, 51, 51);">的</font>**<font style="color:rgb(51, 51, 51);">命令行界面</font>**<font style="color:rgb(51, 51, 51);">（CLI）。用户通过 Docker CLI 向守护进程发送命令，由守护进程执行相应操作</font>

**<font style="color:rgb(51, 51, 51);">功能：</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">允许用户使用命令与 Docker 守护进程通信，如创建容器、构建镜像、查看容器状态等。</font>

**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">交互方式</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">：常用的命令行工具是 </font>`<font style="color:rgb(51, 51, 51);">docker</font>`<font style="color:#8A8F8D;background-color:rgb(250, 252, 253);">（Docker 客户端与 Docker 守护进程之间通过 REST API 或 Unix 套接字通信）</font>

**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">常用的命令：</font>**

+ `<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">docker run</font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">：运行容器。</font>
+ `<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">docker ps</font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">：列出正在运行的容器。</font>
+ `<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">docker build</font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">：构建 Docker 镜像。</font>
+ `<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">docker exec</font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">：在容器中执行命令。</font>

<h3 id="P4v0t">**<font style="color:rgb(51, 51, 51);">Docker 守护进程（Docker Daemon）</font>**</h3>
Docker Daemon 用于<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">负责</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">管理容器生命周期</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">、</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">构建镜像</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">、</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">分发镜像</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">等任务</font>

**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">功能</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">：</font>

+ <font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">启动和停止容器。</font>
+ <font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">构建、拉取和推送镜像。</font>
+ <font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">管理容器的网络和存储。</font>
+ <font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">启动、停止、查看容器日志等。</font>
+ <font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">与 Docker 注册表进行通信，管理镜像的存储与分发。</font>

<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">启动</font>Docker Daemon：`sudo systemctl start docker`



<h3 id="Z6MBu">**<font style="color:rgb(51, 51, 51);">Docker 引擎 API（Docker Engine API）</font>**</h3>
<font style="color:rgb(51, 51, 51);">Docker 引擎 API 是 </font>**<font style="color:rgb(51, 51, 51);">Docker 提供的 RESTful 接口</font>**<font style="color:rgb(51, 51, 51);">，允许</font>**<font style="color:rgb(51, 51, 51);">外部客户端</font>**<font style="color:rgb(51, 51, 51);">与 Docker </font>**<font style="color:rgb(51, 51, 51);">守护进程</font>**<font style="color:rgb(51, 51, 51);">进行</font>**<font style="color:rgb(51, 51, 51);">通信</font>**<font style="color:rgb(51, 51, 51);">。</font><font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">API 提供了 HTTP 请求的接口，支持跨平台调用</font>

**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">功能</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">：</font>

+ <font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">向 Docker 守护进程发送 HTTP 请求，实现容器、镜像的管理。</font>
+ <font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">提供 RESTful 接口，允许通过编程与 Docker 进行交互。</font>

<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">可</font><font style="color:rgb(51, 51, 51);">以通过 </font>`<font style="color:rgb(51, 51, 51);">curl</font>`<font style="color:rgb(51, 51, 51);"> 或其他 HTTP 客户端访问 Docker 引擎 API。例如，查询当前 Docker 守护进程的版本：</font>

`<font style="color:rgb(51, 51, 51);">curl --unix-socket /var/run/docker.sock </font>[http://localhost/version](http://localhost/version)`



<h3 id="xCaEZ">**<font style="color:rgb(51, 51, 51);">Docker 容器（Docker Containers）</font>**</h3>
<font style="color:rgb(51, 51, 51);">容器是 </font>**<font style="color:rgb(51, 51, 51);">Docker 的执行环境</font>**<font style="color:rgb(51, 51, 51);">，它是轻量级、独立且可执行的软件包。容器是从 Docker 镜像启动的，</font>**<font style="color:rgb(51, 51, 51);">包含了运行某个应用程序所需的一切</font>**<font style="color:rgb(51, 51, 51);">——</font>**<font style="color:rgb(51, 51, 51);">从操作系统库到应用程序代码</font>**<font style="color:rgb(51, 51, 51);">。容器在运行时与其他容器和宿主机</font>**<font style="color:rgb(51, 51, 51);">共享操作系统内核</font>**<font style="color:rgb(51, 51, 51);">，但容器之间的</font>**<font style="color:rgb(51, 51, 51);">文件系统和进程是隔离</font>**<font style="color:rgb(51, 51, 51);">的。</font>

**<font style="color:rgb(51, 51, 51);">功能</font>**<font style="color:rgb(51, 51, 51);">：</font>

+ <font style="color:rgb(51, 51, 51);">提供独立的运行环境，确保应用程序在不同的环境中具有一致的行为。</font>
+ <font style="color:rgb(51, 51, 51);">容器是临时的，通常在任务完成后被销毁。</font>

<font style="color:rgb(51, 51, 51);">容器的生命周期是由 Docker 守护进程管理的。</font>**<font style="color:rgb(51, 51, 51);">容器可以在任何地方运行</font>**<font style="color:rgb(51, 51, 51);">，因为它们不依赖于底层操作系统的配置，所有的运行时依赖已经封装在镜像中。</font>

<h3 id="JUrnQ">Docker 镜像**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">（Docker Images）</font>**</h3>
<font style="color:rgb(51, 51, 51);">Docker 镜像可以理解为</font>**<font style="color:rgb(51, 51, 51);">静态的容器</font>**<font style="color:rgb(51, 51, 51);">，每个镜像都</font>**<font style="color:rgb(51, 51, 51);">包含了应用程序运行所需的一切环境</font>**

**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">功能</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">：</font>

+ <font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">镜像是构建容器的基础，每个容器实例化时都会使用镜像。</font>
+ <font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">镜像是只读的，不同容器使用同一个镜像时，容器中的文件系统层是独立的。</font>

<font style="color:rgb(51, 51, 51);">Docker 镜像可以通过 </font>`<font style="color:rgb(51, 51, 51);">docker pull</font>`<font style="color:rgb(51, 51, 51);"> </font>**<font style="color:rgb(51, 51, 51);">从 Docker Hub 或私有注册表拉取</font>**<font style="color:rgb(51, 51, 51);">，也可以</font>**<font style="color:rgb(51, 51, 51);">通过</font>**`**<font style="color:rgb(51, 51, 51);">docker build</font>**`**<font style="color:rgb(51, 51, 51);">从Dockerfile 构建</font>**<font style="color:rgb(51, 51, 51);">。</font>

<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">拉取 Ubuntu 镜像：</font>`docker pull ubuntu`

<font style="color:rgb(51, 51, 51);"></font>

<h3 id="ZlG80">**<font style="color:rgb(51, 51, 51);">Docker 仓库（Docker Registries）</font>**</h3>
Docker 仓库就是用来**存储镜像的地方**。**最常用的公共仓库是****<font style="color:rgb(51, 51, 51);"> Docker Hub</font>**<font style="color:rgb(51, 51, 51);">，用户可以从中拉取镜像，也可以将自己的镜像分享给别人。除了公共仓库，用户可以部署自己的私有仓库来管理企业内部的镜像</font>

<font style="color:rgb(51, 51, 51);">推送镜像到 Docker Hub：</font>

`<font style="color:rgb(51, 51, 51);">docker push <username>/<image_name></font>`

<font style="color:rgb(51, 51, 51);"></font>

<h3 id="Pghsh">**<font style="color:rgb(51, 51, 51);">Docker Compose</font>**</h3>
`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">Docker Compose</font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);"> 是一个</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">用于定义和运行多容器 Docker 应用</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">的工具。通过 Compose，用户可以</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">使用</font>**`**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">docker-compose.yml</font>**`**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);"> 配置文件定义多个容器</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">（服务），并可以</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">通过一个命令启动这些容器</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">。</font>

**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">功能</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">：</font>

+ <font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">定义和运行多个容器组成的应用。</font>
+ <font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">通过 YAML 文件来配置应用的服务、网络和卷等。</font>

<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">创建一个简单的 </font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">docker-compose.yml</font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);"> 文件来配置一个</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">包含 Web 服务和数据库服务的应用</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">：</font>

```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "8080:80"
  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: example
```

<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">启动 Compose 定义的所有服务：</font>

```yaml
docker-compose up
```

  


<h3 id="docker-swarm">**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">Docker Swarm</font>**</h3>
<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">Docker Swarm 是 Docker 提供的</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">集群管理和调度工具</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">。它允许</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">将多个 Docker 主机（节点）组织成一个集群</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">，并</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">通过 Swarm </font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">集群管理工具</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">来调度和管理容器</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">。Swarm 可以实现容器的</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">负载均衡、高可用性和自动扩展</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">等功能。</font>

<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">初始化 Swarm 集群：</font>

`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">docker swarm init</font>`

<h3 id="4a15c7f1">**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">Docker 网络（Docker Networks）</font>**</h3>
<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">Docker 网络</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">允许容器之间相互通信</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">，并</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">与外部世界进行连接</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">。Docker 提供了多种网络模式来满足不同的需求，如 </font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">bridge</font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);"> 网络（默认）、</font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">host</font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);"> 网络和 </font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">overlay</font>`<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);"> 网络等。</font>

**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">功能</font>**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">：</font>

+ <font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">管理容器间的网络通信。</font>
+ <font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">支持不同的网络模式，以适应不同场景下的需求。</font>

<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">创建一个自定义网络并将容器连接到该网络：</font>

```dockerfile
docker network create my_network
docker run -d --network my_network ubuntu
```

<h3 id="c998bfd6">**<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">Docker 卷（Docker Volumes）</font>**</h3>
<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">是一种数据持久化机制，允许数据在容器之间共享，并且独立于容器的生命周期</font>

<font style="color:rgb(51, 51, 51);background-color:rgb(250, 252, 253);">创建并挂载卷：</font>

```dockerfile
docker volume create my_volume
docker run -d -v my_volume:/data ubuntu
```

<h1 id="eYSAW">Docker 相关命令</h1>
<h2 id="SStia">容器</h2>
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

<h2 id="gjE3X">镜像</h2>
`docker search`检索镜像

`docker pull`拉取镜像

`docker images`列出镜像

`docker image rm`删除镜像

`docker save`导出镜像

`docker load`导入镜像

**Dockerfile 相关：**

`docker build`构建镜像

`docker run`运行镜像

`COPY`复制文件

`ADD`高级复制

`CMD`容器启动命令

`ENV`环境变量

`EXPOSE`暴露端口

<h2 id="MfcN7">服务</h2>
`docker version`查看 docker 详细版本信息

`docker -v`查看docker简要信息

`ststemctl start docker`启用docker

`ststemctl stop docker`关闭 docker

`ststemctl enable docker` 设置开机启动

`service docker restart` 重启 docker 服务

`service docker stop`关闭 docker 服务（这个命令和上方的 ststemctl stop 的效果一样，不过它是历史遗留命令）

