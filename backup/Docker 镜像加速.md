## 镜像源
科大镜像：https://docker.mirrors.ustc.edu.cn/<br>
网易：https://hub-mirror.c.163.com/<br>
阿里云：https://<阿里云id>.mirror.aliyuncs.com<br>
Docker 官方加速器: https://registry.docker-cn.com 
<!--more-->
## 针对 Ubuntu14.04、Debian7Wheezy 等系统
* 编辑 /etc/default/docker 文件
```bash
DOCKER_OPTS="--registry-mirror=https://registry.docker-cn.com"
```
* 重启服务
```bash
 service docker restart
```
 ## 针对Ubuntu16.04+、Debian8+、CentOS7等系统
 *  编辑/etc/docker/daemon.json文件（如果没有就新建）
 ```bash
 {
        "registry-mirrors":[
                "https://hub-mirror.c.163.com/",
                "https://docker.mirrors.ustc.edu.cn/"
        ]
}
```
* 重新启动服务
```bash
systemctl daemon-reload
systemctl restart docker
```
## 检查加速器是否生效
```
docker info
```
* 出现如下内容即为生效

![image.png](/static/img/3217b6df7d0dd7c0880ad502f5be88b3.image.webp)
