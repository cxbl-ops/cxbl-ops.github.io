# 前言
Docker 是一种开源的容器化平台，用于开发、部署和运行应用程序。它允许开发者将应用程序和其依赖项封装在一个称为容器的独立单元中，这些容器可以在任何支持Docker的环境中运行，而无需担心环境差异或依赖问题。

<!-- more -->

## 两种命令的区别
service 命令通常用于管理传统的SysV初始化脚本管理的服务。这些初始化脚本是一种早期的服务管理方式，已被现代的 systemd 替代。<br>
systemctl 是用于管理 systemd 初始化系统的服务的命令。systemd 是现代Linux系统中常见的初始化系统，它提供了更强大的服务管理功能，并支持更复杂的依赖关系和单元文件配置。
:::warning{title="注意"}
有些linux发行版可能只支持其中一种，或者两种都支持。
:::

<!-- more -->

## 启动docker服务
```
service docker start || systemctl start docker
```
## 关闭docker服务
```
service docker stop || systemctl stop docker
```
## 拉取镜像(image)
```
docker pull ubuntu  #镜像名 ubuntu
```
## 启动docker某个image（镜像）的 container（容器）
```
docker run -it ubuntu /bin/bash
```

- docker run：启动container
- ubuntu：你想要启动的image
- -t：进入终端
- -i：获得一个交互式的连接，通过获取container的输入
- /bin/bash：在container中启动一个bash shell

:::warning{title="注意"}
加入-d会使容器后台运行，-itd并不会进入容器内部
:::
## 效果
```
root@CT108:~# docker run -t -i ubuntu /bin/bash
root@c973fa951900:/#
```
## 退出container
```
exit || Ctrl+D 
```
## 查看container
```
docker ps #查看运行中的container
docker ps -a #查看所有容器，包括已经退出的
```
## 查看镜像
```
docker images
```

<!-- more -->

## 再次进入container
### 1.已经完全退出的container
```
docker exec -it ubuntu /bin/bash
```
### 2.在后台运行的container
```
docker attach ubuntu
```
## 提交镜像
```
docker commit -m "install haddop" 6c2215199d42 hadoop
# -m "install hadoop" 是一个选项，用于提供一个提交消息
6c2215199d42 是要创建镜像的容器的 ID。
hadoop 是为新镜像指定的名称。
```
