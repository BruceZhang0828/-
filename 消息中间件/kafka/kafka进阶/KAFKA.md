### KAFKA

学习思路 -> 框架角度,横向 对比 纵向的知识点

#### what

粗略的宏观:  

消息中间件

kafka : AFK

x : 副本概念 出主机的 可以有读写分离,容易出现一致性问题。只能在主的P上进行W/R；

y:  topic业务

z:  partition

大数据 必然的 分治无关 聚合有关联

- 无关的数据就分散到不同的分区里,以追求并发并行

- 有关的数据按照原有的数据发送到同一个分区里

分区内部是有序的

分区外部无序

offset : 偏移量 

##### KAFKA架构:

zookeeper : 分布式协调        不要存储   选择多个broker 确定contorller的角色

​	单机管理 主从集群 =需要=> 分布式协调

broker(jvm) :  broker-0 , broker-1  是contorller的角色

topic(逻辑的)  -内部->  分区(物理的) : p0 p1

admin-api : = zookeeper=> 找到controller

producer: = 数据 => p0,p1 新版本通过controller 产生的matadata "b t p"  

**在业务层次上,分布式 角色之间的通信,不要因为业务需求,让zk集群成为负担**

consumer: p:c 1:1 或者是 n:1的关系。1：n的关系绝对不行。

组：不同业务。对接mysql，对接es。

kafka的broker的partition保存了，producer发送来的数据，重点是数据怎么去用。

**单一的使用场景下，先要保证，即便追求性能，用多个consumer，应该注意，不能一分区由由多个consumer消费。数据的重复利用是要站在Group上的，但是group组内要保障上述描述。**

在consumer消费的时候，

​	1.丢失，2.重复

围绕的是offset,也就是消费的进度。 节奏？频率？先后？ 

1. 异步的，5秒之内，先干活，持久化offset：重复消费
2. 同步的，业务的操作和offset的持久化
3. 没有控制好顺序，offset持久了，但是业务写失败了

runtime内存里维护了自己的offset(偏移量的进度)。内存中 ！

老版本通过zookeeper来维护，新版本：可以自身维护 = 通过 =》 topic  自己维护的offset。

第三方维护：



#### why

#### how



