# 安装依赖
```bash
sudo apt install -y openssl curl
```
<!--more-->
## 创建操作目录
```bash
cd ~ && mkdir -p fisco && cd fisco
```
![image.png](/static/img/7e274b7811f9061a09fe1b91f1fee5f9.image.webp)
## 下载脚本
```bash
curl -#LO https://github.com/FISCO-BCOS/FISCO-BCOS/releases/download/v2.9.1/build_chain.sh && chmod u+x build_chain.sh
```
## 创建4节点
```bash
bash build_chain.sh -l 127.0.0.1:4 -p 30300,20200,8545
```
![image.png](/static/img/505d092e1e04e740f01a0ce2bbafc60e.image.webp)
<!--more-->
## 启动所有节点
```bash
bash nodes/127.0.0.1/start_all.sh
```
![image.png](/static/img/9fe412d60af3c56b87763c6a7d310837.image.webp)
## 检查进程
```bash
ps -ef | grep -v grep | grep fisco-bcos
```
![image.png](/static/img/ba678a8114064ca7530218c1eeea712c.image.webp)
## 检查日志输出
#### 查看node0连接的节点数
```bash
tail -f nodes/127.0.0.1/node0/log/log*  | grep connected
```
![image.png](/static/img/7dcf1e2114c1c3e43a48f547cbb34b3d.image.webp)
#### 检查是否共识
```bash
tail -f nodes/127.0.0.1/node0/log/log*  | grep +++
```
![image.png](/static/img/03c4e2475afba9da2ec136debbe1c69b.image.webp)
至此，FISCO BCOC联盟链单群组4节点搭建完成<br>
本文参考链接：https://fisco-bcos-documentation.readthedocs.io/zh_CN/latest/docs/installation.html