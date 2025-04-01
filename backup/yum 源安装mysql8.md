* 下载安装包

```shell
wget https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
```

* 安装mysql的rpm包

```shell
yum -y install mysql80-community-release-el7-3.noarch.rpm
```

* 安装mysql服务

```shell
yum install -y mysql-community-server --nogpgcheck   #--nogpgcheck,不检查gpg密钥
```

* 启动服务& 设置自启动

```shell
systemctl start mysqld   #启动MySQL
systemctl enable mysqld		#开机自启MySQL
```

* 查看初始密码

```shell
grep "password" /var/log/mysqld.log
```

* 修改密码

```shell
sudo mysql_secure_installation     #设置密码
Enter password for user root:
#输入/var/log/mysqld.log中获得密码
New password:
#输入新的密码
Re-enter new password:
#重复输入新的密码
Change the password for root ? ((Press y|Y for Yes, any other key for No)
#是否想改变root的密码
Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No)
#输入Y
Remove anonymous users? (Press y|Y for Yes, any other key for No)
#删除匿名用户
Disallow root login remotely? (Press y|Y for Yes, any other key for No)
#是否禁止远程登录
Remove test database and access to it? (Press y|Y for Yes, any other key for No)
#是否删除test数据库
Reload privilege tables now? (Press y|Y for Yes, any other key for No)
#是否刷新权限
```

* 密码规则

```shell
SHOW VARIABLES LIKE 'validate_password%';       #查看有哪些密码策略

SET GLOBAL validate_password.policy= 0;       #密码复杂度设置为低    1为中  2为高

set global validate_password.length = 6;    #设置密码长度为6

ALTER USER 'root'@'localhost' IDENTIFIED BY '密码';    # 再次修改密码

create user root@"%" identified by "密码";      #允许root用户远程连接

grant all privileges on *.* to root@"%" with grant option;#为新用户赋权 