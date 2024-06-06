## 逻辑存储结构
![[Pasted image 20240404121050.png]]
表空间、段、区、页、行
idb
## 架构
### 内存结构
#### 缓冲池
![[Pasted image 20240404113049.png]]

#### change buffer
![[Pasted image 20240404113623.png]]

#### 自适应哈希索引
![[Pasted image 20240404113704.png]]

#### 日志缓冲区
![[Pasted image 20240404113406.png]]
### 磁盘结构

#### 系统表空间、每个表空间
![[Pasted image 20240404114727.png]]
#### 通用表空间、撤销表空间、临时表空间
![[Pasted image 20240404114606.png]]
#### 双写缓冲区和重做日志
![[Pasted image 20240404114536.png]]
### 后台线程
![[Pasted image 20240404115120.png]]




## 事务原理
![[Pasted image 20240404115305.png]]
### redo log
顺序IO（日志缓冲到日志磁盘）效率高于随机IO（缓冲池到磁盘，脏页更新到磁盘是周期性的）
![[Pasted image 20240404115952.png]]
#### undo log
![[Pasted image 20240404121126.png]]

## MVCC

### 基本概念
![[Pasted image 20240404164432.png]]
### 隐藏字段
![[Pasted image 20240404164856.png]]

### undolog版本链
![[Pasted image 20240404170008.png]]

### readview
#### 核心字段
![[Pasted image 20240404170431.png]]
#### 版本链数据访问规则
![[Pasted image 20240404170419.png]]

##### RC级别 不可重复读
![[Pasted image 20240404172043.png]]

##### RR级别 可重复读、幻读
![[Pasted image 20240404172336.png]]

### 总结
![[Pasted image 20240404172323.png]]
