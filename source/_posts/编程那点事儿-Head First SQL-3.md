---
title: Head First SQL（三）
date: 2017-07-08 0:48:21
categories: 编程那点事儿
tags:
  - SQL
  - 读书
---
<blockquote class="blockquote-center">**SQL(Structured Query Language) 语言**。
</blockquote>

<!--more-->

关系型数据库使用 SQL(Structured Query Language) 语言，每个 SQL 句子叫做 SQL Query 或 SQL Statement。我们可以用 CLI 指令或 GUI 软件，用 SQL 语言对数据库进行查询和操作。SQL 分成 DDL 和 DML 两种，都是用分号 `;` 结尾。下面我们分别介绍，最后稍微引入一点高级用法。

## **Data Definition(定义) Language - DDL**

告诉数据库去定义前面说的 Schema。

每家方法不太一样，建议会用 GUI 即可。PostgreSQL 和 MySQL 都是数据库服务器，可以管理很多数据库（例如你可以架很多 Rails 网站，但只需一个数据库服务器，里面建立不同数据库）。

建立数据库时，请注意选择编码(Encoding)。PostgrSQL 可用 `utf8`，MySQL 可用 `utf8mb4`（mb4就是most bytes 4的意思，专门用来兼容四字节的unicode）。

以下主要用 SQLite3 举例。

### **新建**

#### **建立数据库档案**

在 Terminal 输入 `sqlite3 your_db_name.db` 就会打开(或产生)一个 **数据库**。(MySQL： `mysql -u root -p`，PostgreSQL： `psql <database_name>`。)

#### **建立 Table**

```
# 建立 events 表，并新增三个字段 `name`, `capacity` 和 `date`。默认是字段允许 NULL，除非加上 NOT NULL。
 CREATE TABLE events (name VARCHAR(50) NOT NULL, capacity INTEGER, date DATE);
```

