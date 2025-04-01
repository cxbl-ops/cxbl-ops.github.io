# 一，nginx的安装
## 环境准备
ubuntu云服务器一台(虚拟机也可)

<!-- more -->

# 使用apt库进行安装
```bash
#默认安装最新版
apt install nginx -y
```
# 二、SSL 证书部署
## 在 nginx 目录新建 cert 文件夹存放证书文件。
```bash
cd /usr/local/nginx
mkdir cert
```
## 将申请的证书上传至cert文件夹
```bash
scp /Users/yourname/Downloads/ssl.pem root@xxx.xx.xxx.xx:/usr/local/nginx/cert/
scp /Users/yourname/Downloads/ssl.key root@xxx.xx.xxx.xx:/usr/local/nginx/cert/
```

:::note{title="注"}
scp [本地文件路径，可以直接拖文件至终端里面] [<服务器登录名>@<服务器IP地址>:<服务器上的路径>]<br>
证书可以在阿里云，cloudflare获得
:::
# 三、Nginx.conf 配置
## 打开配置文件
```
vim /usr/local/nginx/conf/nginx.conf 
```
## 配置 https server。注释掉之前的 http server 配置，新增 https server
```
server {
    # 服务器端口使用443，开启ssl, 
    listen       443 ssl;
    # 域名，多个以空格分开
    server_name  xxx.com www.xxx.com;
    
    # ssl证书地址
    ssl_certificate     /usr/local/nginx/cert/ssl.pem;  # pem文件的路径
    ssl_certificate_key  /usr/local/nginx/cert/ssl.key; # key文件的路径
    
    # ssl验证相关配置
    ssl_session_timeout  5m;    #缓存有效期
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;    #加密算法
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;    #安全链接可选的加密协议
    ssl_prefer_server_ciphers on;   #使用服务器端的首选算法

    location / {
        root   html;
        index  index.html index.htm;
    }
}
```
### 将 http 重定向 https。
```
server {
    listen       80;
    server_name  hack520.com www.xxxx.com;
    return 301 https://$server_name$request_uri;
}
```
# 四、重启 nginx
```bash
/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
```
或
```bash
service nginx restart
```