### kafka的环境搭建

操作系统环境:centos 7

#### 单机版搭建

JDK1.8,配置Java-home

- 删除centos7中的OpenJDK

  ```shell
  # 查找安装目录
  rpm -qa | grep java
  # 删除 noarch可以不删除
  rpm -e -- nodeps java-xxx
  ```

- 下载rpm版本

  ```shell
  # 下载jdk8
  https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html
  # 通过软件上传到linux
  # 创建安装目录
  mkdie /usr/local/java
  # 解压安装包
  tar -zxvf jdk-8u281-linux-x64.tar.gz -C /usr/local/java/
  # 配置Java_home
  vi /etc/bashrc
  # 在文件最后插入
  JAVA_HOME=/usr/local/java/jdk1.8.0_281
  PATH=${JAVA_HOME}/bin:$PATH
  CLASSPATH=.:${JAVA_HOME}/lib
  export PATH JAVA_HOME CLASSPATH
  # 重新加载配置文件
  source /etc/bashrc
  # 查看java是否安装成功
  java -version
  ```

主机名和IP映射

```shell
# 查看主机名
cat /etc/sysconfig/netwoek
# 没有就自定义
# 增加主机名和ip映射
vi /etc/hosts
# 增加
192.168.xx.xx hostname
# 测试
ping hostname
```



关闭防火墙防火墙开机启动

```shell
# 关闭防火墙
systemctl stop firewalld.service 
# 关闭开启启动防火墙
systemctl disable filewalld.service
```

安装启动zookeeper

- zookeeper的官网下载 3.4.6

- 解压

- 修改配置文件

  ```shell
  # 拷贝原始的配置文件
  cp conf/zoo_sample.cfg conf/zoo.cfg
  # 修改配置文件 将数据存储路径自定义了
  vi conf/zoo.cfg
  # 新增数据存储的路径
  mkdir /root/zkdata
  # 启动zookeeper
  ./bin/zkServer.sh start zoo.cfg
  # 检查启动状态
  ./bin/zkServer.sh status
  ```

  

  

安装启动关闭kafka

kafka 2.2.0

- 解压
- 配置

```shell
# 修改配置文件
vi server.properties
# 修改 主机名 
listeners=PLAINTEXT://k_node1:9092
#kafka日志
log.dirs=/自定义日志存储文件
# 指定zookeeper
zookeeper.connect=hostname:2181
```

- 启动

  ```shell
  # 启动kafka
  ./bin/kafka-server-start.sh -daemon  config/server.properties 
  ```

- 测试消息队列功能

  ```shell
  # 创建topic
  ./bin/kafka-topics.sh --zookeeper k_node1:2181 --create --topic topic01 --partitions 3 --replication-factor 1
  # 创建消费者
  ./bin/kafka-console-consumer.sh --bootstrap-server k_node1:9092 --topic topic01 --group group1
  # 创建生产者
  ./bin/kafka-console-producer.sh --broker-list k_node1:9092 --topic topic01
  # 在生产者会话窗口回车之后输入 hello world
  # 消费者会话窗口会显示出hello world
  ```

#### 集群版搭建

同步时钟

yum install ntp -y

zookeeper配置

server.1=node1:2888:3888

server.2=node2:2888:3888

server.3=node3:2888:3888



echo 1 > /root/zkdata/myid

echo 2 > /root/zkdata/myid

echo 3 > /root/zkdata/myid