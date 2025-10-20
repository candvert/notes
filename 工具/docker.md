- [安装](#安装)
	- [windows安装](#windows安装)
	- [ubuntu安装](#ubuntu安装)
- [基本原理](#基本原理)
- [常用命令](#常用命令)
- [镜像](#镜像)
- [容器](#容器)
- [仓库](#仓库)
- [构建镜像](#构建镜像)
- [Docker Compose](#Docker%20Compose)
- [数据卷](#数据卷)

## 安装
### Windows安装
对于 Windows，使用 Docker 有两种方式：
- 通过官方的 Docker Desktop 
- 在 wsl 中使用 docker
Docker Desktop 实际也是使用 wsl，但是提供了一个图形化界面
Docker Desktop 在安装之前，必须启用虚拟化技术，可以通过任务管理器的”性能“选项卡查看”虚拟化“状态是否为”已启用“，还需在 Windows 上启用 WSL 2 功能
需要以管理员身份安装 Docker Desktop
Docker Desktop 下载地址：https://www.docker.com/products/docker-desktop/
### ubuntu安装
```sh
# 卸载所有可能冲突的包：
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done

# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo systemctl start docker
sudo systemctl status docker
sudo docker run hello-world

完整步骤
按照官网的步骤进行安装（有的步骤会出现问题，需要多试几次）：
https://docs.docker.com/engine/install/ubuntu/

安装完后创建/etc/docker/daemon.json文件，写入下面内容配置镜像源，安装便完成了
{
    "registry-mirrors": [
        "http://hub-mirror.c.163.com",
        "https://mirrors.tuna.tsinghua.edu.cn",
        "http://mirrors.sohu.com",
        "https://ustc-edu-cn.mirror.aliyuncs.com",
        "https://ccr.ccs.tencentyun.com",
        "https://docker.m.daocloud.io",
        "https://docker.awsl9527.cn"
    ]
}





卸载：
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras

sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd

sudo rm /etc/apt/sources.list.d/docker.list
sudo rm /etc/apt/keyrings/docker.asc
```
## 基本原理
![](/images/docker_1.png)
## 常用命令
```sh
# 从仓库拉取镜像
docker pull mysql
# 查看本地镜像
docker images
# 删除镜像
docker rmi mysql
# 运行镜像
docker run mysql
# 后台运行镜像
docker run -d mysql


# 运行镜像
# -d让容器后台运行
# --name选项为容器起名字，必须唯一
# -p设置端口映射，宿主端口:容器端口
# -e设置环境变量
docker run -d --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123 mysql:5.7
# 简写
docker run -d mysql


# 查看运行中的容器
docker ps
# 查看所有容器
docker ps -a
# 查看容器的日志
docker logs mysql
# 实时输出容器的日志
docker logs -f mysql
# 进入容器的环境
docker exec -it mysql bash
# 查看容器详情
docker inspect mysql
# 删除容器（不加-f参数只能删除已停止的容器）
docker rm mysql
# 停止正在运行的容器
docker stop mysql
# 重新启动停止的容器
docker start mysql


# 查看所有容器
docker images
# 构建容器
docker build -t YOUR_DOCKER_USERNAME/docker-quickstart .
# 指定版本
docker tag <YOUR_DOCKER_USERNAME>/docker-quickstart <YOUR_DOCKER_USERNAME>/docker-quickstart:1.0
# 推送到 Docker Hub
docker push <YOUR_DOCKER_USERNAME>/docker-quickstart:1.0
```
## 镜像
镜像有两个重要原则：
- 镜像是不可变的。一旦创建，就无法修改。您只能创建新镜像或在其上进行更改
- 镜像由层组成。每一层代表一组文件系统变更，包括添加、删除或修改文件

Docker 镜像由多个只读层（read-only layers）堆叠而成，每个层对应 Dockerfile 中的一条指令（如FROM、COPY、RUN），记录文件系统的增量变化

可以将 Docker Hub 上现成的各种 Docker 官方镜像（比如 Python、Node.js）用作起点并添加自己的文件和配置

搜索并拉取镜像：
```sh
docker search docker/welcome-to-docker
docker pull docker/welcome-to-docker
```
列出下载的镜像：
```sh
docker image ls
```
列出镜像的层：
```sh
docker image history docker/welcome-to-docker
```
## 容器
启动容器时，您会将容器的一个端口暴露到您的计算机上
启动容器：
```sh
docker run -d -p 8080:80 docker/welcome-to-docker
```
对于此容器，可在端口 8080 上访问
验证容器是否已启动并正在运行：
```sh
docker ps
```
列出所有容器：
```sh
docker ps -a
```
停止容器：
```sh
# 首先运行 docker ps 获取容器的 ID
# 然后输入 docker stop 命令并提供 ID
docker stop <the-container-id>
```
## 仓库
Docker Hub 是一个任何人都可以使用的公共镜像仓库，也是默认的镜像仓库

registry 和 repository 不同，Docker Hub 是一个 registry，您可以在 Docker Hub 免费版创建一个私有 repository 和无限多个公共 repository。每个 repository 可以包含多个 image
## 构建镜像
Dockerfile 是一个用于创建容器镜像的文本文档。它为镜像构建器提供了有关要运行的命令、要复制的文件、启动命令等的说明。

例如，以下 Dockerfile 将生成一个可运行的 Python 应用程序：
```sh
FROM python:3.13
WORKDIR /usr/local/app

# 安装依赖
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

# 复制源代码
COPY src ./src
EXPOSE 8080

# 设置用户以便容器不会以 root 用户运行
RUN useradd app
USER app

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8080"]
```

Dockerfile 中一些最常见的指令包括：
- `FROM <image>`：指定基础镜像
- `WORKDIR <path>`：指定工作目录或文件复制和命令执行的目录
- `COPY <host-path> <image-path>`：从主机复制文件到映像中
- `RUN <command>`：告诉构建器运行指定的命令
- `ENV <name> <value>`：设置容器将使用的环境变量
- `EXPOSE <port-number>`：暴露端口
- `USER <user-or-uid>`：设置默认用户
- `CMD ["<command>", "<arg1>"]`：容器将运行的默认命令

构建镜像：
```sh
docker build -t YOUR_DOCKER_USERNAME/docker-quickstart .
```
## Docker Compose
容器的最佳实践之一是，每个容器应该只做一件事，并且做好它

使用 Docker Compose 可以协调并运行多个容器

使用 Docker Compose，您可以在一个 YAML 文件中定义所有容器及其配置

一个 compose.yaml 文件示例：
```yaml
# ​​version​​：指定所使用的 Compose 版本
# ​​services​​：用于定义应用中的各个容器服务。每个服务都有一个自定义的名称（如 web, db）
# ​​networks & volumes​​：定义服务之间通信所需的网络和用于数据持久化的数据卷
# ​​image​​：指定服务使用的镜像。如果本地不存在，Compose 会尝试拉取它
# ​​build​​：如果服务需要从 Dockerfile 构建镜像，则使用此选项指定构建上下文路径
# container_name​​：为容器指定一个自定义名称，而非自动生成
# depends_on​​：明确指定服务之间的依赖关系，确保所依赖的服务先启动
# ports​​：将容器端口映射到宿主机端口，格式为 "宿主机端口:容器端口"
# networks​​：指定服务要加入的网络
# volumes​​：用于挂载数据卷，实现数据持久化或宿主机与容器之间的数据共享
# driver: bridge，指定了该网络使用的驱动程序为bridge（桥接模式）。这是Docker中最常见的网络驱动




# version: '3.8'

services:
  nginx:
    image: nginx:latest
	# container_name: nginx_host1
	ports:
	  - 8081:80
	volumes:
	  - /opt/nginx:/opt/nginx/html
    # networks:
    #   - mynetwork

  webapp:
    build: .
    ports:
      - 8000:5000
    volumes:
      - .:/code
    depends_on:
      - nginx
    # networks:
    #   - mynetwork

# networks:
#   mynetwork:
#     driver: bridge
```
同一个 compose.yaml 文件中定义的容器都会自动加入同一个子网
启动容器：
```sh
docker compose up -d
docker compose up -d --build
```
停止并删除容器：
```sh
docker compose down
# 并删除卷
docker compose down --volumes
```
停止容器：
```sh
docker compose stop
```
让停止的容器继续运行：
```sh
docker compose start
```
## 数据卷
```sh
# 数据卷（volume）是一个虚拟目录，是容器内目录与宿主机目录之间映射的桥梁

# 执行docker run命令时，使用-v 数据卷:容器内目录便可完成数据卷挂载
# 当创建容器时，如果挂载的数据卷不存在，会自动创建数据卷
# 自动创建的数据卷会挂载到/var/lib/docker/volumes/下
# 可以使用-v 本地目录:容器内目录完成本地目录挂载，本地目录需以/或./开头，否则会被识别为数据卷
docker run -v html:/usr/share/nginx/html nginx
docker run -v /nginx_temp:/usr/share/nginx/html nginx

# 创建数据卷
docker volume create
# 查看所有数据卷
docker volume ls
# 删除指定数据卷
docker volume rm
# 查看某个数据卷的详细信息
docker volume inspect
# 清除数据卷
docker volume prune
```
## compose示例程序
[带有 Rust 后端和 Postgres 数据库的示例 React 应用程序](https://github.com/docker/awesome-compose/tree/master/react-rust-postgres)

项目结构：
```go
.
├── backend
│   ├── Dockerfile
│   ...
├── compose.yaml
├── frontend
│   ├── ...
│   └── Dockerfile
└── README.md
```
compose.yaml
```yaml
name: react-rust-postgres
services:
  frontend:
    build:
      context: frontend
      target: development
    networks:
      - client-side
    ports:
      - 3000:3000
    volumes:
      - ./frontend/src:/code/src:ro

  backend:
    build:
      context: backend
      target: development
    environment:
      - ADDRESS=0.0.0.0:8000
      - RUST_LOG=debug
      - PG_DBNAME=postgres
      - PG_HOST=db
      - PG_USER=postgres
      - PG_PASSWORD=mysecretpassword
    networks:
      - client-side
      - server-side
    volumes:
      - ./backend/src:/code/src
      - backend-cache:/code/target
    depends_on:
      - db

  db:
    image: postgres:12-alpine
    restart: always
    environment:
      - POSTGRES_PASSWORD=mysecretpassword
    networks:
      - server-side
    ports:
      - 5432:5432
    volumes:
      - db-data:/var/lib/postgresql/data

networks:
  client-side: {}
  server-side: {}

volumes:
  backend-cache: {}
  db-data: {}
```