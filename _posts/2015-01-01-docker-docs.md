---
layout: post
author: charlie
tags: [docs]
title: docker-docs
---
# Docker Docs

Dockerfile: [https://docs.docker.com/build/building/multi-stage/#use-multi-stage-builds]()

Docker Compose: [https://docs.docker.com/compose/gettingstarted/]()

## Install

[https://www.docker.com/](https://www.docker.com/)

> mac 使用orbstack可以轻量化使用docker，缺点是目前还不能以本地镜像重新构建镜像。

## Docker hub

[https://hub.docker.com/](https://hub.docker.com/)

私有化部署Docker Hub可以使用Harbor等开源容器镜像仓库来实现，以下是使用Harbor进行私有化部署的一般步骤：

### 准备工作

- **安装Docker和Docker Compose**：在部署Harbor的服务器上，需要先安装Docker和Docker Compose。可以根据服务器的操作系统，参考官方文档进行安装。
- **准备服务器**：确保服务器有足够的资源，包括CPU、内存和存储，以满足实际的使用需求。
- **配置域名和SSL证书**：如果希望通过HTTPS访问Harbor，需要准备一个域名，并为其申请SSL证书，将证书文件放置在服务器的指定目录。

### 下载和配置Harbor

- **下载Harbor离线安装包**：从Harbor官方网站（https://goharbor.io/）下载适合你环境的离线安装包，通常是一个tar.gz格式的文件。
- **解压安装包**：使用命令行工具，将下载的安装包解压到指定目录，如 `/opt/harbor`。
- **配置Harbor**：进入解压后的目录，编辑 `harbor.yml`文件，根据实际需求修改配置参数，主要包括：
  - **hostname**：设置Harbor的访问域名，如果使用IP访问，也可以设置为服务器的IP地址。
  - **http/https**：配置访问协议，如果使用HTTPS，需要指定SSL证书和密钥的路径。
  - **database**：配置数据库相关参数，默认使用内置的MySQL，也可以配置为外部数据库。
  - **storage**：设置镜像存储的方式和路径，可选择本地存储或分布式存储等。

### 安装和启动Harbor

- **执行安装脚本**：在Harbor解压目录下，执行安装脚本 `install.sh`，安装过程中会自动拉取所需的镜像并进行配置。
- **启动Harbor**：安装完成后，可以使用Docker Compose启动Harbor服务，命令为 `docker-compose up -d`，该命令会在后台启动Harbor的各个组件，包括Web界面、Registry、数据库等。

### 访问和使用Harbor

- **登录Harbor**：在浏览器中输入配置的域名或IP地址，访问Harbor的Web界面，使用默认的用户名 `admin`和密码 `Harbor12345`登录，首次登录后可以修改密码。
- **创建项目和上传镜像**：登录后可以创建项目，用于组织和管理镜像。在本地安装了Docker的客户端机器上，通过命令行登录到私有化的Harbor仓库，然后可以将本地镜像推送到Harbor仓库中。例如：

```bash
# 登录Harbor仓库
docker login your_harbor_url
# 标记本地镜像
docker tag your_image:tag your_harbor_url/project/your_image:tag
# 推送镜像到Harbor
docker push your_harbor_url/project/your_image:tag
```

### 配置与维护

- **用户和权限管理**：在Harbor的Web界面中，可以创建用户、用户组，并为不同的项目设置访问权限，实现对镜像仓库的精细管理。
- **镜像复制和同步**：如果有多个Harbor仓库或需要与其他镜像仓库进行数据同步，可以配置镜像复制策略，实现镜像在不同仓库之间的自动复制和同步。
- **备份与恢复**：定期对Harbor的数据进行备份，包括数据库、镜像文件等，以防止数据丢失。可以使用工具或脚本实现自动化备份，在出现故障时能够快速恢复。

## 常用命令

```bash
# 查看、删除容器
docker ps [-a] [| grep keyword_to_search]
docker rm container_name/id
# 查看、删除镜像
docker images [| grep keyword_to_search]
docker rmi image_name/id
# 数据复制
docker cp /path/of/data  container_name/id:/new/path/of/data
# 删除缓存
docker builder prune
# 构建容器
docker run -d -p 8000:8000 -v ./:/workspace -name container_name -it image_name
```

### docker 运行示例

```
docker run --gpus all \
-v ${PWD}:/workspace \
--shm-size=8gb \
--ipc=host \
--ulimit memlock=-1 \
--ulimit stack=67108864 \
-p 18080:18080 \
-it tensorrt:trt8.5.1-cu11.3-ubuntu2004
```

## Win Docker Desktop

### 迁移docker镜像

```python
wsl –export docker-desktop docker-desktop.tar
wsl –export docker-desktop-data docker-desktop-data.tar
wsl –unregister docker-desktop wsl –unregister docker-desktop-data
wsl –import docker-desktop-data F:-desktop-data docker-desktop-data.tar
wsl –import docker-desktop F:-desktop docker-desktop.tar
```

### GPU测试

```python
service docker start
docker run –-gpus all nvcr.io/nvidia/k8s/cuda-sample:nbody nbody -gpu -benchmark
```

### 重置密码

```python
ubuntu2004 config –default-user root
passwd torch
ubuntu2004 config –default-user torch
```

# k8s等集群服务部署

## Docker swarn

- Docker公司使用Go语言开发的docker集群管理平台
- 将一群docker宿主机变成一个单一的虚拟主机
- 与k8s类似，功能比k8s少
- 调度最小单位少容器，而k8s是pod

## k8s

- Pod
  - 在k8s中代表pod中可以运行一个或者多个容器
  - pods之间通过localhost通信
- Service
  - 一旦service被创建，k8s会为其分配一个集群内唯一的IP，称为ClusterIP，而且在service的整个生命周期，ClusterIP不会发生改变
  - ClusterIP是一个虚拟的ip地址，无法被ping到，仅限于k8s内使用
  - Service对客户端屏蔽了底层pod的寻址过程，并且由kube-proxy进程将对Service的请求转发到具体的pod上，由具体的调度算法绝地搞。这样一来就实现了负载均衡。
- Label
  - label本质上来说是一个键值对，具体的值由用户决定。
  - label是一个标签，可以打在pod上，也可以打到service上。
  - label与标记的资源上一个一对多的关系
- Replica Set
  - 一旦被创建，集群就会定期检查当前存活的Pod数量，如果多了，集群就会停掉一些，如果少， 就会创建一些。保证集群中始终有我们设置的副本数量的pods中运行。
- yaml文件
  - k8s使用yaml来编排部署应用
  - 编写
    - 缩进表示层级关系
    - 使用空格缩进，不支持制表符tab缩进
    - 通常开头缩进两格
    - 关键字后缩进一格，比如，冒号与逗号后面需要缩进一个字符
    - #表示注释
