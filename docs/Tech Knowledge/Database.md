---
layout: default
title: Database
nav_order: 4
parent: Tech Knowlegde
---

# Database

## 基础知识

### ACID

Atomicity：要么完成，要么不完成，没有中间状态。发生错误，系统会回滚。

Consistency：一致性。例如现有一致性约束a+b=10，若a发生改变，b也必须被改变，以保证约束成立，否则transaction失败。

Isolation：保证并发事务之间不会互相影响，每一transaction认为只有自己在使用该系统（串行化）。同一时间只有一个请求用于同一数据。

Durability：transaction成功之后，更改永久的保存在数据库中，不会因为断电等丢失。



### CAP

CAP theorem，又称 Brewer's theorem，指出对于一个分布式系统，不可能同时满足以下三点：

1. Consistency：所有节点在同一时间拥有相同数据。
2. Acailability：保证每个请求不管成功失败都有相应。
3. Partition tolerance：系统中任意信息的丢失或失败不会影响系统继续运作。

根据三选二，系统可以被分为三大类：

1. CA：可扩展性差（RDBMS）
2. CP：性能差（MongoDB，HBase，Redis）
3. AP：一致性低（Cassandra）



### BASE

BASE：Basically Available, Soft-state, Eventually Consistent。 由 Eric Brewer 定义。

CAP理论的核心是：一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求，最多只能同时较好的满足两个。

BASE是NoSQL数据库通常对可用性及一致性的弱要求原则:

1. Basically Availble --基本可用

2. Soft-state --软状态/柔性事务。 "Soft state" 可以理解为"无连接"的, 而 "Hard state" 是"面向连接"的
3. Eventual Consistency -- 最终一致性， 也是是 ACID 的最终目的。



### ACID vs BASE

| ACID                    | BASE                                  |
| ----------------------- | ------------------------------------- |
| 原子性(**A**tomicity)   | 基本可用(**B**asically **A**vailable) |
| 一致性(**C**onsistency) | 软状态/柔性事务(**S**oft state)       |
| 隔离性(**I**solation)   | 最终一致性 (**E**ventual consistency) |
| 持久性 (**D**urable)    |                                       |



### NoSQL 数据库分类

| 类型          | 部分代表                                         | 特点                                                         |
| ------------- | ------------------------------------------------ | ------------------------------------------------------------ |
| 列存储        | HbaseCassandraHypertable                         | 顾名思义，是按列存储数据的。最大的特点是方便存储结构化和半结构化数据，方便做数据压缩，对针对某一列或者某几列的查询有非常大的IO优势。 |
| 文档存储      | MongoDBCouchDB                                   | 文档存储一般用类似json的格式存储，存储的内容是文档型的。这样也就有机会对某些字段建立索引，实现关系数据库的某些功能。 |
| key-value存储 | Tokyo Cabinet / TyrantBerkeley DBMemcacheDBRedis | 可以通过key快速查询到其value。一般来说，存储不管value的格式，照单全收。（Redis包含了其他功能） |
| 图存储        | Neo4JFlockDB                                     | 图形关系的最佳存储。使用传统关系数据库来解决的话性能低下，而且设计使用不方便。 |
| 对象存储      | db4oVersant                                      | 通过类似面向对象语言的语法操作数据库，通过对象的方式存取数据。 |
| xml数据库     | Berkeley DB XMLBaseX                             | 高效的存储XML数据，并支持XML的内部查询语法，比如XQuery,Xpath。 |



## 例子

### Bigtable

Google开发，数据分布式存储于GFS，由master-slave模式的服务器分配。
不是关系型数据库，而是一个 key-value map，其键有 row key, col key, & timestamp。
用于存储大规模结构化的数据。

### Cassandra

Facebook开发，开源分布式NoSQL数据库系统。数据存于本地，而非GFS/HDFS中。
P2P架构，基于Consistent hashing。
数据结构是Wide Column Stores，每行数据由row key唯一标识之后，可以有最多20亿个列，每个列有一个column key标识，每个column key下对应若干value。这种模型可以理解为是一个二维的key-value存储，即数据被定义成一个类似map<key1, map<key2,value>>的类型。

### HBase

Apache开发。参考了BigTable。列式的分布式数据库。底层依赖HDFS作为其物理存储。特殊情况下也可直接使用本机的文件系统。

### Hive

底层依赖HDFS作为其物理存储。

### MongoDB

NoSQL数据库，一般以类似json的key-value格式存储，value可以包含其他文档或数组。CAP-CP。
存储内容是文档型的。
可通过建立任何属性的索引实现RDBMS某些功能。

### Redis

key-value存储，通过key迅速查到value。value不仅限于字符串。Master-slave。



## 参考

