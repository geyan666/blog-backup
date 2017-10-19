---
title: Head First SQL（二）
date: 2017-07-07 21:46:21
categories: 编程那点事儿
tags:
  - SQL
  - 读书
---
<blockquote class="blockquote-center">**关系型数据库** 的特性。
</blockquote>

<!--more-->

在关系型数据库中，有一些共通的特性，下面我们来简单介绍。

## **特性一: Schema**

所谓的 Schema （概要）就是数据库存数据前，需要先定义 Tables(表) 和 Columns(字段)，如下面这张表：

![](http://ogudt6aal.bkt.clouddn.com/image/20170707215157_hWC6Xj_table例子.jpeg)

有 id、name、capacity、user_id 等字段，其中每一行(row)就一笔数据(data)。

相信大家用过 Microsoft Excel 或 Mac 的 Numbers，和这个很像：

![](http://ogudt6aal.bkt.clouddn.com/image/20170707215458_cxOLVH_Numbers.jpeg)

### **数据类型**

在上面的 Numbers 表中，每一格都可以随便填，但是 Schema 不一样。我们需要定义数据类型(Data Type)。每个字段(Column)都要指定，只有符合数据类型才能存进数据库。不同数据库的数据类型大同小异，大体上有：
```
# 字符串：varchar 或 text。varchar 默认是 255 字符、text 默认是 65535 字符。

# 数字: Integer, Decimal, Float。

# Blob 二进制: 可以存放档案。但不建议把档案直接存入数据库（1-数据库塞太大不易备份和管理  2-无法针对二进制档案进行条件搜索）
由于人们对于读取档案心理也会默认比较慢，所以通常只在数据库纪录档案的 metadata(元数据)，例如文件名、大小等。
实际档案则放在档案系统上，或上传到七牛或AWS S3等空间。

# Boolean

# Date

# Time

# Datetime
```
在 Rails Migration 中，其实就是在定义这些表的字段是什么格式，如：
```
create_table :events do |t|
  t.string  :name
  t.text    :description
  t.integer :capacity
  t.integer :user_id, :null => false

  t.timestamps
end
```
[Rails 实战圣经有一份对照表](https://ihower.tw/rails/migrations-cn.html#sec2 "Rails 实战圣经有一份对照表")，Rails 会根据数据库的不同，自动对应不同的数据类型。

### **数据验证**

除了定义 Data Type，你还可以在 Schema 中定义限制 (constraint)，最基本如 `NOT NULL`（不能为空）。

数据库还可以设定数据验证，不过你会发现这跟在 Rails model 中的 validations 作用是一样的。通常我们在 Rails 里验证，而不是在数据库这层做。优缺点是：

- 在 Rails 做有弹性，用 Ruby 来写验证逻辑
- 在 DB 层做是硬条件，无法跳过

早期的数据库非常重视数据验证，因为早期的数据库用户是直接操作数据库。近年来的网络公司，用户不会直接操作数据库，而是经过网站服务器(例如 Rails)，用写好的功能间接地使用数据库，因此我们可以在 **应用层** 验证好数据即可。

## **特性二: SQL 标准语法**

关系型数据库使用 SQL (Structured Query Language - 结构化查询语言）来操作数据库，例如：
```
# 插入一笔数据到 events 表
 INSERT INTO events VALUES ("RubyConf", 100);
```

```
# 取出 events 表的所有数据
 SELECT * FROM events;
```
在 Rails 之中虽然我们没有直接撰写 SQL 句子，其实 Rails 内部是自动帮我们转换好了，如上面的句子，我们是写这样的 Ruby 代码：
```
 Event.create( :name => "RubyConf", :capacity => 100)
```
和
```
 Event.all
```
Rails 内部自动转换成 SQL 句操作数据库。

## **特性三: ACID**

### **事务**

ACID 全称 Atomicity（原子性）, Consistency（一致性）, Isolation（独立性）, Durability（耐久性） （ https://en.wikipedia.org/wiki/ACID ）。在深入解释前，我们先介绍一个关系型数据库的功能，叫做 Transaction（事务）。

想像这样一个场景：当你银行转帐时，A 的余额会减少、B 的余额会增加。如果这是两个 SQL 操作的话，我们如何保证这两个 SQL 操作是一起成功的？不能发生 A 钱变少但是 B 没有收到钱的情况！或是考虑一个更极端的场景，如果你和别人同时同一秒钟互相转帐，数据库会不会算错？

要达成这种跨 Tables 多个 SQL 操作必须同时完成(成功或失败)的需求，就必须用上 Transaction。语法是用 `BEGIN;` 和 `COMMIT;` 把 SQL 句包裹在一起：
```
# 每个 SQL 句必须用分号 `;` 代表结束
 BEGIN;
  INSERT INTO histories (user_id, amount) VALUES (1, -100);
  INSERT INTO histories (user_id, amount) VALUES (2, 100);
  UPDATE accounts SET balance=200 WHERE id=1;
  UPDATE accounts SET balance=300 WHERE id=2;
COMMIT;
# 这样 BEGIN; 和 COMMIT; 中间的所有 SQL 句，就会一起递交给数据库，要么一起成功要么一起失败
```

非常多场景会用到这个 Transaction 功能来保证数据的正确性。在 Rails 中，每个 model 的 `save` 其实都会用 Transaction 包起来，包括 model 里面的所有 callback 都会在同一个 Transaction 事务里面。

如果是跨 model 的话，你也可以用 `ActiveRecord::Base.transaction` 来包裹 Transaction 事务，例如：
```
 ActiveRecord::Base.transaction do
  A.save
  B.save
  C.save
end
```

### **ACID**

ACID 其实就是在说明 Transactin 功能：

- Atomicity 原子性：一个事务（transaction）中的所有操作，要么全部完成要么全部不完成，不会结束在中间某环节。事务在执行过程中发生错误，会回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。
- Consistency 一致性：在事务开始之前和事务结束以后，数据库的完整性没有破坏。这表示写入的资料必须完全符合所有的规则。
- Isolation 隔离性：数据库允许多个并发事务同时对其数据进行读写和修改，隔离性可以防止多个事务并发执行时由于交叉而导致数据的不一致。事务隔离分为不同级别，包括读未提交（Read uncommitted）、读提交（read committed）、可重复读（repeatable read）和串行化（Serializable）。
- Durability 持久性：事务处理结束后，对数据的修改就是永久的，即便系统故障也不会丢失。

通过 Transactions 事务功能，关系型数据库可以在多人在线同时执行多个 SQL 句的情况下，也可以保证数据的正确性。

---

下一节我们重点介绍 **SQL(Structured Query Language) 语言**。

[Head First SQL（一）](http://51world.win/2017/07/07/%E7%BC%96%E7%A8%8B%E9%82%A3%E7%82%B9%E4%BA%8B%E5%84%BF-Head%20First%20SQL-1/ "Head First SQL（一）") ：**数据库** 概念初识。
[Head First SQL（二）](http://51world.win/2017/07/07/%E7%BC%96%E7%A8%8B%E9%82%A3%E7%82%B9%E4%BA%8B%E5%84%BF-Head%20First%20SQL-2/ "Head First SQL（二）") ：**关系型数据库** 的特性。
[Head First SQL（三）](http://51world.win/2017/07/08/%E7%BC%96%E7%A8%8B%E9%82%A3%E7%82%B9%E4%BA%8B%E5%84%BF-Head%20First%20SQL-3/ "Head First SQL（三）") ：**SQL(Structured Query Language) 语言**。
[Head First SQL（四）](http://51world.win/2017/07/08/%E7%BC%96%E7%A8%8B%E9%82%A3%E7%82%B9%E4%BA%8B%E5%84%BF-Head%20First%20SQL-4/ "Head First SQL（四）") ：**数据库如何设计**。
