

title: kafka 相关

date: 2017-06-30 00:00:00
tags: [kafka ]



---

一句话：分布式环境下`生产者消费者** 解耦`的中间容器，使用在` 发布-订阅的消息服务`/`log4j,nginx,mysql慢SQL各日志收集` 等场景中


作用 :   数据缓冲、异步通信、汇集日志、系统解耦

---
# Kafka基本概念

- Broker[`集群机器`]：消息中间件处理结点，一个Kafka节点就是一个broker，多个broker可以组成一个Kafka集群。
-  Topic[`主题`]：一类消息，例如page view日志、click日志等都可以以topic的形式存在，Kafka集群能够同时负责多个topic的分发。
-   Partition[` 主题的分目录存储` ]：topic物理上的分组，一个topic可以分为多个partition，每个partition是一个有序的队列。
-   Segment[` 主题消息的文件分组存储` ]：partition物理上由多个segment组成。
-  offset[`消息序号`]：每个partition都由一系列有序的、不可变的消息组成，这些消息被连续的追加到partition中。partition中的每个消息都有一个连续的序列号叫做offset,用于partition唯一标识一条消息。

#  Kafka消息发送的机制
首先获取topic的所有`Patition`
生产者上线，将数据发布到topic（` 指定partition/未 指定 则 余数/自 算法` ），消息会路由到 Patition



