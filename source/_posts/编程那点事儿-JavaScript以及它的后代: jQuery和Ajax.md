---
title: JavaScript 以及它的后代：jQuery 和 Ajax
date: 2017-07-04 10:36:21
categories: 编程那点事儿
tags:
  - JavaScript
  - jQuery
  - Ajax
---
<blockquote class="blockquote-center">学习之前最重要的是先要了解各种基本的 **概念** 和它们的 **关联**。
</blockquote>

<!--more-->

## 写在前面

写 web，在掌握了[基本的 html 和 css 之后](http://51world.win/2017/05/31/%E7%BC%96%E7%A8%8B%E9%82%A3%E7%82%B9%E4%BA%8B%E5%84%BF-HTML%7CCSS%7CXHTML/ "基本的 html 和 css 之后")，就该向 JavaScript 进发了。

学习之前听同事说这东西 **入门易**（毕竟现在网页数量井喷式爆发，随手拿来各种源代码修修补补就能用）**精通难**（比如阮一峰好像现阶段就在阿里写 JS）。

无论学习什么知识，学习之前最重要的是先要了解各种基本的 **概念** 和它们的 **关联**，甚至它们的 **发展历史**。

## JavaScript - jQuery - Ajax 概览

### JavaScript：
<blockquote>
- 常用来为网页添加各种动态功能，为用户提供更流畅美观的效果（如：数据验证处理、页面动态效果、定时任务、与用户交互、发送/接收服务器端数据等）
- 通过嵌入在 HTML 中来实现，直接在浏览器中解释执行（脚本语言）
- 主流框架：YUI，Dojo，Prototype，**jQuery** ...
- 组成部分：核心(ECMAScript)、文档对象模型(Document Object Model，简称DOM)、浏览器对象模型(Browser Object Model，简称BOM)
</blockquote>

### Ajax：
<blockquote>
- “ Asynchronous JavaScript and XML ” (异步JavaScript和XML)，不是一个技术，是 **几种技术**（XML、CSS 以及 JavaScript 等）
- Web 页面不用打断交互进行重新加裁，就可以动态地更新，接近本地桌面应用的直接、更动态的Web用户界面
- 使用 XML HttpRequest 与服务器进行异步通信
</blockquote>

### jQuery：
<blockquote>
- 快速简洁的 JavaScript 库（集合 **Ajax** 技术的 **JS** 库，封装 JS 和 Ajax 的功能，提供接口），使用户能更方便处理 HTML documents、events、动画，方便地为网站提供Ajax交互
- Query 是 “ 查询 ” 的意思（基于 JavaScript 的查询，查询目标：DOM 结构中的 Node（节点）
- 独立、链式语法、CSS1-3选择器、跨浏览器
- 插件多
</blockquote>

<br>
让我们用一张图对三者做个总结：

![JavaScript 以及它的后代：jQuery 和 Ajax 关系](http://ogudt6aal.bkt.clouddn.com/image/JavaScript%20%E4%BB%A5%E5%8F%8A%E5%AE%83%E7%9A%84%E5%90%8E%E4%BB%A3%EF%BC%9AjQuery%20%E5%92%8C%20Ajax%20%E5%85%B3%E7%B3%BB.png "JavaScript 以及它的后代：jQuery 和 Ajax 关系")

这里需要多介绍一个网页开发经常用到的东西 —— **JSON**（JavaScript Object Notation - js 对象标记法）。

从名称可以看出，JSON 是基于 JavaScript的，是 JS 的一个子集。JSON 是用 JS 语法来表示数据的一种轻量级语言（相比较上面说的 XML 这种结构复杂的语言）。

另外还有 **Node.js**，这里先不讨论，以后按需补充。

更多关于它们的关系，可以[点击这里查看](https://www.zhihu.com/question/31305968 "点击这里查看")。

## 继续学习（待续）

### 《Head First jQuery》

http://51world.win/2017/07/04/%E7%BC%96%E7%A8%8B%E9%82%A3%E7%82%B9%E4%BA%8B%E5%84%BF-Head%20First%20jQuery/

### 《Head First Ajax》

http://51world.win/2017/07/04/%E7%BC%96%E7%A8%8B%E9%82%A3%E7%82%B9%E4%BA%8B%E5%84%BF-Head%20First%20Ajax/
