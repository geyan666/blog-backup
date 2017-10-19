---
title: Hexo NexT 主题添加动画插件
date: 2017-09-04 12:42:21
categories: 编程那点事儿
tags:
  - Hexo
  - 前端
  - NexT
---
<blockquote class="blockquote-center">嗨！看我博客左上角的小人儿～(手机端就算了)
</blockquote>

<!--more-->

## **写在前面**

![](http://ogudt6aal.bkt.clouddn.com/image/20170904200756_QnvKPq_tianjiadonghua.gif)

之前刚架自己独立博客时[偶然间看到了这个小动画](https://kangseven7.github.io/ "偶然间看到了这个小动画")，觉得非常有趣。奈何那时自己属于技术新手，只能眼巴巴地看着别人“高端大气上档次,炫酷狂拽叼炸天”，自己在墙角暗暗“觊觎”。

今天无聊翻看收藏夹时又看到了它，自我感觉应该有此程度的装X能力了，遂装之。

现留配置笔记，以作纪念。

## **添加动画插件**

### **追根溯源**

各种跟踪，代码作者貌似是[这个 G 友](https://github.com/idhyt/hexo-theme-next/commits/master "这个 G 友")，证据如下：

![](http://ogudt6aal.bkt.clouddn.com/image/20170904170616_z3UQG6_tianjiadonghua-1.jpeg)

向他致敬。

### **添加步骤**

**1.**
<blockquote>在主题配置文件 `_config.yml` 中预先添加如下代码：
``` bash
# Some suprise plugs
suprise:
  # 踢皮球动画开关
  ball_enable: true
```
</blockquote>

**2.**
<blockquote>在 NexT 主题目录中，打开文件 `next/layout/_layout.swig` 预先添加如下代码：
``` bash
# 判断上面 ball_enable 开关，导入 _partials/suprise/ball.swig (此文件我们下一步添加)
{% if theme.suprise.ball_enable %}
    {% include ’_partials/suprise/ball.swig’ %}
{% endif %}
```
![](http://ogudt6aal.bkt.clouddn.com/image/20170904172031_SPjVKx_tianjiadonghua-2.jpeg)
</blockquote>

**3.**
<blockquote>上一步说要导入 `ball.swig` ，现在就创建一个文件 `next/layout/_partials/suprise/ball.swig` ，这里面是显示的字 `why you are here?` ，代码如下：
``` html
<div id="idhyt-surprise-ball">
  <div id="idhyt-surprise-ball-animation">
    <!--why you are here? -->
    <span id="layer0Go" class="drag">w</span>
    <span id="layer1Go" class="drag">h</span>
    <span id="layer2Go" class="drag">y</span>
    <span id="layer3Go" class="drag"></span>
    <span id="layer4Go" class="drag">y</span>
    <span id="layer5Go" class="drag">o</span>
    <span id="layer6Go" class="drag">u</span>
    <span id="layer7Go" class="drag">a</span>
    <span id="layer8Go" class="drag">r</span>
    <span id="layer9Go" class="drag">e</span>
    <span id="layer10Go" class="drag">h</span>
    <span id="layer11Go" class="drag">e</span>
    <span id="layer12Go" class="drag">r</span>
    <span id="layer13Go" class="drag">e</span>
    <span id="layer14Go" class="drag">?</span>
    <span id="layer15Go" class="drag ball"></span>
  </div>
</div>
```
</blockquote>

**4.**
<blockquote>导入动画插件。在文件 `next/source/css/_custom/custom.styl` 最开始处添加这一句代码：
``` bash
// Custom styles.
@import "_suprise/ball"; # 下一步会添加这个配置文件
```
</blockquote>

**5.**
<blockquote>建立一个动画配置文件，位置：`next/source/css/_custom/_suprise/ball.styl` 。[文件内容地址在这里](https://github.com/idhyt/hexo-theme-next/blob/0fbc84eeb3da275cf53e8c3f64109b4881ae771a/source/css/_custom/_suprise/ball.styl "文件内容地址在这里")。
</blockquote>

到此，就完成了动画的添加。

**6.**
<blockquote>如果显示的位置有些错位，可以自己试着调整 `ball.styl` 代码处：
<img src="http://ogudt6aal.bkt.clouddn.com/image/20170904173914_bdxkRx_tianjiadonghua-3.jpeg" width="400">
</blockquote>

祝你玩得开心。
