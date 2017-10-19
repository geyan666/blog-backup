---
title: CSS 选择器
date: 2017-07-05 15:36:21
categories: 编程那点事儿
tags:
  - CSS
  - 读书
---
<blockquote class="blockquote-center">CSS 的 44 个选择器，基本涵盖了 CSS 2 和 CSS 3 的所有规定。
</blockquote>

<!--more-->

以下罗列包括 44 个选择器，基本涵盖了 CSS 2 和 CSS 3 的所有规定。

## 基本选择器

选择器	| 含义
----|----
*	| 通用元素选择器，匹配任何元素
E	| 标签选择器，匹配所有使用E标签的元素
.info |	class选择器，匹配所有class属性中包含info的元素
#footer |	id选择器，匹配所有id属性等于footer的元素

举例：

```
* { margin:0; padding:0; }
p { font-size:2em; }
.info { background:#ff0; }
p.info { background:#ff0; }
p.info.error { color:#900; font-weight:bold; }
#info { background:#ff0; }
p#info { background:#ff0; }
```

## 多元素的组合选择器

选择器	| 含义
----|----
E,F	| 多元素选择器，同时匹配所有E元素或F元素，E和F之间用逗号分隔
E F	| 后代元素选择器，匹配所有属于E元素后代的F元素，E和F之间用空格分隔
E > F	| 子元素选择器，匹配所有E元素的子元素F
E + F	| 毗邻元素选择器，匹配所有紧随E元素之后的同级元素F

举例：

```
div p { color:#f00; }
#nav li { display:inline; }
#nav a { font-weight:bold; }
div > strong { color:#f00; }
p + p { color:#f00; }
```

## CSS 2.1

### 属性选择器

