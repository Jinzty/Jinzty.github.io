配置docker环境
新建虚拟机，挂载虚拟硬盘，挂载boot2docker.iso镜像，固定分配boot2docker-data共享文件夹
虚拟机设置端口转发22，ssh连接上去
sudo vi /var/lib/boot2docker/profile
```
DOCKER_REMOTE=yes
DOCKER_TLS=no
```
sudo /etc/init.d/docker restart
端口转发2375，VScode安装插件docker配置连接
```
{
    "docker.host": "tcp://localhost:32375",
    "docker.tlsVerify": "0",
}
```
win10安装docker-client
不想安装docker-desktop，https://master.dockerproject.com/windows/x86_64/docker.zip下载解压配置环境变量path
设置环境变量DOCKER_HOST、如果前面没把tls关闭还需设置DOCKER_TLS_VERIFY、DOCKER_CERT_PATH
安装好可以直接执行docker指令docker ps
VScode安装插件Remote-Containers
在boot2docker-data共享目录下新建项目，Remote-Containers: Add Development Container Configuration Files，环境配置文件devcontainer.json
src目录下编写代码，debug界面按设置按钮，debuger配置文件launch.json
参考https://github.com/Jinzty/remote-go，配置完成
Remote-Containers: Reopen in Container
获取/编译镜像，运行容器挂载代码，连接容器
现在即可愉快的在docker容器环境内开发调试了

另外可以通过Remote Explorer直接连上已运行的其他容器
