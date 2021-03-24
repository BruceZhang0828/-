### Kafka

#### 初识kafka

流处理平台，提供了消息的订阅与发布。

##### 作用：

- 系统间解耦
- 异步通信
- 削峰填谷

##### 用途：

- 消息队列Message Queue
- Kafka Streaming流处理

#### 消息队列

工作模式：

1.至多一次：producer将数据写入消息系统，consumer拉取消息，一旦消息被消费之后，由消息服务器主动删除队列中的数据，一般只允许一个consumer，消息队列中的数据不允许被重复消费。

2.没有限制：producer发布完数据以后，消息可以被多个消费者同时消费，一个消费者可以多次消费同一个记录，主要因为消息服务器一般可以长时间存储海量消息。

#### kafka的基本架构

kafka集群以topic形式负责分类集群中的record，每一个record属于一个topic。

topic底层都会对应一组分区的日志用于持久化topic的record。topic的每一个分区一定会有一个borker担当分区的leader，其他的broker担当分区的follower。

leader负责数据的读写，follower负责同步该分区的数据。如果leader宕机，会选举follower作为新的leader。

集群中的leader的监控和topic的部分元数据存储在zookeeper中。