选择器	| 含义
----|----
E[att]	| 匹配所有具有att属性的E元素，不考虑它的值。（注意：E在此处可以省略，比如"[cheacked]"。以下同）
E[att=val]	| 匹配所有att属性等于"val"的E元素
E[att~=val]	| 匹配所有att属性具有多个空格分隔的值、其中一个值等于"val"的E元素
E[att&#124;=val]	| 匹配所有att属性具有多个连字号分隔（hyphen-separated）的值、其中一个值以"val"开头的E元素，主要用于lang属性，比如"en"、"en-us"、"en-gb"等等

举例：

```
p[title] { color:#f00; }
div[class=error] { color:#f00; }
td[headers~=col1] { color:#f00; }
p[lang|=en] { color:#f00; }
blockquote[class=quote][cite] { color:#f00; }
```

### 伪类

选择器	| 含义
----|----
E:first-child	| 匹配父元素的第一个子元素
E:link	| 匹配所有未被点击的链接
E:visited	| 匹配所有已被点击的链接
E:active	| 匹配鼠标已经其上按下、还没有释放的E元素
E:hover	| 匹配鼠标悬停其上的E元素
E:focus	| 匹配获得当前焦点的E元素
E:lang(c)	| 匹配lang属性等于c的E元素

举例：

```
p:first-child { font-style:italic; }
input[type=text]:focus { color:#000; background:#ffe; }
input[type=text]:focus:hover { background:#fff; }
q:lang(sv) { quotes: "\201D" "\201D" "\2019" "\2019"; }
```

### 伪元素

选择器	| 含义
----|----
E:first-line	| 匹配E元素的第一行
E:first-letter	| 匹配E元素的第一个字母
E:before	| 在E元素之前插入生成的内容
E:after	| 在E元素之后插入生成的内容

举例：

```
p:first-line { font-weight:bold; color;#600; }
.preamble:first-letter { font-size:1.5em; font-weight:bold; }
.cbb:before { content:""; display:block; height:17px; width:18px; background:url(top.png) no-repeat 0 0; margin:0 0 0 -18px; }
a:link:after { content: " (" attr(href) ") "; }
```

## CSS 3

### 同级元素通用选择器

选择器	| 含义
----|----
E ~ F	| 匹配任何在E元素之后的同级F元素

举例：

```
p ~ ul { background:#ff0; }
```

### 属性选择器

选择器	| 含义
----|----
E[att^="val"]	| 属性att的值以"val"开头的元素
E[att$="val"]	| 属性att的值以"val"结尾的元素
E[att*="val"]	| 属性att的值包含"val"字符串的元素

举例：

```
div[id^="nav"] { background:#ff0; }
```

### 伪类

#### 与用户界面有关的伪类

选择器	| 含义
----|----
E:enabled	| 匹配表单中激活的元素
E:disabled	| 匹配表单中禁用的元素
E:checked	| 匹配表单中被选中的radio（单选框）或checkbox（复选框）元素
E::selection	| 匹配用户当前选中的元素

举例：

```
input[type="text"]:disabled { background:#ddd; }
```

#### 结构性伪类

选择器	| 含义
----|----
E:root	| 匹配文档的根元素，对于HTML文档，就是HTML元素
E:nth-child(n)	| 匹配其父元素的第n个子元素，第一个编号为1
E:nth-last-child(n)	| 匹配其父元素的倒数第n个子元素，第一个编号为1
E:nth-of-type(n)	| 与:nth-child()作用类似，但是仅匹配使用同种标签的元素
E:nth-last-of-type(n)	| 与:nth-last-child() 作用类似，但是仅匹配使用同种标签的元素
E:last-child	| 匹配父元素的最后一个子元素，等同于:nth-last-child(1)
E:first-of-type	| 匹配父元素下使用同种标签的第一个子元素，等同于:nth-of-type(1)
E:last-of-type	| 匹配父元素下使用同种标签的最后一个子元素，等同于:nth-last-of-type(1)
E:only-child	| 匹配父元素下仅有的一个子元素，等同于:first-child:last-child或 :nth-child(1):nth-last-child(1)
E:only-of-type	| 匹配父元素下使用同种标签的唯一一个子元素，等同于:first-of-type:last-of-type或 :nth-of-type(1):nth-last-of-type(1)
E:empty	| 匹配一个不包含任何子元素的元素，注意，文本节点也被看作子元素

举例：

```
p:nth-child(3) { color:#f00; }
p:nth-child(odd) { color:#f00; }
p:nth-child(even) { color:#f00; }
p:nth-child(3n+0) { color:#f00; }
p:nth-child(3n) { color:#f00; }
tr:nth-child(2n+11) { background:#ff0; }
tr:nth-last-child(2) { background:#ff0; }
p:last-child { background:#ff0; }
p:only-child { background:#ff0; }
p:empty { background:#ff0; }
```

#### 反选伪类

选择器	| 含义
----|----
E:not(s)	| 匹配不符合当前选择器的任何元素

举例：

```
:not(p) { border:1px solid #ccc; }
```

#### :target 伪类

选择器	| 含义
----|----
E:target	| 匹配文档中特定"id"点击后的效果


举例：

请参看HTML DOG上关于该选择器的 [详细解释](http://htmldog.com/articles/suckerfish/target/ "详细解释") 和 [实例](http://htmldog.com/articles/suckerfish/target/example/ "实例")。

## 拓展：xpath 路径表达式

简单说，**xpath 就是选择 XML 文件中节点的方法**。

所谓节点（node），就是 XML 文件的最小构成单位，一共分成 7 种。

```
- element（元素节点）
- attribute（属性节点）
- text （文本节点）
- namespace （名称空间节点）
- processing-instruction （处理命令节点）
- comment （注释节点）
- root （根节点）
```

xpath 可以用来选择这 7 种节点。下面只涉及最常用的第一种 element（元素节点），因此可以将下文中的节点和元素视为同义词。

### xpath 表达式的基本格式

xpath 通过 "路径表达式"（Path Expression）来选择节点。在形式上，"路径表达式" 与传统的文件系统非常类似。

```
# 斜杠（/）作为路径内部的分割符。

# 同一个节点有绝对路径和相对路径两种写法。

# 绝对路径（absolute path）必须用"/"起首，后面紧跟根节点，比如/step/step/...。

# 相对路径（relative path）则是除了绝对路径以外的其他写法，比如 step/step，也就是不使用"/"起首。

# "."表示当前节点。

# ".."表示当前节点的父节点
```

### 选择节点的基本规则

```
- nodename（节点名称）：表示选择该节点的所有子节点

- "/"：表示选择根节点

- "//"：表示选择任意位置的某个节点

- "@"： 表示选择某个属性
```

### 选择节点的实例

先看一个XML实例文档：

```
<?xml version="1.0" encoding="ISO-8859-1"?>
<bookstore>
  <book>
    <title lang="eng">Harry Potter</title>
    <price>29.99</price>
  </book>
  <book>
    <title lang="eng">Learning XML</title>
    <price>39.95</price>
  </book>
</bookstore>
```

```
bookstore ：选取 bookstore 元素的所有子节点。

/bookstore ：选取根节点bookstore，这是绝对路径写法。

bookstore/book ：选取所有属于 bookstore 的子元素的 book元素，这是相对路径写法。

//book ：选择所有 book 子元素，而不管它们在文档中的位置。

bookstore//book ：选择所有属于 bookstore 元素的后代的 book 元素，而不管它们位于 bookstore 之下的什么位置。

//@lang ：选取所有名为 lang 的属性。
```

### xpath 的谓语条件（Predicate）

所谓 "谓语条件"，就是对路径表达式的附加条件。

所有的条件，都写在方括号"[]"中，表示对节点进行进一步的筛选。

```
/bookstore/book[1] ：表示选择bookstore的第一个book子元素。

/bookstore/book[last()] ：表示选择bookstore的最后一个book子元素。

/bookstore/book[last()-1] ：表示选择bookstore的倒数第二个book子元素。

/bookstore/book[position()<3] ：表示选择bookstore的前两个book子元素。

//title[@lang] ：表示选择所有具有lang属性的title节点。

//title[@lang='eng'] ：表示选择所有lang属性的值等于"eng"的title节点。

/bookstore/book[price] ：表示选择bookstore的book子元素，且被选中的book元素必须带有price子元素。

/bookstore/book[price>35.00] ：表示选择bookstore的book子元素，且被选中的book元素的price子元素值必须大于35。

/bookstore/book[price>35.00]/title ：表示在例14结果集中，选择title子元素。

/bookstore/book/price[.>35.00] ：表示选择值大于35的"/bookstore/book"的price子元素。
```

### 通配符

```
# "*"表示匹配任何元素节点。

# "@*"表示匹配任何属性值。

# node()表示匹配任何类型的节点。
```

```
//* ：选择文档中的所有元素节点。

/*/* ：表示选择所有第二层的元素节点。

/bookstore/* ：表示选择bookstore的所有元素子节点。

//title[@*] ：表示选择所有带有属性的title元素。
```

### 选择多个路径

用"|"选择多个并列的路径。

```
//book/title | //book/price ：表示同时选择book元素的title子元素和price子元素。
```

---

## 说明

本文参考 [阮一峰博客](http://www.ruanyifeng.com/blog/ "阮一峰博客") 整理。
