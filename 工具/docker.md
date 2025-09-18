- [安装](#安装)
- [基本原理](#基本原理)
- [常用命令](#常用命令)
- [数据卷](#数据卷)
- [构建镜像](#构建镜像)
- [Docker Compose](#Docker%20Compose)

## 安装
```
2025/2/1按下列步骤成功安装
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
```
## 基本原理
![](images/docker_1.png)
## 常用命令
```shell
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

# 运行镜像
# -d让容器后台运行
# --name选项为容器起名字，必须唯一
# -p设置端口映射，宿主端口:容器端口
# -e设置环境变量
docker run -d --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123 mysql:5.7
# 简写
docker run -d mysql
```
## 数据卷
```shell
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
## 构建镜像
![](images/docker_3.png)


![](images/docker_2.png)
```shell
# 结尾的.代表Dockfile所在的目录
docker build -t myImage:1.0 .
```
## Docker Compose
```shell
# Docker Compose通过一个单独的docker-compose.yml文件来定义一组相关联的应用容器，帮助我们实现多个相互关联的Docker容器的快速部署
```
