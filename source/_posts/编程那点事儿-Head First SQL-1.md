---
title: Head First SQL（一）
date: 2017-07-07 19:36:21
categories: 编程那点事儿
tags:
  - SQL
  - 读书
---
<blockquote class="blockquote-center">**数据库** 概念初识。
</blockquote>

<!--more-->

编程语言虽然有的提供好用的 ORM(Object Relational Mapping-对象关系映射) 库来操作数据库，但是了解 SQL 基础仍非常重要，忽略它可能造成未知的 **效能问题**。

另外，**数据分析** 相关的应用，例如制作报表等等，也需要更加熟悉进阶的 SQL 查询语法。现在我们就来熟悉基本 SQL 和数据库设计相关概念，掌握必备的最少知识。

为了保证介绍到位，文章比较长，于是我把它分成了几部分形成了一个小系列，耐心读完后再碰见数据库就再也不害怕了。

（说明：以下举例基于 Mac 和 Rails）

## **数据库含义**

简单说，数据库的作用就是为了保存和读取数据。

程序运行时的数据会放在内存中，但是只要程序结束或关机就消失了。需要永久保存数据的话，需要存在硬盘上。比如我们可以利用简单的文本文件当作档案通过读写来将数据放在硬盘上。不过当数据又多又杂、档案怎么命名、档案用什么格式才方便之后查询... 这些都是问题。

数据库就是为解决这些问题而发明，除了可以保存大量数据，还提供了方便的查询(Query)机制，甚至 CRUD(Create, Read, Update, Delete) 操作。

## **数据库分类**

### **关系型数据库**

数据库有很多种，其中一种叫做 **「关系型数据库」** (RDBMS: Relational Database Management System) 是目前最普遍使用的。这种数据库包括开源的（SQLite、MySQL、PostgreSQL）和付费的（Oracal、Microsoft SQL Server）等。简单介绍如下：

