---
title: 无处不在的 Emoji 表情由来
date: 2016-12-20 2:36:21
categories: 编程那点事儿
tags:
  - Emoji
  - 计算机原理
---
<blockquote class="blockquote-center">浅入浅出 **Emoji** 😭-😄-🌹-😠-😢-💀。
</blockquote>

<!--more-->

## 含义

Emoji 就是可以插入文字内的图形符号（也就是可以把它看作文字），如下：

<img src="http://ogudt6aal.bkt.clouddn.com/image/emoji.jpg" width="450" alt="Emoji">  

它是一个日语词，e 表示"絵"，moji 表示"文字"。连在一起读音 emoji，就是日语词汇"絵文字"。2014年8月，牛津词典在线版（Oxford Dictionary Online）把“Emoji”添加到新词汇中，这意味着它已经变成一个正式词汇。

Emoji 在上个世纪90年代，由日本电信商引入服务，最早用于在短消息之中插入表情。2007年，苹果公司的 iPhone 支持了 Emoji，导致它在全世界范围的流行。

<img src="http://ogudt6aal.bkt.clouddn.com/image/%E6%89%8B%E6%9C%BA%E4%B8%8A%E7%9A%84emoji.jpeg" width="350" alt="手机上的Emoji">  

## 原理

### Unicode 标准化

早期的 Emoji 是将一些特定的符号组合替换成图片，比如将 **:)** 替换成 😀。这种方法很难标准化，能够表达的范围也有限。

2010年，Unicode 开始为 Emoji 分配码点。也就是说，现在的 Emoji 符号就是一个 **文字**，它会被 **渲染为图形**。

<img src="http://ogudt6aal.bkt.clouddn.com/image/emoji%E7%9A%84unicode%E7%A0%81%E8%A1%A8%E5%AE%9E%E4%BE%8B.png" alt="emoji的unicode码表实例">  

由于越来越受欢迎，[Emoji 的国际标准](http://www.unicode.org/emoji/charts/full-emoji-list.html "Emoji 的国际标准") 在 2015 年出台，目前已经是 5.0 版了。

- Emoji 1.0：2015年8月
- Emoji 2.0：2015年11月
- Emoji 3.0：2016年6月
- Emoji 4.0：2016年11月
- Emoji 5.0 (beta)：2017年3月

### 渲染实现

Unicode 只是规定了 Emoji 的码点和含义，并没有规定它的样式。举例来说，码点 U+1F600 表示一张微笑的脸，但是这张脸长什么样，则由 **各个系统自己实现**。

<img src="http://ogudt6aal.bkt.clouddn.com/image/emoji%E5%90%84%E4%B8%AA%E7%B3%BB%E7%BB%9F%E8%87%AA%E5%B7%B1%E5%AE%9E%E7%8E%B0.png" alt="emoji各个系统自己实现">  

因此，当我们输入这个 Emoji 的时候，并不能保证所有用户看到的都是一模一样的脸。如果用户的系统没有实现这个 Emoji 符号，用户就会看到一个没有内容的方框，因为系统无法渲染这个码点。

目前，苹果系统、安卓系统、Twitter、Github、Facebook 都有自己的 Emoji 实现：

- 苹果 http://emojipedia.org/apple/
- 安卓 http://emojipedia.org/google/
- Twitter https://twitter.github.io/twemoji/preview.html
- Github https://gist.github.com/rxaviers/7360908
- Facebook http://emojipedia.org/facebook/

## 使用

Emoji 虽然可以看作是文字，但是无法书写，必须使用其他方法插入到文档。

- （1）最简单的方法当然是复制/粘贴，你可以到 http://getemoji.com 选中一个 Emoji 贴在自己的文档即可。
- （2）另一种方法是通过码点输入 Emoji。以 HTML 网页为例，将码点 U+1F600 写成 HTML 实体的形式 **&#1 2 8 5 1 2;**（十进制(去掉空格)）或 **&#x1 F 6 0 0;**（十六进制(去掉空格)），就可以插入网页。码点到这个[页面](http://emojipedia.org/facebook/http://emojipedia.org/facebook/ "页面")查询。
- （3）JavaScript 输入 Emoji，可以使用 [node-emoji 这个库](https://www.npmjs.com/package/node-emoji "node-emoji 这个库")：

```
var emoji = require('node-emoji');

// 返回 coffee 的 Emoji
emoji.get('coffee');

// 返回文字标签对应的 Emoji
// 对应关系到这个页面查询： https://www.webpagefx.com/tools/emoji-cheat-sheet/
emoji.get(':fast_forward:');

// 将文字替换成 Emoji
emoji.emojify('I :heart: :coffee:!');

// 随机返回一个 Emoji
emoji.random();

// 查询 Emoji
// 返回结果是一个数组
emoji.search('cof');
```

- （4）还可以通过 CSS 插入 Emoji：

```
<link href="https://afeld.github.io/emoji-css/emoji.css" rel="stylesheet">
<i class="em em-baby"></i>
```

## 特殊的 Emoji 组合

Unicode 除了使用单个码点表示 Emoji，还允许多个码点组合表示一个 Emoji。

其中的一种方式是"零宽度连接符"（ZERO WIDTH JOINER，缩写 ZWJ）U+200D。举例来说，下面是三个 Emoji 的码点：

- U+1F468：男人
- U+1F469：女人
- U+1F467：女孩

上面三个码点使用 U+200D 连接起来，**U+1F468 U+200D U+1F469 U+200D U+1F467**，就会显示为一个 Emoji 👨‍👩‍👧，表示他们组成的家庭。如果用户的系统不支持这种方法，就还是显示为三个独立的 Emoji 👨👩👧。

## emoji 相关趣闻

- 根据 [emojitracker](http://emojitracker.com/ "emojitracker") (网页动态显示当前世界 twitter 上 emoji 统计数据)的调查，全世界最流行的 emoji，第一名是笑出眼泪 😂，第二名是红心❤️。

<img src="http://ogudt6aal.bkt.clouddn.com/emojitracker.png" alt="emojitracker">  

- 日历的 Emoji 📅（U+1F4C5） 在苹果系统之中，一律是7月17日。这是苹果公司发布 iCal 的日子。[有人戏称这个日子是"世界 Emoji 日"](http://baike.baidu.com/link?url=40LKef0o8sUHJPPaSnkwsS-aIabdSBp3L5KM3JkxHBE7KkAy0D-97PUwEDu4Pn-iWhjP3N8fDKKTnfu23aq3MwBlbiq8Ng2OcoRp0cBevEG87neztjW2SEuhS-1jy6aWgMNuSWVuqYusnFF9bp0EfK "有人戏称这个日子是"世界 Emoji 日"")。

<img src="http://ogudt6aal.bkt.clouddn.com/image/%E4%B8%96%E7%95%8C%E8%A1%A8%E6%83%85%E5%8C%85%E6%97%A5.jpg" alt="世界表情包日">  

---

## 说明
本文参考 [阮一峰博客](http://www.ruanyifeng.com/blog/ "阮一峰博客") 整理。
