---
layout: post
title:  "使用VScode的Remote-Containers远程连接docker开发"
categories: Remote-Containers
tags: VScode docker
author: Jinzty
---
<h1>配置docker环境</h1>

有的话直接可以跳过，最好有个远程机器上部署了docker。如果没有docker环境，在win下使用boot2docker镜像虚拟机快速搭建

新建虚拟机，挂载虚拟硬盘，挂载boot2docker.iso镜像，固定分配boot2docker-data共享文件夹

虚拟机设置端口转发22，ssh连接上去

sudo vi /var/lib/boot2docker/profile
```
DOCKER_REMOTE=yes
DOCKER_TLS=no
```
sudo /etc/init.d/docker restart

设置端口转发2375，VScode安装插件docker配置连接
```
{
    "docker.host": "tcp://localhost:32375",
    "docker.tlsVerify": "0",
}
```
<h1>win10安装docker-client</h1>

不想安装docker-desktop，直接下载client包https://master.dockerproject.com/windows/x86_64/docker.zip

解压并配置环境变量path，设置环境变量DOCKER_HOST、如果前面没把tls关闭还需设置DOCKER_TLS_VERIFY、DOCKER_CERT_PATH

安装好可以直接cmd下执行docker指令docker ps

<h1>VScode远程连接到容器中开发</h1>

VScode安装插件Remote-Containers

在boot2docker-data共享目录下新建项目，Remote-Containers: Add Development Container Configuration Files，环境配置文件devcontainer.json

src目录下编写代码，debug界面按设置按钮，debuger配置文件launch.json，参考https://github.com/Jinzty/remote-go

Remote-Containers: Reopen in Container，获取/编译镜像，运行容器挂载代码，连接容器

现在即可愉快的在docker容器环境内开发调试了

另外可以通过Remote Explorer直接连上已运行的其他容器，操作还有指令都比docker exec去连容器方便许多
