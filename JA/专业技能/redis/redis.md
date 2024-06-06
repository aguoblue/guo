
### 数据结构
key
value
	String
	Hash
	List
	Set
	SortedSet
	GEO
	BitMap
	HyperLog
SpringDataRedis
	jedis
	lettuce
RedisTemplate
StringRedisTemplate


session共享问题
	多台Tomcat并不共享session存储空间，请求到其他tomcat服务时导致数据丢失的问题
	替代方案
		数据共享
		内存存储
		key、value结构
	redis

redis
	缓存更新
		内存淘汰  Redis内存不足自动淘汰部分数据
		超时剔除  TTL
		主动更新

主动更新
	Cache Aside Pattern 缓存调用者更新数据库同时更新缓存
	缓存和数据库整和成一个服务，由服务来维护一致性
	调用者只操作缓存，由其他线程异步的将数据持久化到数据库

Cache Aside Pattern
![[Pasted image 20240330220919.png]]
操作顺序会有线程安全问题
	读线程和写线程之间的切换导致缓存不一致

缓存穿透
	访问缓存和数据库都没有的内容
	1空对象
	2布隆

缓存雪崩
	同一时段大量缓存过期或者redis宕机，大量请求到达数据库
	TTL随机
	Redis集群
	降级限流
	多级缓存
		多个层面建立缓存

缓存击穿
	热点key，被高并发访问并且缓存重建业务较复杂的key突然失效了，无数的请求到达数据库
	互斥锁
	逻辑过期
	![[Pasted image 20240330224201.png]]
![[Pasted image 20240330225700.png]]


Redis全局唯一ID

乐观锁
悲观锁

集群下多个jvm锁监视器不一样


分布式锁
![[Pasted image 20240331221701.png]]
![[Pasted image 20240331222214.png]]锁过期了会出现线程安全问题
释放锁时先做判断，是不是自己的锁，再释放锁，中间也会出现线程安全问题

判断锁和释放锁必须有原子性

lua脚本原子性


![[Pasted image 20240331224757.png]]
集群时
	读写分离
	对主写、对从读

Redisson
看源码懂
	可重入
		数据结构Map
	可重试
		尝试时间，订阅-唤醒
	可续约看门狗定时任务自动续约，保证执行业务期间，锁不会过期，其他线程进不来
		**分布式锁是不能设置永不过期的**，这是为了避免在分布式的情况下，一个节点获取锁之后宕机从而出现死锁的情况，所以需要个分布式锁设置一个过期时间。但是这样会导致一个线程拿到锁后，**在锁的过期时间到达的时候程序还没运行完**，导致锁超时释放了，那么其他线程就能获取锁进来，从而出现问题。
![[Pasted image 20240401094856.png]]

集群
	主从
		主从同步
		向主索取锁后，主从同步的过程中主节点挂了，从节点哨兵发现后，选出一个从节点作为主节点，如果其他线程来获取锁，也能获取成功，这样出现主从一致性问题
	连锁
![[Pasted image 20240401091653.png]]


![[Pasted image 20240401094817.png]]


消息队列

![[Pasted image 20240401102825.png]]
![[Pasted image 20240401105736.png]]

![[Pasted image 20240401105717.png]]

![[Pasted image 20240401110719.png]]

![[Pasted image 20240401111137.png]]


![[Pasted image 20240401112531.png]]

![[Pasted image 20240401114636.png]]

## 持久化

RDB
	主进程save
	子进程bgsave
	退出自动save
	内部触发RDB
		save 900 1
		save 300 10
![[Pasted image 20240401130413.png]]

AOF
	默认关闭
	![[Pasted image 20240401131047.png]]
![[Pasted image 20240401131101.png]]

![[Pasted image 20240401131150.png]]

## 主从

![[Pasted image 20240401131301.png]]
![[Pasted image 20240401132844.png]]
![[Pasted image 20240401133729.png]]

![[Pasted image 20240401135735.png]]


![[Pasted image 20240401135714.png]]

## 哨兵

![[Pasted image 20240401140350.png]]
![[Pasted image 20240401140538.png]]

![[Pasted image 20240401141710.png]]![[Pasted image 20240401141729.png]]


## 分片集群
![[Pasted image 20240401150203.png]]

## 散列插槽
![[Pasted image 20240401190932.png]]


