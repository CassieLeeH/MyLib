# 消息队列基本概念

## 什么是消息队列

**异步通讯**的**中间件** : 存放消息的容器。当我们需要使用消息时，取出消息供我们使用。

消息是一种数据结构（当然，对象也可以看做是一种特殊的消息），它包含**消费者与生产者双方都能识别的数据**，这些数据**需要在不同的进程（机器）之间进行传递**，并可能会被多个完全不同的客户端消费。

生产者先将消息投递一个叫做「队列」的容器中，然后再从这个容器中取出消息，最后再转发给消费者，仅此而已。本质：**一发一存一消费**，可以视为一种转发器。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210503225704218.png)

## 消息队列的功能

分布式场景下，通过消息传递和消息排队模型，提供**系统解耦、异步通信和流量削峰**的功能。

**场景举例**：电商业务中最常见的「订单支付」场景：在订单支付成功后，需要更新订单状态、更新用户积分、通知商家有新订单、更新推荐系统中的用户画像等等。

**缺点：系统彼此依赖，耦合性比较高，消息只能同步逐层传递，如果订单比较多，系统的吞吐量就可能不足。**

此时引入消息队列：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210504084517524.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzk1ODU1Ng==,size_16,color_FFFFFF,t_70)

引入 MQ 后，订单支付只需要关注它最重要的流程：更新订单状态即可。其他不重要的事情全部交给 MQ 来通知。这便是 MQ 解决的最核心的问题：**系统解耦**。

改造前订单系统依赖 3 个外部系统，改造后仅仅依赖 MQ，而且后续业务再扩展（比如：营销系统打算针对支付用户奖励优惠券），也不涉及订单系统的修改，从而保证了核心流程的稳定性，**降低了维护成本**。

这个改造还带来了另外一个好处：因为 MQ 的引入，更新用户积分、通知商家、更新用户画像这些步骤全部变成了**异步执行**，能减少订单支付的整体耗时，**提升订单系统的吞吐量**。这便是 MQ 的另一个典型应用场景：**异步通信**。

除此以外，由于**队列能转储消息，对于超出系统承载能力的场景，可以用 MQ 作为 “漏斗” 进行限流保护**，即所谓的**流量削峰**。

## 消息队列与RPC

从MQ 的通信模型来看，可以理解成：两次 RPC + 消息转储。

**1、**引入 MQ 后，由之前的一次 RPC 变成了现在的两次 RPC，而且生产者只跟队列耦合，它根本无需知道消费者的存在。
**2、**多了一个中间节点「队列」进行消息转储，相当于将同步变成了异步。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210504084404451.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80Mzk1ODU1Ng==,size_16,color_FFFFFF,t_70)

## 常见类型

**点对点模型**

也叫消息队列模型。如果拿上面那个“民间版”的定义来说，那么系统A发送的消息只能被系统B接收，其他任何系统都不能读取A发送的消息。日常生活的例子比如电话客服就属于这种模型：同一个客户呼入电话只能被一位客服人员处理，第二个客服人员不能为该客户服务。

**发布/订阅模型**

主题（Topic）  发布者（Publisher）订阅者（Subscriber）。

和点对点模型不同的是，这个模型可能存在多个发布者向相同的主题发送消息，而订阅者也可能存在多个，它们都能接收到相同主题的消息。生活中的报纸订阅就是一种典型的发布/订阅模型。

## 消息队列设计

（待定，积累更多实际经验补充）

功能性需求：发消息、存消息、消费消息

非功能性需求：高性能、高可用、高扩展



# Kafka入门与基本使用

项目使用的Kafka版本为2.7.0

## Kafka术语整理

- 消息：Record。Kafka是消息引擎嘛，这里的消息就是指Kafka处理的主要对象。

- 主题：Topic。主题是承载消息的逻辑容器，在实际使用中多用来区分具体的业务。

- 分区：Partition。一个有序不变的消息序列。每个主题下可以有多个分区。

- 消息位移：Offset。表示分区中每条消息的位置信息，是一个单调递增且不变的值。

- 副本：Replica。Kafka中同一条消息能够被拷贝到多个地方以提供数据冗余，这些地方就是所谓的副本。副本还分为领导者副本和追随者副本，各自有不同的角色划分。副本是在分区层级下的，即每个分区可配置多个副本实现高可用。

