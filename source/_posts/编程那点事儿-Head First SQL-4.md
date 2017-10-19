---
title: Head First SQL（四）
date: 2017-07-08 3:36:21
categories: 编程那点事儿
tags:
  - SQL
  - 读书
---
<blockquote class="blockquote-center">繁杂的数据库是 **如何设计** 得井井有序的。
</blockquote>

<!--more-->

一个数据库会包含很多表，那这些表如何按需求来设计？设计哪些表？设计哪些字段？

## **数据库规范化**

[数据库规范化](https://zh.wikipedia.org/zh-cn/%E6%95%B0%E6%8D%AE%E5%BA%93%E8%A7%84%E8%8C%83%E5%8C%96 "数据库规范化")(Normalization)是数据库设计的一个重要概念，目的是去除重复数据，保持数据一致性。实际的做法是 **将重复的字段抽出来变成另一个新的表**。

规范化分成一阶规范化、二阶规范化、三阶规范化、DK/NF规范化等不同级别，一般来说我们的应用软件会做到二阶或三阶。

通过例子来看：假设我们要设计一个场景是记录使用者「 User」参与多个活动「 Event」。

### **未规范化**

user name |	user city |	zipcode |	event 1 |	event 1 date | event 2 | event 2 date |	event 3 |	event 3 date
----------|----------|----------|----------|----------|----------|----------|----------|----------
Tom |	Beijing |	300 |	RubyConf |	2015/9/11 |	JSConf |	2015/5/1 |	CSSConf |	2016/1/1
John |	Shanghai |	100 |	RubyConf |	2015/9/11				

这种 Table 设计缺点很多：

- Tom 如果需要参加第四个活动，就必须变更 Table 的 Schema 增加更多字段。但是变更 Scmema 是一件成本很高的事。
- John 没有参加这么多活动，多余的字段都是 NULL，对数据库来说是浪费空间。
- Tom 跟 John 都有参加 RubyConf，但是 2015/9/11 这个活动日期重复存了。而且如果活动改日期，这些值都要改。

### **一阶: 移除重复语意的 columns**

刚刚的 event 1 - event 1 date, event 2 - event 2 date, event 3 - event 3 date 俩俩都是重复的，让我们消灭它们：

user name |	user city |	zipcode |	event |	event date
----------|----------|----------|----------|----------
Tom |	Beijing |	300 |	RubyConf |	2015/9/11
Tom |	Beijing |	300 |	JSConf |	2015/5/1
Tom |	Beijing |	300 |	CSSConf | 2016/1/1
John |	Shanghai |	300 |	RubyConf |	2015/9/11

优缺点:

- 相比于没有规范化的，现在新报名不需要修改 Schame 了
- 但是重复的资料更多，包括用户数据和活动数据

### **二阶: 移除重复语意的 row**

接下来需要拆表了，我们拆成三张表：users 表、events 表、registration 表，并且 users 和 events 要加上可识别的 id 字段。

在 Rails 中，这就是 User model 和 Event model 透过 Registration model 来达成多对多关系。

users table：

user id |	user name |	user city |	zipcode
----------|----------|----------|----------
1 |	Tom |	Beijing |	300
2 |	John |	Shanghai |	100

events table：

event id |	event |	event date
----------|----------|----------
1 |	RubyConf |	2015/9/11
2 |	JSConf |	2015/5/1
3 |	CSSConf |	2016/1/1

registrations table：

user id |	event id |	register_at
----------|----------|----------
1 |	1 |	2016-03-16 12:00:00
1 |	2 |	2016-03-16 12:30:00
1 |	3 |	2016-03-17 12:00:00
2 |	1 |	2016-03-18 12:00:00

这下看起来没有重复的资料了，如果要改 user 或 event 的数据，只需要改一个地方。

但是还有一个小地方可以改进，让我们继续看：

### **三阶: 移除不依赖主 ID 的资料**

在 users table 中，city 名字其实只跟 zipcode 相关，跟 user id 没关系，因此这个 city 可以拆出来。在 users table 中只需要留着 zipcode 就好了。

users table

user id |	user name |	zipcode
----------|----------|----------
1 |	Tom |	300
2 |	John |	300

zipcodes table

zipcode |	user city
----------|----------
300 |	Beijing
100 |	Shanghai

### **小结**

规范化让数据不会重复和高度一致，节省空间、增加修改数据的效率、避免数据不一致的错误。

## **数据库设计实际操作**

数据库规范化看起来好像很难，我们实际设计的时候，并不是从一阶二阶三阶这样慢慢思考的，而是用 Model 的关系来思考。

### **关联设计** （Associations）

在传统数据库中，会使用 [ER diagram](https://en.wikipedia.org/wiki/Entity%E2%80%93relationship_model "ER diagram") (entity-relationship model) 描述关联的标准图表。（定义了一对一关系、一对多关系、多对多关系等的图表，在 Rails 中用 has_one, has_many, belongs_to 等）

### **主键** （Primary Key）

主键就是可以唯一识别的字段，在 Rails 中会默认产生一个字段是 id。

主键特性：

- 不能 NULL 也不能重复
- 最常见是 Simple ID Column Key (一个 column 是主键) 设计。也可以是 Compound/Composite Key (多个 columns 组成一个主键)，但 Rails 不支持。

如何选择你的 Primary Key ?

- 最常见是自动递增整数主键(Auto incrementing Primary Key)，这是 Rails 默认方式，也是大家熟悉的 ID
- [UUID](https://zh.wikipedia.org/wiki/%E9%80%9A%E7%94%A8%E5%94%AF%E4%B8%80%E8%AF%86%E5%88%AB%E7%A0%81 "UUID") （通用唯一识别码）: 1.分布式系统喜欢用 2.当作 token URL 功能
- Natural key (例如身分证号码, ISBN, 国码) ，不过你需要确认不会重复，例如 ISBN 其实会重复的

由于 Rails 默认使用自动递增的整数当作 ID，一般不建议去改(可以自己增加别的字段来当作 Model URL)。

加主键的语法：
```
CREATE TABLE events (id INTEGER NOT NULL PRIMARY KEY, name TEXT, capacity INTEGER);
```
加自动递增整数主键(各家语法不一样，以下是 SQLite3)：
```
CREATE TABLE events (id INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT, name TEXT, capacity INTEGER);
```
这些在 Rails Migration 中已经帮我们做了。

### **外键** （Foreign Key / Reference Key）

外键是指用来关联一对多的字段，例如上述 registrations 表中的 user_id 和 event_id。外键的命名没有特别规定，通常是 _id 结尾。

你不需要特别告诉数据库这个是外键就可以当它是外键来使用。在 Rails 中，写 belongs_to 的那个 model，就是外键字段的那个 model。

在 Rails Migration 中可以用 [add_foreign_key](http://guides.rubyonrails.org/active_record_migrations.html#foreign-keys "add_foreign_key") 语法告诉数据库这个是外键，这样数据库会提供 Referential integrity (Reference constraint) 验证：

- 新增或修改数据时，要参考的数据存在，不然数据库报错
- 删除数据时，没有其他资料参考它，不然数据库报错

传统数据库设计非常重视数据的正确性，不过在 Rails 中则偏好在应用层解决，比如利用 `dependent` 属性来处理删除的情况，例如：
```
class Event < ApplicationRecord
  has_many :registrations, :dependent => :destroy
end
```
其中 `:dependent` 可以有几种不同的处理方式，例如：

- `:destroy` 把依赖的 registrations 也一并删除，并且执行 Registration 的 destroy 回呼
- `:delete` 把依赖的 registrations 也一并删除，但不执行 Registration 的 destroy 回呼
- `:nullify` 这是默认值，不会帮忙删除 registrations，但会把 registrations 的外部键 event_id 都设成NULL
- `:restrict_with_exception` 如果有任何依赖的 registrations 资料，则连 event 都不允许删除。执行删除时会丢出错误 ActiveRecord::DeleteRestrictionError。
- `:restrict_with_error` 不允许删除。执行删除时会回传 false，在 @event.errors 中会留有错误讯息。

## **逆规范化** （ denormalized）

数据库规范化并不是完全的真理，在不同场景下甚至会做逆规范化的设计。

在一般场景下，也就是运营用途([OLTP](http://baike.baidu.com/link?url=dhyUbbKFHr6Ev0NO7WHMI_53vGEYaRZNiG_ySiFUVpRL7hLlGU5c6IOEB262gZsT58nMST87vPPAx2mhYwGyKa "OLTP"))的 Schema 通常会达到比较高的规范化。但是在做分析用途([OLAP](http://baike.baidu.com/link?url=T4fXcVSd-DBQGTS-h8-cibquGEpqJWwt2Pu7gypd7oykepvoopcJzy42afAb1aCinu4nDfs2dJSA2RZ_VoMz4DjOvz62PLpHO4VhCYA-PwHloayTeBCj7LVHEVoipbAJYgDNCCcvucyVULuqkY1MVwSxiuUFEhLdOoWEenyWBHxx1jDbP5V5dt5wMsklGy_O "OLAP"))的 Schema，则会偏好逆正规化的设计。因为它不会修改数据，只会查询，所以不会有修改数据造成数据不一致的风险。这领域叫做数据仓库 [Data Warehouse](https://zh.wikipedia.org/zh-cn/%E8%B3%87%E6%96%99%E5%80%89%E5%84%B2 "Data Warehouse") 。

从 OLTP 数据库资料转到 OLAP 的过程就做 ETL (Extract-Transform-Load)。

另外，在一些需要局部效能最佳化的场景，也会做一些逆规范化的设计，例如 Rails 的 计数 (Counter Cache) 功能，将数量额外用一个字段先存下来，免去之后计算的查询时间。这也是一种逆规范化的设计。

## **Rails 相关 gem**

[rails-erd](https://github.com/voormedia/rails-erd "rails-erd") 这个 gem 可以分析 Rails 工程项目产生 ERD(entity-relationship diagram) 图表。

注意这个 gem 需要安装 graphviz 工具：

![](http://ogudt6aal.bkt.clouddn.com/image/20170708050949_6JqLaq_rails-erd.jpeg)

## **SQL 参考资料和更多教程**

网络上关于 SQL 的教程非常多，以下列出一些便于继续进修学习：

- [Udacity: Intro to Relational Databases (有简中字幕)](https://www.udacity.com/course/intro-to-relational-databases--ud197 "Udacity: Intro to Relational Databases (有简中字幕)")，建议看 Lesson 1, 2, 4，不用看 Lesson 3: Python DB-API 和 Lesson 5: Final Project。
- [LaunchSchool: Introduction to SQL](https://launchschool.com/books/sql/read/introduction "LaunchSchool: Introduction to SQL")
- [Learn SQL](https://www.codecademy.com/learn/learn-sql "Learn SQL")
- [SQL: Analyzing Business Metrics](https://www.codecademy.com/learn/sql-analyzing-business-metrics "SQL: Analyzing Business Metrics")
- [SQL Exercises](https://en.wikibooks.org/wiki/SQL_Exercises "SQL Exercises")
- [PostgreSQL Exercises](https://pgexercises.com/ "PostgreSQL Exercises")
- [SQL Tutorial](http://sqlzoo.net/wiki/SQL_Tutorial "SQL Tutorial")
- [如果有人问你数据库的原理，叫他看这篇文章](http://blog.jobbole.com/100349/ "如果有人问你数据库的原理，叫他看这篇文章")

---

[Head First SQL（一）](http://51world.win/2017/07/07/%E7%BC%96%E7%A8%8B%E9%82%A3%E7%82%B9%E4%BA%8B%E5%84%BF-Head%20First%20SQL-1/ "Head First SQL（一）") ：**数据库** 概念初识。
[Head First SQL（二）](http://51world.win/2017/07/07/%E7%BC%96%E7%A8%8B%E9%82%A3%E7%82%B9%E4%BA%8B%E5%84%BF-Head%20First%20SQL-2/ "Head First SQL（二）") ：**关系型数据库** 的特性。
[Head First SQL（三）](http://51world.win/2017/07/08/%E7%BC%96%E7%A8%8B%E9%82%A3%E7%82%B9%E4%BA%8B%E5%84%BF-Head%20First%20SQL-3/ "Head First SQL（三）") ：**SQL(Structured Query Language) 语言**。
[Head First SQL（四）](http://51world.win/2017/07/08/%E7%BC%96%E7%A8%8B%E9%82%A3%E7%82%B9%E4%BA%8B%E5%84%BF-Head%20First%20SQL-4/ "Head First SQL（四）") ：**数据库如何设计**。