## 集群伸缩
添加节点到一个集群
从一个集群删除节点



## 故障转移
自动
手动
![[Pasted image 20240401192748.png]]

## 多级缓存
![[Pasted image 20240401200120.png]]
caffeine缓存



## 数据同步



## 键值设计
String
BigKey



## 批处理优化

![[Pasted image 20240401223021.png]]
原生的M操作
pipeline批处理
注意
	批处理时不建议一次携带太多命令
	pipeline的多个命令之间不具备原子性


![[Pasted image 20240401223726.png]]


## 服务端优化
持久化配置

![[Pasted image 20240401225303.png]]

慢查询
![[Pasted image 20240401230029.png]]

![[Pasted image 20240401230043.png]]


redis安全

![[Pasted image 20240402140504.png]]



# 原理篇

## 数据结构
### 动态字符串SDS

![[Pasted image 20240402191210.png]]

### Intset

![[Pasted image 20240402192856.png]]
![[Pasted image 20240402192300.png]]

### dict
![[Pasted image 20240402193719.png]]

![[Pasted image 20240402195110.png]]

![[Pasted image 20240402195124.png]]

### Ziplist
![[Pasted image 20240402200104.png]]
![[Pasted image 20240402200122.png]]

![[Pasted image 20240402200517.png]]

![[Pasted image 20240402200538.png]]

### Quicklist

![[Pasted image 20240402201957.png]]
![[Pasted image 20240403110105.png]]

### SkipList跳表
![[Pasted image 20240402202906.png]]
![[Pasted image 20240402203153.png]]


### RedisObject
![[Pasted image 20240402203642.png]]

![[Pasted image 20240402204640.png]]

![[Pasted image 20240402204656.png]]

### String
![[Pasted image 20240403105829.png]]
![[Pasted image 20240403105759.png]]
### List

![[Pasted image 20240403110220.png]]

![[Pasted image 20240403110233.png]]
### Set

![[Pasted image 20240403110537.png]]

### Zset
![[Pasted image 20240403111739.png]]

![[Pasted image 20240403111752.png]]

![[Pasted image 20240403111823.png]]

![[Pasted image 20240403111837.png]]

### Hash
![[Pasted image 20240403112154.png]]


## 用户空间和内核空间

### 阻塞IO
![[Pasted image 20240403113556.png]]

### 非阻塞IO
![[Pasted image 20240403113802.png]]

### IO多路复用
![[Pasted image 20240403114425.png]]

![[Pasted image 20240403114413.png]]


#### select
![[Pasted image 20240403121501.png]]

### poll
![[Pasted image 20240403200640.png]]

### epoll
![[Pasted image 20240404080830.png]]

![[Pasted image 20240404080857.png]]

### IO多路复用-事件通知机制
![[Pasted image 20240404081734.png]]


### epoll - web流程
![[Pasted image 20240404082228.png]]
### 信号驱动IO
![[Pasted image 20240404082521.png]]

### 异步IO
![[Pasted image 20240404082652.png]]

### 同步和异步
![[Pasted image 20240404082806.png]]


## Redis网络模型

redis的核心业务部分（命令部分），是单线程
整个redis是多线程
![[Pasted image 20240404083439.png]]
![[Pasted image 20240404083448.png]]

![[Pasted image 20240404092325.png]]
![[Pasted image 20240404092348.png]]
![[Pasted image 20240404092421.png]]
![[Pasted image 20240404092448.png]]
![[Pasted image 20240404092506.png]]
![[Pasted image 20240404090624.png]]

## 通信协议

### RESP协议
![[Pasted image 20240404093626.png]]
![[Pasted image 20240404094107.png]]

## 内存使用策略

### 过期策略
#### 怎么设置TTL
![[Pasted image 20240404100221.png]]
![[Pasted image 20240404100236.png]]
#### 怎么删除TTL到期的key value
##### 惰性删除
![[Pasted image 20240404100438.png]]
大量key过期后没人访问，仍然占用内存
#### 周期删除
![[Pasted image 20240404101053.png]]
![[Pasted image 20240404101511.png]]
![[Pasted image 20240404101526.png]]
### 淘汰策略
![[Pasted image 20240404101741.png]]
![[Pasted image 20240404102019.png]]
![[Pasted image 20240404102321.png]]