![](http://ogudt6aal.bkt.clouddn.com/image/20170708013142_FSDdvl_建立Table.jpeg)

可以用 SQLite3 的 GUI DB Browser for SQLite 打开 demo.db 档案：

![](http://ogudt6aal.bkt.clouddn.com/image/20170708013339_g31de8_GUI-DB-Browser-for-SQLite.jpeg)

用 GUI 进行 Schame 操作比较简单，在 Rails 统一都用 Migrations 机制来变更 Schema。

### **修改**

- Table 重命名：如 `ALTER TABLE persons RENAME TO people;`
- 新增字段：如 `ALTER TABLE people ADD COLUMN status VARCHAR(50);`
- 修改和移除字段：SQLite3 不支持，需新建一个 table 后把资料复制过去

### **删除**

Table 删除： `DROP TABLE IF EXISTS people;`

通常不会让终端使用者动态新建 table 或修改 schema。数据库的 Schema 像是程序的一部分，代码会依赖于 Schema 设计。另外也有效能的考量，变更 Schema 操作很耗时，特别是数据很大的情况下，修改 Schema 会锁住整个 Table 从而影响网站正常运行。

### **Migration 机制**

数据库 Schema 不是一成不变的，会随着软件变更升级也需要修改。因此，一些软件会实现一种叫做 Migration 的功能（如 Rails Migration），通过 Schema Migration 记录当前 schema 版本。开机时会检查当前程序版本和数据库版本是否相同，不同则执行 Migration 更新 schema。这些 Migration 代码也会放进版本控制系统 Git 里面，这样整个团队的开发者或不同服务器都可利用 Migration 来一致管理 Schame。

## **Data Manipulation(操作) Language - DML**

操作数据的 SQL 就是 DML，即 CRUD(Create, Read, Update, Delete)。

### **新增**

```
# 新增数据 capacity=200 name="JSConf" 到 events 这个 table 中
INSERT INTO events (capacity, name) VALUES (200, "JSConf");
```

对应的 Rails 语法是：

```
Event.create( :capacity => 200, :name => "JSConf")
```

在 GUI 的视窗中查看：

![](http://ogudt6aal.bkt.clouddn.com/image/20170708014959_ilCqkf_INSERT-INTO1.jpeg)

插入多笔数据 `INSERT INTO events (capacity, name) VALUES (300, "COSCUP"), (300, "OSDC.TW");`

也可以在 GUI 的视窗中，练习输入 SQL 句：

![](http://ogudt6aal.bkt.clouddn.com/image/20170708015108_idt9eb_INSERT-INTO2.jpeg)

### **查询**

#### **一般查找**

```
# 取出全部 events 资料
SELECT * FROM events;
# 对应的 Rails 语法
Event.all

# 只取出指定的字段
SELECT name, capacity FROM events;
# 字段前面也可以补上表的名称
SELECT events.name, events.capacity FROM events;
# 对应的 Rails 语法
Event.select(:name, :capacity).all

# 排序
SELECT name, capacity FROM events ORDER BY created_at;

# 分页
SELECT name, capacity FROM events ORDER BY capacity DESC, name ASC LIMIT 20 OFFSET 20;

# 取出第一笔
SELECT * FROM events ORDER BY id LIMIT 1
# 对应的 Rails 语法
Event.order("id").first
```

特殊的，比如：

#### **查找有哪些 tables 和 columns**

各家语法不一样：
<blockquote>
SQLite3: `.tables` 和 `.schema tablename`
MySQL: `show tables` 和 `describe tablename`
PostgreSQL: `\dt` 和 `\d tablename`
</blockquote>
这些查询在 Rails 启动后，也会帮我们做。你可以在 rails console 中对 model 执行 columns 方法，例如 `Event.columns` 就会取出这个表有哪些字段。

#### **特殊条件查找**

```
SELECT * FROM events WHERE date = '2015-03-15';
SELECT * FROM events WHERE date = '2015-03-15' OR date = '2015-03-16'; # 或
SELECT * FROM events WHERE date = '2015-03-15' AND capacity >= 100; # 且
SELECT * FROM events WHERE date BETWEEN '2015-03-15' AND '2015-03-30'; # 区间

SELECT * FROM events WHERE name LIKE '%Ruby%'; # 模糊比对

SELECT * FROM events WHERE description IS NOT NULL;
```

小心大小写。不同数据库默认不同。MySQL 不分大小写(case insensitive)，PostgreSQL 区分大小写(case sensitive)。

#### **Indexes 索引**

上面的 WHERE、ORDER 条件字段最好都要加上数据库索引(Index)，如果没有索引的话，是 O(n) 的效率(这里又叫作 Full Table Scan，需要扫描整个表)，数据库越大越慢。如果有索引，是 O(logn)，在数据量大的情况二者相差非常大。

模糊搜寻 LIKE 查询都会变成 Full Table Scan。（ Rails 中 [ransack](https://github.com/activerecord-hackery/ransack "ransack") gem 是用 LIKE 语法，几万笔数据还能接受，再大的数据量就需要用另外的 Full-Text Searching 引擎了，例如 [ElasticSearch](https://www.elastic.co/ "ElasticSearch") 。 )

加索引的 SQL 语法：

```
# 加索引
CREATE INDEX events_user_id_idx ON events(user_id);
# 索引并且值是唯一
CREATE UNIQUE INDEX xxx_idx ON xxx(yyy);

# 在 Rails Migration 中要加上索引的话，可用 add_index 语法
add_index :events, :date。
```

将字段设成 `unique` 跟设成 `unique index` 是一样的。

当然也不是所有字段通通都加上索引就好了，因为加索引会让写入数据变慢(因为要建立索引，也会增加储存空间)，但是查询时会加快。

### **修改**

```
# 修改 events table 的所有数据把 capacity 改成 10
UPDATE events SET capacity=10;
# 对应的 Rails 语法
Event.update_all( :capacity => 100 )

# 用 WHERE 可以指定只修改 name 是 "RubyConf" 的数据
UPDATE events SET capacity=100 WHERE name="RubyConf";
# 对应的 Rails 语法
Event.where( :name => "RubyConf" ).update_all( :capacity => 100)

# 修改 capacity 和 name
UPDATE events SET capacity=100, name="RubyConf 2015" WHERE name="RubyConf";
# 对应的 Rails 语法
Event.where( :capacity => 100, :name => "RubyConf" ).update_all( :capacity => 100)
```

在 Rails 中，比较常见只修改一笔数据，如：
```
@event = Event.find(123)
@event.update( :capacity => 200)
# 对应的 SQL：
SELECT * FROM events WHERE id = 123;
UPDATE events SET capacity=200 WHERE id=123;
```
### **删除**

```
# 全部删除
DELETE FROM events;
# 对应的 Rails 语法
Event.delete_all

# 只删除 name="RubyConf"
DELETE FROM events WHERE name="RubyConf";
# 对应的 Rails 语法
Event.where( :name => "RubyConf" ).delete_all
```

在 Rails 中，比较常见只删除一笔数据，如：
```
@event = Event.find(123)
@event.destroy
# 对应的 SQL：
SELECT * FROM events WHERE id = 123;
DELETE FROM events WHERE id = 123;
```

---

## **高级操作**

### **Joins**

SQL 查询厉害在于，可以同时关联(Join)多张表来进行复杂查询。我们用个例子说明。先准备数据：

```
# 建立 user 一对多 events。执行 `sqlite3 test.db`，并输入以下 SQL：
CREATE TABLE events (id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT, name TEXT, capacity INTEGER, user_id INTEGER);
CREATE TABLE users (id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT, name TEXT);
INSERT INTO users (name) VALUES ('ihower');
INSERT INTO users (name) VALUES ('john');
INSERT INTO users (name) VALUES ('roy');
INSERT INTO events (name, capacity, user_id) VALUES ('rubyconf',100, 1);
INSERT INTO events (name, capacity, user_id) VALUES ('jsconf', 200, 1);
INSERT INTO events (name, capacity, user_id) VALUES ('cssconf', 150, 2);
INSERT INTO events (name, capacity, user_id) VALUES ('htmlconf', 300, NULL);
```

建立好的数据库如下图：

![](http://ogudt6aal.bkt.clouddn.com/image/20170715034239_2hl89u_test.db.jpeg)

跨 Tables 进行 Join 查询，常用的有 Inner Join 和 Outer Join 两种：

#### **Inner Join** （合并两 tables，接不起来就不要）

```
# 取出所有活动，以及该活动参与者资料：

SELECT * FROM events INNER JOIN users ON events.user_id = users.id;
# 或
SELECT * FROM events, users WHERE events.user_id = users.id;
```

结果如图：

![](http://ogudt6aal.bkt.clouddn.com/image/20170715035410_GIaGY3_Inner-Joining.jpeg)

对应的 Rails 语法是 `User.joins(:events)` 。

#### **Outer Join**（合并两张 tables，接不起来就填 NULL）

这里我们介绍 **Left Outer Join** 。

```
# 取出所有活动，以及该活动的参与者资料(包括没有参与者的活动)
SELECT * FROM events LEFT OUTER JOIN users ON events.user_id = users.id;
```

结果如图：

![](http://ogudt6aal.bkt.clouddn.com/image/20170715040300_64cb96_Left-Outer-Joining.jpeg)

#### **AS 语法**

因为有多张 tables 在 SQL 时，**column 最好加上 table name 当作 prefix(前缀) (特别是有重复的 column name 时，在 WHERE 条件里可能会无法判断)**，或者可以 **加上别名 AS**。

例如：

```
SELECT events.id AS event_id, events.capacity AS ec, events.name FROM events INNER JOIN users ON events.user_id = users.id WHERE ec=100;
```

#### **Rails 的 includes 原理**

Rails ActiveRecord 的 includes 方法(当没有 WHERE 过滤条件时)则采用另一种 SQL 策略来作 Outer joining。

例如 `Event.includes(:user)` 这个 Rails 语法，实际上的 SQL 是：

```
SELECT * FROM events;
# 然后透过程式组合出所有的 user_id 成为 (如：1, 2, 3, 5, 9, 11, 14)，然后再
SELECT * FROM users WHERE users.id IN (1, 2, 3, 5, 9, 11, 14);
# 不过，如果有 users 身上有 WHERE 条件的话，Rails 会变成 LEFT OUTER JOIN。
```

#### **SQL Joins 图表**

更多的关系可以参考下表：（详细戳 [Visual Representation of SQL Joins](https://www.codeproject.com/Articles/33052/Visual-Representation-of-SQL-Joins "Visual Representation of SQL Joins")）

![](http://ogudt6aal.bkt.clouddn.com/image/20170715041722_umWpUc_SQL-Joins图表.jpeg)

下图更清楚说明不同 Join 差别：（详细戳 [SQL Joins Better](https://blog.jooq.org/2016/07/05/say-no-to-venn-diagrams-when-explaining-joins/ "SQL Joins Better")）

![](http://ogudt6aal.bkt.clouddn.com/image/20170715042235_UFjKDe_SQL-Joins-Better.jpeg)

### **Functions**

数据库也有提供一些 Function 可以用在 SQL 里面：

#### **Aggregations** (计算)

(加上 AS 别名比较好识别处理)

```
# 数量
SELECT COUNT(*) AS event_count FROM events;
```
对应的 Rails 语法是 `Event.count`

```
# 最小和最大值
SELECT MIN(capacity) as min_capacity FROM events;
SELECT MAX(capacity) as max_capacity FROM events;
```
对应的 Rails 语法是 `Event.minimum(:capacity)` 和 `Event.maximum(:capacity)`

```
# 总和
SELECT SUM(capacity) as sum_capacity FROM events;
```
对应的 Rails 语法是 `Event.sum(:capacity)`

```
# 平均
SELECT SUM(capacity) / COUNT(capacity) as avg_capacity FROM events;
或
SELECT AVG(capacity) as avg_capacity FROM events;
```
对应的 Rails 语法是 `Event.average(:capacity)`

#### **GROUP BY** (分类)

GROUP BY 功能主要用来搭配上述 aggregating 来使用，例如请回答这个问题：计算每个 user 有多少 events?

让我们试试看：

```
SELECT user_id, COUNT(*) FROM events;
# 这样不行，user_id 只会回传第一笔 user_id

SELECT user_id, COUNT(*) FROM events GROUP BY user_id;
# 还是不行，没有 user 的名字

SELECT users.name, COUNT(events.id) FROM events JOIN users ON users.id = events.user_id GROUP BY user_id;
# 不对，结果少了 Roy

SELECT users.name, COUNT(events.id) FROM users LEFT JOIN events ON users.id = events.user_id GROUP BY user_id;
# 正确答案

SELECT users.name, COUNT(events.id) AS c FROM users LEFT JOIN events ON users.id = events.user_id GROUP BY user_id HAVING c > 1 ORDER BY c DESC;
# 可再加条件和排序(其中 WHERE 是给 source tables 的条件，HAVING 才是 aggregation 后的条件)
```

![](http://ogudt6aal.bkt.clouddn.com/image/20170715043435_SD9e5f_GROUPBY.jpeg)

#### **DISTINCT** (去重)

```
SELECT DISTINCT(user_id) FROM events;
```

#### **其他**

数据库还提供其他函数，例如 SQLite 的 [Useful Functions](http://www.tutorialspoint.com/sqlite/sqlite_useful_functions.htm "Useful Functions")、 [Date And Time Functions](https://www.sqlite.org/lang_datefunc.html "Date And Time Functions") 等。

不过通常比较少用，因为我们偏好取出数据后，交由 Ruby 处理。

### 为何用高级操作

回头想想看为什么需要 Join 语法。SQL 的 Join 语法对新手比较困难，没办法完全掌握。很多时候我们在 Rails 先将需要的数据取出来，然后用 Ruby 进行过滤跟组合似乎也可达成目标，为什么需要用很复杂的 SQL Joining 语法?

主要的原因是 **查询速度** 和 **内存空间** 。数据库是一套针对 SQL 优化非常快速的软件，可以用远比 Ruby 高效的方式来取出数据。如果全部的数据都拿出来用 Ruby 处理，很可能内存不够。例如：`请回答去年第三季所有商品的销售额，并根据分类计算总额。`去年一整年的销售可能上百万笔数据，如果要逐笔捞出用 Ruby 处理，效能会非常低。这时候就必须用 SQL 精准地捞出想要的数据。

---

下一节我们来介绍 **数据库如何设计**。

[Head First SQL（一）](http://51world.win/2017/07/07/%E7%BC%96%E7%A8%8B%E9%82%A3%E7%82%B9%E4%BA%8B%E5%84%BF-Head%20First%20SQL-1/ "Head First SQL（一）") ：**数据库** 概念初识。
[Head First SQL（二）](http://51world.win/2017/07/07/%E7%BC%96%E7%A8%8B%E9%82%A3%E7%82%B9%E4%BA%8B%E5%84%BF-Head%20First%20SQL-2/ "Head First SQL（二）") ：**关系型数据库** 的特性。
[Head First SQL（三）](http://51world.win/2017/07/08/%E7%BC%96%E7%A8%8B%E9%82%A3%E7%82%B9%E4%BA%8B%E5%84%BF-Head%20First%20SQL-3/ "Head First SQL（三）") ：**SQL(Structured Query Language) 语言**。
[Head First SQL（四）](http://51world.win/2017/07/08/%E7%BC%96%E7%A8%8B%E9%82%A3%E7%82%B9%E4%BA%8B%E5%84%BF-Head%20First%20SQL-4/ "Head First SQL（四）") ：**数据库如何设计**。
