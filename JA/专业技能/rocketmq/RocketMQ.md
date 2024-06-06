

## 介绍
![[Pasted image 20240412090618.png]]
![[Pasted image 20240412090640.png]]
集群特点
![[Pasted image 20240412090707.png]]
集群模式
![[Pasted image 20240412090730.png]]
2m-2s
![[Pasted image 20240412090815.png]]
工作流程
![[Pasted image 20240412090843.png]]

mqadmin命令、可视化工具监控和简化命令

## 消息发送

1、同步消息
2、异步消息
3、单向消息
4、消息消费  
同一个组内的集群，接收同一个topic、tag时，默认负载均衡，可设定广播模式
5、顺序消息
一个topic，broker里面有多个queue
RocketMQ 通过将顺序消息发送到同一个队列并由单个消费者消费该队列来保证消息的顺序。生产者在发送消息时可以使用自定义的 Message Queue Selector 根据业务键（如订单ID）将消息路由到特定的队列。消费者则需要配置为顺序消费，这通常意味着在一个消费者组内，只有一个线程或进程负责消费特定队列的消息。
6、延迟消息
7、批量消息
8、过滤消息
9、事务消息
![[Pasted image 20240413090519.png]]

## 消息存储
![[Pasted image 20240413100254.png]]
![[Pasted image 20240413100322.png]]

消息存储![[Pasted image 20240413100607.png]]
消息发送
![[Pasted image 20240413100509.png]]
![[Pasted image 20240413100554.png]]
存储结构
![[Pasted image 20240413101323.png]]

## 刷盘机制
![[Pasted image 20240413101344.png]]
## 高可用性

##### nameserver 集群 高可用
##### broker集群 高可用
![[Pasted image 20240413102354.png]]
##### 保障消费者可消费
![[Pasted image 20240413103959.png]]
##### 保障生产者可生产
![[Pasted image 20240413103518.png]]
续上图![[Pasted image 20240413104247.png]]

##### 主从复制
![[Pasted image 20240413105025.png]]

一般采用同步复制，异步刷盘


## 消费者和生产者集群
负载均衡
生产者![[Pasted image 20240413110228.png]]
消费者![[Pasted image 20240413110053.png]]
广播模式


## 消息重试
![[Pasted image 20240413111017.png]]

![[Pasted image 20240413111058.png]]
![[Pasted image 20240413111132.png]]
![[Pasted image 20240413111142.png]]
还可自定义重试次数
重试次数可获得

## 死信队列
![[Pasted image 20240413111309.png]]


## 消息幂等性
![[Pasted image 20240413111720.png]]



#### 面试题：Broker 配置更新后是否一定要进行重启？有没有其他方法可以避免重启？

**答案**： 通常，对于关键配置更改，如修改连接到 NameServer 的地址，需要重启 Broker 以确保配置正确生效。对于一些动态支持更新的配置项，可以通过管理工具如 `mqadmin` 进行在线更新，这种情况下不需要重启。为了减少重启对生产环境的影响，可以采用滚动重启的策略。

#### 面试题：如何确保在多 Broker 环境中更新配置时服务的连续性？

**答案**： 在多 Broker 环境中，推荐使用滚动重启策略来更新配置。这意味着一次只重启一个 Broker，而其他 Broker 继续提供服务。此外，确保所有 Broker 在更新前后均能正常连接到 NameServer，以及集群状态稳定，通过测试和监控验证更新是否成功应用并且系统运行正常。




# 如何保证消息不丢失

## 什么地方丢
生产者发

主从同步

消息存盘

消费者消费


# 消息幂等性

# 保证消息顺序


# 消息高效读写





# 部署

[【Rocketmq】通过 docker 快速搭建 rocketmq 环境-腾讯云开发者社区-腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1610494)


```
docker pull foxiswho/rocketmq:4.8.0


 docker network create rocketmq-net

docker run -d --name mqNameServer -p 9876:9876 --network rocketmq-net foxiswho/rocketmq:4.8.0 sh mqnamesrv


brokerClusterName = DefaultCluster brokerName = broker-a brokerId = 0 deleteWhen = 04 fileReservedTime = 48 brokerRole = ASYNC_MASTER flushDiskType = ASYNC_FLUSH brokerIP1 = {本地外网 IP}


docker run -d \
  -p 10911:10911 \
  -p 10909:10909 \
  -v /root/mqconf/data/broker/logs:/root/logs \
  -v /root/mqconf/rocketmq/data/broker/store:/root/store \
  -v /root/mqconf/conf/broker.conf:/home/rocketmq/rocketmq-4.8.0/conf/broker.conf \
  --name rmqbroker \
  --network rocketmq-net \
  -e "NAMESRV_ADDR=mqNameServer:9876" \
  -e "BROKER_IP1=106.53.217.117" \
  -e "JAVA_OPT_EXT=-server -Xms512m -Xmx512m -Xmn256m" \
  foxiswho/rocketmq:4.8.0 \
  sh mqbroker -n mqNameServer:9876 -c /home/rocketmq/rocketmq-4.8.0/conf/broker.conf


```
在 MySQL 命令行中，执行以下 SQL 查询来获取当前的日期和时间：
SELECT NOW();