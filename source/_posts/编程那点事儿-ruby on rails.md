---
title: ruby on rails 知识点备忘
date: 2016-11-09 10:36:21
categories: 编程那点事儿
tags:
  - ruby
  - rails
---
<blockquote class="blockquote-center">身为程序员，稍稍原地踏步，不久就会变成傻逼。
</blockquote>

<!--more-->

## ruby 基本功能

- 终端机启动 IRB（Interactive Ruby Shell），Ruby 测试环境

``` bash
 $ irb (exit 退出)
```

- 删除错误的 controller 或 model

``` bash
 $ rails d controller xxx
 $ rails d model xxx (原来命令)
```

如果建立错误model以后还执行过 rake db:migrate 的话，记得在“删除错误model and 建立正确model”以后，依次执行以下三个命令，用来重建正确数据。

``` bash
 $ rake db:drop
 $ rake db:create
 $ rake db:migrate
```

 - 生成种子数据  
 如 seeds.rb: Abc.create!( :xxx => "0822") ，后执行
 ``` bash
  $ rake db:seed
 ```

## 待续...
