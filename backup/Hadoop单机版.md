# 一，安装jdk
一般为
```bash
apt install openjdk-8-jdk
```
默认安装位置在`/usr/lib/jvm/`文件夹中
<!--more-->
# 二，下载hadoop
- 创建文件夹
```bash
mkdir /usr/local/hadoop
```
- 下载命令
```bash
 wget https://archive.apache.org/dist/hadoop/common/hadoop-3.1.3/hadoop-3.1.3.tar.gz -O /usr/local/hadoop/hadoop-3.1.3.tar.gz
 ```
 - 进入`/usr/local/hadoop`目录解压缩
 ```bash
 tar -zxvf hadoop-3.1.3.tar.gz
 ```
 # 三，配置hadoop
 - 配置hadoop启动项
 ```bash
vim /etc/profile.d/hadoop.sh
export HADOOP_HOME=/usr/local/hadoop/hadoop-3.1.3
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
source /etc/profile
 ```
 - 创建hadoop用户(使用root用户启动可跳过)
 ```bash
新建用户
adduser hadoop
设置密码
passwd hadoop
新建用户组
groupadd hadoop
用户加到用户组
usermod -G hadoop hadoop
 ```
 - 创建hadoop文件目录并授权
 ```bash
mkdir -p /home/hadoop/tmp # 临时文件目录
mkdir -p /home/hadoop/hdfs/name # namenode文件目录
mkdir -p /home/hadoop/hdfs/data # datanode文件目录
mkdir -p /home/hadoop/log # 日志文件目录
sudo chown -R hadoop /home/hadoop # 设置hadoop目录所有者
```
# 四，修改hadoop配置文件
## 以下操作均在`usr/local/hadoop/hadoop-3.1.3/etc/hadoop`目录<br>
-  hadoop-env.sh
在`hadoop-env.sh`中找到`export JAVA_HOME=`并修改为
```
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```
- core-site.xml
```xml
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
        <description>hdfs监听端口</description>
    </property>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>file:/home/hadoop/tmp</value>
        <description>hadoop临时数据存放</description>
    </property>
</configuration>
```
- hdfs-site.xml
```xml
<configuration>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>file:/home/hadoop/hdfs/name</value>
        <description>HDFS的元数据储存目录</description>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:/home/hadoop/hdfs/data</value>
         <description>数据块存储目录</description>
    </property>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
      <description>HDFS中数据块的复制因子,value代表复制次数</description>
    </property>
</configuration>
```

:::tip{title="提示"}
root用户还需要在`hadoop-3.1.3/sbin`目录下修改`start-dfs.sh，stop-dfs.sh，start-yarn.sh，stop-yarn.sh`在顶部添加即可
:::
- start-dfs.sh & stop-dfs.sh
```bash
HDFS_DATANODE_USER=root
HADOOP_SECURE_DN_USER=hdfs
HDFS_NAMENODE_USER=root
HDFS_SECONDARYNAMENODE_USER=root
```
- start-yarn.sh & stop-yarn.sh
 ```bash
YARN_RESOURCEMANAGER_USER=root
HADOOP_SECURE_DN_USER=yarn
YARN_NODEMANAGER_USER=root
 ```
# 五，设置SSH免密访问本地
- 切换至`/home/hadoop`目录下,也就是刚才新建的用户目录
```bash
ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa
cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

输入以下命令，测试免密是否成功
```bash
ssh localhost
```
如创建的时候提示`.ssh目录`不存在，则切回root用户自行创建后再使用hadoop用户创建

:::tip{title="提示"}
root用户也需要设置免密登录
:::
- root用户：
   ```bash
  ssh-keygen -t rsa #rsa和上面的dsa表示加密方式
  cat ~/.ssh/id_rsa.pub > ~/.ssh/authorized_keys
   ```

# 六，启动hadoop
- 格式化hadoop文件系统
```bash
./bin/hdfs namenode -format
```
- 启动
```
./sbin/start-all.sh
```
-使用`jps`命令查看是否启动成功

![image.png](/static/img/59736f802af3e3d04ded902263df16ba.image.webp)

