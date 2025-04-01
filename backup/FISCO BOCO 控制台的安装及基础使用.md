# 一，安装依赖
- 安装java(推荐14，实测11也可用)
```bash
apt install -y default-jdk
```
<!--more-->
- 获取控制台并返回fisco目录
```bash
cd ~/fisco && curl -LO https://github.com/FISCO-BCOS/console/releases/download/v2.9.2/download_console.sh && bash download_console.sh
```
![image.png](/static/img/e46cb6a535e23cc7898e38a55e4477ca.image.webp)
:::tip{title="提示"}
如无法连接到github，则使用 cd ~/fisco && curl -#LO https://gitee.com/FISCO-BCOS/console/raw/master-2.0/tools/download_console.sh && bash download_console.sh
:::
- 拷贝控制台配置文件

若节点未采用默认端口，请将文件中的20200替换成节点对应的channel端口。
```bash
cp -n console/conf/config-example.toml console/conf/config.toml
```
<!--more-->
- 配置控制台证书
```bash
cp -r nodes/127.0.0.1/sdk/* console/conf/
```
# 二，启动并使用控制台
- 启动
```
cd ~/fisco/console && bash start.sh
```
![image.png](/static/img/fa545df3076b75475f00eff8b1089e23.image.webp)
- 用控制台获取信息
![image.png](/static/img/f23629e46fffa83369379a6260268e7c.image.webp)
- 部署HelloWorld合约
- 
![image.png](/static/img/d727085f1cec63fd5c9d684d644ff662.image.webp)

:::tip{title="提示"}
合约存储在console/contracts/solidity/文件夹中
:::
至此，控制台搭建完毕<br>
本文链接源自https://fisco-bcos-documentation.readthedocs.io/zh_CN/latest/docs/installation.html#id8