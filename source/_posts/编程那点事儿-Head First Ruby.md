---
title: Head First Ruby
date: 2017-06-09 10:16:21
categories: 编程那点事儿
tags:
  - Ruby
  - 读书
---
<blockquote class="blockquote-center">用更少的代码，做更多的事情。
</blockquote>

<!--more-->

## 写在前面

之前看完《Head First HTML 与 CSS, XHTML》写了个[小总结](http://51world.win/2017/05/31/%E7%BC%96%E7%A8%8B%E9%82%A3%E7%82%B9%E4%BA%8B%E5%84%BF-HTML%7CCSS%7CXHTML/ "小总结")，这是在纯野生状态下靠着兴趣玩了多半年 web 各种语言和产品（买各种云主机，搭各种博客，改各种前端）后看的第一本和网络相关完整的教材。网络上的教程和知识点毕竟只能在电子屏幕上而且碎片化，现在觉得还是手把手抓着纸质的知识心理踏实啊...

前几天想找个网络方面的码农岗，试着面试了3家结果都说理论不扎实，只好回来恶补。对于理工男来说，脸皮不值钱，一切拿技术说话。跳槽虽然是涨薪的一种方式，但是我觉得长期来看还是 **99%的能力，1%的运气**。

在没多少 Ruby 知识储备情况下，搭建了[两个 Web 应用](http://wangwei.party "两个 Web 应用")，懵懵懂懂。计算机语言之间虽然神似，可是 **基础不牢，地动山摇** 。没有理论指导，只会照猫画虎，这是肯定满足不了我的好奇心与“占有欲”。这月4号下午买的一大套书到了，快马加鞭未下鞍，到今天为止又用了 **5口气** 读完了《Head First Ruby》，现在做一下总结。

继续补充自己的 **知识 tree** 。嗯，现在开始，什么都不晚。

![5口气读完《Head First Ruby》](http://ogudt6aal.bkt.clouddn.com/image/5%E5%8F%A3%E6%B0%94%E8%AF%BB%E5%AE%8C%E3%80%8AHead%20First%20Ruby%E3%80%8B.jpeg "5口气读完《Head First Ruby》")

## Head First Ruby

### Ruby 的哲学

- 解释型语言，执行前无需编译
- 一切都是对象

### 基本语法元素 和 面向对象

- super
- yield 调用块
- each 简练

### 散列

### include module(mixin) - 模块

- 不能initialize
- ||= 操作符赋值

#### 混入 Comparable（飞船操作符 <=>） 和 Enumerable(处理集合相关)

### 异常

- raise "abc" 后的代码不会执行，直接跳出

``` bash
begin
  xxx.method1
  puts "show #{xxx.method2}"
rescue xxxError => error
  puts "Error: #{error.message}"
  retry #返回到begin开始处；当心死循环
ensure
  xxx.method3
end
```

### 单元测试

- MiniTest 库

```bash
require 'minitest/autorun'
class xxx < Minitest::test
  def setup
    @aaa = AAA.new
    @aaa.b = c
  end

  def test_yyy
    zzz
  end
end

  def teardown
    @aaa.b = d
  end
```

### 一个 Web 例子

![一个 Web 例子](http://ogudt6aal.bkt.clouddn.com/image/%E4%B8%80%E4%B8%AA%20Web%20%E4%BE%8B%E5%AD%90.jpeg "一个 Web 例子")

- Sinatra 库示例
  - 需求介绍:
   - 建立（建立工程目录；安装 Sinatra 库处理 Web 请求）
   - 处理请求（建立路由得到电影列表；创建 HTML 页面）
   - 用 HTML 显示对象（显示对象列表；增加新电影对象的表单）
   - 保存和加载对象（创建 - 保存 - 加载 - 查找 - 显示）
  - 代码仓库: https://github.com/taketimeasafriend/sinatra-movies
  - 代码文件目录:
```bash
proj folder
|
|
|---app.rb(核心，引用头文件 - 创建db.yml存数据 - 添加路由 - erb :index/new/show)
|
|---aaa.yml(注意键值匹配)( http://yaml.org/ )
|
|---lib folder
|     |
|     |---aaa.rb(attr_accessor :xxx, :yyy, :zzz, :id)
|     |---aaa_store.rb(initialize - find - all - save)
|
|---views
      |
      |---index.erb(<%= @aaa.each do |aaa| %>)
      |---new.erb(<form method="post" action="/aaa/create">)
      |---show.erb(<%= @aaa.b %>)
```

### 剩下的

#### ruby on rails(erb模版 - 数据库配置 - JavaScript和css)

- http://rubyonrails.org/

#### 分布式dRuby

```bash
require 'drb/drb' #注意防火墙
```
#### CSV

#### 执行文件加参数

```bash
$ ruby xxx.rb test.txt

file = File.open(ARGV[0]) do |file|
```

#### 正则表达式
- ／ 夹
- =～
- {number} #重复次数
- #{$1} #记录表达式的值
- .sub 替换匹配部分

#### 单例方法(实例上定义)
#### 为定义的参数传给 method_missing
#### Rake 工具
#### bundler 控制gem版本
#### 文档和书籍
- 搞书《Programming Ruby》(工具书)
- 《Well-Grounded Rubyist》（深度解析）

---
### 长路漫漫任我闯

Ruby 只是个语法和概念基础，Rails 的路还很长，囊括的内容很碎（常用的 Gem，构建 API，各种 Ruby 技巧提升效率），之后接着更新博客记录，接着 coding...