- 生产者：Producer。向主题发布新消息的应用程序。

- 消费者：Consumer。从主题订阅新消息的应用程序。

- 消费者位移：Consumer Offset。表征消费者消费进度，每个消费者都有自己的消费者位移。

- 消费者组：Consumer Group。多个消费者实例共同组成的一个组，同时消费多个分区以实现高吞吐。

- 重平衡：Rebalance。消费者组内某个消费者实例挂掉后，其他消费者实例自动重新分配订阅主题分区的过程。Rebalance是Kafka消费者端实现高可用的重要手段。

  ![图片展示概念](https://img-blog.csdnimg.cn/1e2242f45b0643b98c1ba69f2e55658d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBASnV6aWp1bl85NjI0NjQ=,size_20,color_FFFFFF,t_70,g_se,x_16)

## Kafka Streams

Kafka也可以作为流处理平台，Kafka与其他主流大数据流式计算框架相比，优势在哪里呢？

**第一点是更容易实现端到端的正确性（Correctness）**。Google大神Tyler曾经说过，流处理要最终替代它的“兄弟”批处理需要具备两点核心优势：**要实现正确性和提供能够推导时间的工具。实现正确性是流处理能够匹敌批处理的基石**。正确性一直是批处理的强项，而实现正确性的基石则是要求框架能提供精确一次处理语义，即处理一条消息有且只有一次机会能够影响系统状态。目前主流的大数据流处理框架都宣称实现了精确一次处理语义，但这是有限定条件的，即它们只能实现框架内的精确一次处理语义，无法实现端到端的。

这是为什么呢？因为当这些框架与外部消息引擎系统结合使用时，它们无法影响到外部系统的处理语义，所以如果你搭建了一套环境使得Spark或Flink从Kafka读取消息之后进行有状态的数据计算，最后再写回Kafka，那么你只能保证在Spark或Flink内部，这条消息对于状态的影响只有一次。但是计算结果有可能多次写入到Kafka，因为它们不能控制Kafka的语义处理。相反地，Kafka则不是这样，因为所有的数据流转和计算都在Kafka内部完成，故Kafka可以实现端到端的精确一次处理语义。

**可能助力Kafka胜出的第二点是它自己对于流式计算的定位**。官网上明确标识Kafka Streams是一个用于搭建实时流处理的客户端库而非是一个完整的功能系统。这就是说，你不能期望着Kafka提供类似于集群调度、弹性部署等开箱即用的运维特性，你需要自己选择适合的工具或系统来帮助Kafka流处理应用实现这些功能。

读到这你可能会说这怎么算是优点呢？坦率来说，这的确是一个“双刃剑”的设计，也是Kafka社区“剑走偏锋”不正面PK其他流计算框架的特意考量。大型公司的流处理平台一定是大规模部署的，因此具备集群调度功能以及灵活的部署方案是不可或缺的要素。但毕竟这世界上还存在着很多中小企业，它们的流处理数据量并不巨大，逻辑也并不复杂，部署几台或十几台机器足以应付。在这样的需求之下，搭建重量级的完整性平台实在是“杀鸡焉用牛刀”，而这正是Kafka流处理组件的用武之地。因此从这个角度来说，未来在流处理框架中，Kafka应该是有一席之地的。



## 生产环境中Kafka集群部署

从操作系统、磁盘、磁盘容量和带宽方面考虑

**操作系统：**

从I/O模型考虑，Kafka客户端底层使用了**Java的selector**，selector在**Linux**上的实现机制是**epoll**，在**Windows**平台上的实现机制是**select**。epoll就比select要好。**因此在这一点上将Kafka部署在Linux上是有优势的，因为能够获得更高效的I/O性能。**

从网络传输效率考虑，Kafka生产和消费的消息都是通过网络传输的，而消息保存在磁盘。故Kafka需要在磁盘和网络间进行大量数据传输。Linux平台实现了这样的零拷贝机制，当数据在磁盘和网络进行传输时避免昂贵的内核态数据拷贝从而实现快速的数据传输。

从社区的支持度上，Linux修BUG的速度也更快。

综上，**将Kafka部署在Linux上是有优势的**。

**磁盘:**

建议是使用普通机械硬盘即可。Kafka大量使用磁盘不假，可它使用的方式多是顺序读写操作，一定程度上规避了机械磁盘最大的劣势，即随机读写操作慢。从这一点上来说，使用SSD似乎并没有太大的性能优势，毕竟从性价比上来说，机械磁盘物美价廉，而它因易损坏而造成的可靠性差等缺陷，又由Kafka在软件层面提供机制来保证，故使用普通机械磁盘是很划算的。

是否应该使用磁盘阵列（RAID）。使用RAID的两个主要优势在于：

- 提供冗余的磁盘存储空间
- 提供负载均衡

以上两个优势对于任何一个分布式系统都很有吸引力。不过就Kafka而言，一方面Kafka自己实现了冗余机制来提供高可靠性；另一方面通过分区的概念，Kafka也能在软件层面自行实现负载均衡。如此说来RAID的优势就没有那么明显了。

综合以上的考量：

- 追求性价比的公司可以不搭建RAID，使用普通磁盘组成存储空间即可。
- 使用机械磁盘完全能够胜任Kafka线上环境。

**磁盘容量:**

规划磁盘容量时考虑下面这几个元素：预留20%-30%空间。

- 新增消息数
- 消息留存时间
- 平均消息大小
- 备份数
- 是否启用压缩

**带宽：**

根据实际带宽资源和业务SLA预估服务器数量，对于千兆网络，建议每台服务器按照700Mbs来计算，避免超过70%的阈值造成网络丢包。



## Kafka集群参数配置

### **Broker端参数**

与**存储信息**相关的参数：

- `log.dirs`：这是非常重要的参数，指定了Broker需要使用的若干个文件目录路径。这个参数是没有默认值的，必须由你亲自指定。
- `log.dir`：注意这是dir，结尾没有s，说明它只能表示单个路径，它是补充上一个参数用的。

**补充：**一般只要设置log.dirs。在线上生产环境中一定要为`log.dirs`配置多个路径，具体格式是一个CSV格式，也就是用逗号分隔的多个路径，比如`/home/kafka1,/home/kafka2,/home/kafka3`这样。



与**ZooKeeper**相关的设置：

ZooKeeper是一个分布式协调框架，负责协调管理并保存Kafka集群的所有元数据信息，比如集群都有哪些Broker在运行、创建了哪些Topic，每个Topic都有多少分区以及这些分区的Leader副本都在哪些机器上等信息。

- `zookeeper.connect`：也是一个CSV格式的参数，比如我可以指定它的值为`zk1:2181,zk2:2181,zk3:2181`。2181是ZooKeeper的默认端口。

**补充：**现在问题来了，如果我让**多个Kafka集群使用同一套ZooKeeper集群**，那么这个参数应该怎么设置呢？这时候chroot就派上用场了。这个chroot是ZooKeeper的概念，类似于别名。如果你有两套Kafka集群，假设分别叫它们kafka1和kafka2，那么两套集群的`zookeeper.connect`参数可以这样指定：`zk1:2181,zk2:2181,zk3:2181/kafka1`和`zk1:2181,zk2:2181,zk3:2181/kafka2`。切记chroot只需要写一次，而且是加到最后的。



与**Broker连接**相关：

- `listeners`：学名叫监听器，其实就是告诉外部连接者要通过什么协议访问指定主机名和端口开放的Kafka服务。
- `advertised.listeners`：和listeners相比多了个advertised。Advertised的含义表示宣称的、公布的，就是说这组监听器是Broker用于对外发布的。
- `host.name/port`：列出这两个参数就是想说你把它们忘掉吧，压根不要为它们指定值，毕竟都是过期的参数了。

**补充**：监听器是若干个逗号分隔的三元组，每个三元组的格式为`<协议名称，主机名，端口号>`。这里的协议名称可能是标准的名字，比如PLAINTEXT表示明文传输、SSL表示使用SSL或TLS加密传输等；也可能是你自己定义的协议名字，比如`CONTROLLER: //localhost:9092`。

一旦你自己定义了协议名称，你必须还要指定`listener.security.protocol.map`参数告诉这个协议底层使用了哪种安全协议，比如指定`listener.security.protocol.map=CONTROLLER:PLAINTEXT表示CONTROLLER`这个自定义协议底层使用明文不加密传输数据。

主机名设置中到底使用IP地址还是主机名？建议：**最好全部使用主机名，即Broker端和Client端应用配置中全部填写主机名**。Broker源代码中也使用的是主机名。



**关于Topic管理：**

- `auto.create.topics.enable`：**是否允许自动创建Topic。最好设置成false**，即不允许自动创建Topic。
- `unclean.leader.election.enable`：**是否允许Unclean Leader选举。如果设置成false**，那么就坚持之前的原则，坚决不能让那些落后太多的副本竞选Leader。这样做的后果是这个分区就不可用了，因为没有Leader了。反之如果是true，那么Kafka允许你从那些“跑得慢”的副本中选一个出来当Leader。这样做的后果是数据有可能就丢失了，因为这些副本保存的数据本来就不全，当了Leader之后它本人就变得膨胀了，认为自己的数据才是权威的。
- `auto.leader.rebalance.enable`：**是否允许定期进行Leader选举**。设置它的值为true表示允许Kafka定期地对一些Topic分区进行Leader重选举。严格来说它与上一个参数中Leader选举的最大不同在于，它不是选Leader，而是换Leader！比如Leader A一直表现得很好，但若`auto.leader.rebalance.enable=true`，那么有可能一段时间后Leader A就要被强行卸任换成Leader B。换一次Leader代价很高的，原本向A发送请求的所有客户端都要切换成向B发送请求，而且这种换Leader本质上没有任何性能收益，因此**建议在生产环境中把这个参数设置成false**。



**关于数据留存参数：**

- `log.retention.{hours|minutes|ms}`：这是个“三兄弟”，都是控制一条消息数据被保存多长时间。从优先级上来说ms设置最高、minutes次之、hours最低。默认保存7天的数据。
- `log.retention.bytes`：这是指定Broker为消息保存的总磁盘容量大小。默认是-1，表明你想在这台Broker上保存多少数据都可以。
- `message.max.bytes`：控制Broker能够接收的最大消息大小。默认的1000012太少了，还不到1MB。



### Topic级别参数

同时设置了Topic级别参数和全局Broker参数的话，**Topic级别参数会覆盖全局Broker参数的值**，而每个Topic都能设置自己的参数值。

**保存保存消息相关：**

- `retention.ms`：规定了该Topic消息被保存的时长。默认是7天，即该Topic只保存最近7天的消息。一旦设置了这个值，它会覆盖掉Broker端的全局参数值。
- `retention.bytes`：规定了要为该Topic预留多大的磁盘空间。和全局参数作用相似，这个值通常在多租户的Kafka集群中会有用武之地。当前默认值是-1，表示可以无限使用磁盘空间。
- `max.message.bytes`。它决定了Kafka Broker能够正常接收该Topic的最大消息大小。

使用自带的命令`kafka-configs`来修改Topic级别参数:

```java
bin/kafka-configs.sh --zookeeper localhost:2181 --entity-type topics --entity-name transaction --alter --add-config max.message.bytes=10485760
```



**JVM参数：**

Kafka服务器端代码是用Scala语言编写的，编译成Class文件在JVM上运行，因此JVM参数设置对于Kafka集群很重要。

- `KAFKA_HEAP_OPTS`：指定堆大小。推荐6GB。
- `KAFKA_JVM_PERFORMANCE_OPTS`：指定GC参数。Java 8，手动设置使用G1收集器，性能表现更优秀。

```
$> export KAFKA_HEAP_OPTS=--Xms6g  --Xmx6g
$> export KAFKA_JVM_PERFORMANCE_OPTS= -server -XX:+UseG1GC -XX:MaxGCPauseMillis=20 -XX:InitiatingHeapOccupancyPercent=35 -XX:+ExplicitGCInvokesConcurrent -Djava.awt.headless=true
$> bin/kafka-server-start.sh config/server.properties
```



**操作系统参数：**

- 文件描述符限制     ulimit -n 设置成超大的数，比如：ulimit -n 1000000
- 文件系统类型    生产环境最好还是使用**XFS**
- Swappiness     建议swappniess配置成一个接近0但不为0的值，比如1
- 提交时间    默认是5秒，可以适当调高



# 客户端实践与原理

## 生产者消息分区机制原理剖析

**分区原因**

Kafka的消息组织方式实际上是三级结构：主题-分区-消息。主题下的每条消息只会保存在某一个分区中，而不会在多个分区中被保存多份。

其实分区的作用就是**提供负载均衡的能力**，或者说对数据进行分区的主要原因，就是为了实现系统的高伸缩性（Scalability）。

不同的分区能够被放置到不同节点的机器上，而数据的读写操作也都是针对分区这个粒度而进行的，这样每个节点的机器都能独立地执行各自分区的读写请求处理。具有相同事件键（例如，客户或车辆 ID）的事件被写入同一个分区，并且 Kafka保证给定主题分区的任何消费者将**始终以与写入事件完全相同的顺序读取该分区的事件**。并且，还可以通过添加新的节点机器来增加整体系统的吞吐量。



![img](https://kafka.apache.org/images/streams-and-tables-p1_p4.png)



**分区策略**是决定生产者将消息发送到哪个分区的算法。Kafka提供了默认的分区策略，也支持自定义。

自定义策略：

如果要自定义分区策略，需要显式地配置生产者端的参数`partitioner.class`。

在编写生产者程序时，编写一个具体的类实现`org.apache.kafka.clients.producer.Partitioner`接口。这个接口也很简单，只定义了两个方法：`partition()`和`close()`，通常你只需要实现最重要的partition方法。我们来看看这个方法的方法签名：

```java
int partition(String topic, Object key, byte[] keyBytes, Object value, byte[] valueBytes, Cluster cluster);
```



**常见分区策略：**

- 轮询策略：挨个轮流顺序分配，默认分配方式。
- 随机策略：随意地将消息放置到任意一个分区上。（想出这个方法的人可以和猴子排序做个pk了）
- 按消息键保存策略：把消息分类，设置不同的消息建，然后顺序存放在不同的分区中。

```java
//随机策略
List<PartitionInfo> partitions = cluster.partitionsForTopic(topic);
return ThreadLocalRandom.current().nextInt(partitions.size());

//按消息键保存策略
List<PartitionInfo> partitions = cluster.partitionsForTopic(topic);
return Math.abs(key.hashCode()) % partitions.size();
```



## 生产者压缩算法

压缩（compression）用时间去换空间的经典trade-off思想，用CPU时间去换磁盘空间或网络I/O传输量，希望以较小的CPU开销带来更少的磁盘占用或更少的网络I/O传输。故**Producer压缩，Broker保持，Consumer解压缩**。

生产者程序中**配置compression.type参数即表示启用指定类型的压缩算法**

比如下面这段程序代码展示了如何构建一个开启GZIP的Producer对象：

```java
 Properties props = new Properties();
 props.put("bootstrap.servers", "localhost:9092");
 props.put("acks", "all");
 props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
 props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");
 // 表明该Producer的压缩算法使用的是GZIP,开启GZIP压缩
 props.put("compression.type", "gzip");
 
 Producer<String, String> producer = new KafkaProducer<>(props);
```

Producer启动后生产的每个消息集合都是经GZIP压缩过的，故而能很好地节省网络传输带宽以及Kafka Broker端的磁盘占用。

**压缩算法性能对比：**

在吞吐量方面：LZ4 > Snappy > zstd和GZIP；

而在压缩比方面，zstd > LZ4 > GZIP > Snappy。

## Java生产者如何管理TCP连接

Kafka采用TCP协议作为所有请求通信的底层协议：TCP有多路复用请求和同时轮询多个连接的能力。

Java Producer端管理TCP连接的方式是：

1. KafkaProducer实例创建时启动Sender线程，从而创建与bootstrap.servers中所有Broker的TCP连接。
2. KafkaProducer实例首次更新元数据信息之后，还会再次创建与集群中所有Broker的TCP连接。
3. 如果Producer端发送消息到某台Broker时发现没有与该Broker的TCP连接，那么也会立即创建连接。
4. 如果设置Producer端connections.max.idle.ms参数大于0，则步骤1中创建的TCP连接会被自动关闭；如果设置该参数=-1，那么步骤1中创建的TCP连接将无法被关闭，从而成为“僵尸”连接。

Producer端关闭TCP连接的方式有两种：**一种是用户主动关闭；一种是Kafka自动关闭**。



## 幂等生产者和事务生产者

消息交付可靠性保障：Kafka对Producer和Consumer要处理的消息提供什么样的承诺。常见的承诺有以下三种：

- 最多一次（at most once）：消息可能会丢失，但绝不会被重复发送。
- 至少一次（at least once）：消息不会丢失，但有可能被重复发送。**默认模式**。
- 精确一次（exactly once）：消息不会丢失，也不会被重复发送。