- **SQLite** - https://www.sqlite.org/
<blockquote>
1 - 轻量级（Rails 默认），不是单独的服务器，而是集成在我们程序(比如 Rails)之中，一个数据库就是一个档案。（比如在 Rails 中是 `db/development.sqlite3` 这个档案）
2 - 主要用于单机，比如移动装置（手机），不适合多人在线使用的场景。（在实际部署 Rails 到服务器时，我们会换成下面介绍的 PostgreSQL 或 MySQL 等数据库服务器）
3 - Mac 本身内建 SQLite3，下载 [DB Browser for SQLite](http://sqlitebrowser.org/ "DB Browser for SQLite") 这个 GUI 软件，就可以打开 SQLite3 的数据档案。
</blockquote>

- **MySQL** - https://www.mysql.com/
<blockquote>
1- 目前最流行的开源数据库，易上手、资源多、效能好，最多网络公司使用的数据库。
2- 是一个数据库服务器，除了安装还必须让 MySQL 服务器在后台运行，要连接 MySQL 也必须输入帐号密码。
3- Mac 用 `brew install mysql` 安装，推荐再安装 [Sequel Pro](https://www.sequelpro.com/ "Sequel Pro") 这套 GUI 软件。
</blockquote>

- **PostgreSQL** - https://www.postgresql.org/
<blockquote>
1- 常与 MySQL 相提并论(Heroku 服务器默认)，功能比 MySQL 多，但在超大量数据场景不如 MySQL 有更多的分布式服务器经验。
2- Mac 用 `brew install postgresql` 安装，GUI 可以考虑安装 [pgAdmin](https://www.pgadmin.org/ "pgAdmin")。
</blockquote>

- **Oracle** 和 **MS SQL Server**
<blockquote>
1- 这两家公司提供的付费数据库。通常非软件开发的大型企业(例如银行、保险业等)会采购。
2- 有更好的自调优功能。
</blockquote>

### **NoSQL**

世界上除了 **「关系型数据库」**，还有不使用 SQL 的，泛称 NoSQL。分类如下：

- **Key-value**
<blockquote>
Redis（ https://redis.io/ ）是一种小型数据结构，作为搭配用的数据库(主要是局部的密集性写入的功能，降低主数据库的负担)。用途包括：
<blockquote>
# 各式计数器：+1
# 简易标记(例如同一 IP 十分钟内只算一次 pageview)
# 排行榜 zadd
# 自制 Inverted-Index Text Search
# Pub/Sub 聊天室
# Job Queue (例如 sidekiq)
</blockquote>
</blockquote>

- **Document-Oriented**
<blockquote>
MongoDB (https://www.mongodb.com/) 是一种泛用型的数据库。特色是数据的结构 Schema 不需要提前定义。（ 在 Rails 社区曾非常流行，因为不需 Migration 去定义 Schema 就可使用 Model。一开始非常方便，后来大家发现营运久了以后，没有 Schema 的数据会变脏，造成维运成本增加。后来就不流行了。）
</blockquote>

- **Column-Oriented**
<blockquote>
1 - 关系型数据库缺点是效能，在处理具体数据时必须锁住一些数据来避免其他人同时修改。因此如果一个写入流量非常大的网站（如一个售票网站，非常多人在开票时准时抢票）效能非常差。特别是当数据非常巨量的时候，需要多台数据库服务器时，写入速度就会成为扩充的瓶颈。
2 - 根据 [CAP 定理](https://zh.wikipedia.org/zh-cn/CAP%E5%AE%9A%E7%90%86 "CAP 定理")告诉我们：关系型数据库在多服务器(P)架构下为了维持 C(一致性) 特性，只能牺牲 A(可用性)。NoSQL 让你有折衷（tradeoff）的空间，牺牲 C 特性以换得 A。但这不是 C 和 A 二元的博弈，而是 C 和 延迟时间的取舍（你愿意容忍多少延迟时间才算作不 A）。
3 - 因此这类型的 NoSQL 不讲 [ACID](http://baike.baidu.com/link?url=ZFDsz3yKGE1D_L6X58c7iJ3Ch5I9_LBiGcAm53WYMvuMNxHGT6UIfcmCjDUu12OoxXFCzf3KRc6z8ABadpW5zq "ACID")，而是讲 BASE 特性 (Basically Available, Soft state, Eventual consistency)，重点是 Eventual consistency(最后结果一致)。想像一个场景: Facebook、Twitter和新浪微博发“微博”，当发送成功时，并不是马上其他人就能看到你的微博，这中间是有延时的。这个延时对关系型数据库是不可接受的，但对这种社交应用来说却没有关系。牺牲一致性，就可以换到更多的写入反应效能，这就是这类 NoSQL 的设计目的。
举例：（如果你的数据量不到 1PB (=1000TB)就不需要考虑这类数据库了，MySQL 或 PostgreSQL 足矣）
<blockquote>
# Apache HBase - https://hbase.apache.org/
# Apache Cassandra - http://cassandra.apache.org/
# Amazon DynamoDB - https://aws.amazon.com/cn/dynamodb/
# Google Cloud Datastore - https://cloud.google.com/datastore/
</blockquote>
</blockquote>

- **Graph: neo4j 图形数据库**
<blockquote>
neo4j（ https://neo4j.com/ ）用节点和边来储存数据。
</blockquote>

---

下一节我们重点介绍 **「关系型数据库」** 的特性。

[Head First SQL（一）](http://51world.win/2017/07/07/%E7%BC%96%E7%A8%8B%E9%82%A3%E7%82%B9%E4%BA%8B%E5%84%BF-Head%20First%20SQL-1/ "Head First SQL（一）") ：**数据库** 概念初识。
[Head First SQL（二）](http://51world.win/2017/07/07/%E7%BC%96%E7%A8%8B%E9%82%A3%E7%82%B9%E4%BA%8B%E5%84%BF-Head%20First%20SQL-2/ "Head First SQL（二）") ：**关系型数据库** 的特性。
[Head First SQL（三）](http://51world.win/2017/07/08/%E7%BC%96%E7%A8%8B%E9%82%A3%E7%82%B9%E4%BA%8B%E5%84%BF-Head%20First%20SQL-3/ "Head First SQL（三）") ：**SQL(Structured Query Language) 语言**。
[Head First SQL（四）](http://51world.win/2017/07/08/%E7%BC%96%E7%A8%8B%E9%82%A3%E7%82%B9%E4%BA%8B%E5%84%BF-Head%20First%20SQL-4/ "Head First SQL（四）") ：**数据库如何设计**。