# Kafka消息消费机制
![]( https://static.oschina.net/uploads/space/2017/0227/154630_xHIL_1403215.png )


消费者通过pull的方式，定期从服务器拉取数据,当然在`pull数据`的时候，服务器会告诉consumer可消费的`消息offset`
`不同 Consumer Group下的消费者可以消费partition中相同的消息`，`相同的Consumer  Group下的消费者只能消费partition中不同的数据`。
服务器会记录每个consumer的在每个topic的每个partition下的消费的offset,然后每次去消费去拉取数据时，都会从上次记录的位置 （ 当consumer和partition增加或者删除时,需要重新执行一遍Consumer Rebalance算法 ）

#  Kafka消息存储机制

kafka的消息是存储在磁盘的，所以数据不易丢失
partition包含两部分： .index索引文件、.log消息内容文件
`.index` 索引文件 (offset消息编号-消息在对应文件中的偏移量)

`.log` 消息文件(完整消息，位置偏移量) ![]( https://static.oschina.net/uploads/space/2017/0227/192112_lUmB_1403215.png )


#  消息检索过程示例

例如读取`offset=368`的消息
（1）找到第368条消息在哪个`segment`
根据`二分查找`，可以快速定位，第368条消息是在00000000000000000300.log文件中
（2）从`index文件`中找到其物理偏移量
读取 00000000000000000300.index 。以368为key，得到value，如299，就是消息的物理位置偏移量
（3）到`log文件`中读取消息内容
读取 00000000000000000300.log  从偏移量299开始读取消息内容。完成了消息的检索过程


` Kafka消息生成，消费，存储机制 - - 博客频道 - CSDN.NET `
http://blog.csdn.net/u013063153/article/details/73799951


` Kafka学习之一 Kafka是什么，主要应用在什么场景? - 宝哥在路上 - 博客频道 - CSDN.NET `
http://blog.csdn.net/code52/article/details/50475511

` Kafka 高性能吞吐揭秘 `
https://segmentfault.com/a/1190000003985468


---
# 示例分析
## 生产者
-  设置配置属性
```
// 设置配置属性
Properties props = new Properties();
//		props.put("metadata.broker.list","172.168.63.221:9092,172.168.63.233:9092,172.168.63.234:9092");
props.put("metadata.broker.list","192.168.3.188:9092");
props.put("serializer.class", "kafka.serializer.StringEncoder");
// key.serializer.class默认为serializer.class
props.put("key.serializer.class", "kafka.serializer.StringEncoder");
// 可选配置，如果不配置，则使用默认的partitioner
//		props.put("partitioner.class", "com.catt.kafka.demo.PartitionerDemo");
props.put("partitioner.class", "example.PartitionerDemo");// 自算法实现partition选择(默认kafka会取余)
// 触发acknowledgement机制，否则是fire and forget，可能会引起数据丢失
// 值为0,1,-1,可以参考
// http://kafka.apache.org/08/configuration.html
props.put("request.required.acks", "1");
ProducerConfig config = new ProducerConfig(props);
```

-  创建producer
```  
// 创建producer
Producer<String, String> producer = new Producer<String, String>(config);
```

-  产生并发送消息
```

long start=System.currentTimeMillis();
for (long i = 0; i < events; i++) {
    long runtime = new Date().getTime();
    String ip = "192.168.3." + i;//rnd.nextInt(255);
    String msg = runtime + ",www.example.com," + ip;
    //如果topic不存在，则会自动创建，默认replication-factor为1，partitions为0
    KeyedMessage<String, String> data = new KeyedMessage<String, String>(
            "page_visits", ip, msg);
    producer.send(data);
}
System.out.println("耗时:" + (System.currentTimeMillis() - start));

// 关闭producer
producer.close();
```



## 消费者（kafkaSDK接入zookeeper）
-  topic 多路Stream消费
```
Map<String, Integer> topicCountMap = new HashMap<String, Integer>();
topicCountMap.put(topic, new Integer(numThreads));
Map<String, List<KafkaStream<byte[], byte[]>>> consumerMap = consumer.createMessageStreams(topicCountMap);// 多路Stream消费
``` - 固定量线程池对应topic多路Stream 消费
```
List<KafkaStream<byte[], byte[]>> streams = consumerMap.get(topic);
// now launch all the threads
executor = Executors.newFixedThreadPool(numThreads);
// now create an object to consume the messages
int threadNumber = 0;
for (final KafkaStream stream : streams) {
    executor.submit(new ConsumerMsgTask(stream, threadNumber));
    threadNumber++;
}
```
- 消费线程  ConsumerMsgTask  
```
public class ConsumerMsgTask implements Runnable {
    private KafkaStream m_stream;
    private int m_threadNumber;
    public ConsumerMsgTask(KafkaStream stream, int threadNumber) {
        m_threadNumber = threadNumber;
        m_stream = stream;
    }
    public void run() {
        ConsumerIterator<byte[], byte[]> it = m_stream.iterator();
        while (it.hasNext())/* 消费(在不消费者未shutdown前,会一直监听在此-即阻塞) */
            System.out.println("Thread " + m_threadNumber + ": " + new String(it.next().message()));
        System.out.println("Shutting down Thread: " + m_threadNumber);
    }
}
```

`Kafka JAVA客户端代码示例 - 推酷`

http://www.tuicool.com/articles/FRJzamq

---
# windows 练习
- zookeeper服务启动
> zkServer
- kafka服务启动
> .\bin\windows\kafka-server-start.bat .\config\server.properties
```
# 注册主题
.\bin\windows\kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
# 生产者上线
.\bin\windows\kafka-console-producer.bat --broker-list localhost:9092 --topic test
# 消费者上线
.\bin\windows\kafka-console-consumer.bat --zookeeper localhost:2181 --topic test


# 查看主题
.\bin\windows\kafka-topics.bat –list –zookeeper localhost:2181
.\bin\windows\kafka-topics.bat –describe –zookeeper localhost:2181 –topic test ```


**效果： ** `生产者窗口`输入消息 >> `消费者窗口`可立即收到


---
## linux 后台启动kafka

前台启动kafka：
> ./kafka-server-start.sh ../config/server.properties


后台启动kafka:
> ./kafka-server-start.sh ../config/server.properties 1>/dev/null 2>&1 &


http://blog.csdn.net/u013063153/article/details/72910748


---
` Kafka在Windows安装运行 - 林炳文Evankaka的专栏 - 博客频道 - CSDN.NET `

http://blog.csdn.net/evankaka/article/details/52421314


` kafka windows单机安装测试 - laputa73的专栏 - 博客频道 - CSDN.NET `
http://blog.csdn.net/laputa73/article/details/48826167

` Windows安装运行Kafka - OPEN 开发经验库`
http://www.open-open.com/lib/view/open1455842080261.html

