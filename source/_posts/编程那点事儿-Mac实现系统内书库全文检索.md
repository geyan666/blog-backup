---
title: Mac实现系统内书库全文检索
date: 2017-09-03 2:36:21
categories: 编程那点事儿
tags:
  - Mac
  - G点
  - 配置
  - 全文检索
---
<blockquote class="blockquote-center"><img src="http://ogudt6aal.bkt.clouddn.com/image/20170903025822_OBQb8A_epub-1.jpeg" width="480">
MacOS 用 Spotlight Search 实现全文检索 `.epub` 书库。</blockquote>

<!--more-->

Mac OS 的好处之一就是内建 **系统级全文检索**，打开 Spotlight 即可使用。如此，我们就可以拥有一个属于自己的电子图书馆，用电脑取代那些笨重的书签和笔记。这对于一个读书爱好者来说是个福音。

可是，略微有点诡异的就是 Spotlight 竟然不支持 `.epub` 文件检索…… 颇令人崩溃。好在有解决方案 —— ePub-quicklook。

## **下载**

到下面这个网址：
<blockquote>https://github.com/jaketmp/ePub-quicklook</blockquote>
下载两个文件：
<blockquote>`epub.qlgenerator`
`epub.mdimporter`</blockquote>

![](http://ogudt6aal.bkt.clouddn.com/image/20170903032245_m5dP5H_epub-2.jpeg)
![](http://ogudt6aal.bkt.clouddn.com/image/20170903032245_Bms52s_epub-3.jpeg)
<img src="http://ogudt6aal.bkt.clouddn.com/image/20170903032245_U5maAy_epub-4.jpeg" width="350">

## **安装**

把这两个文件拷贝到以下目录内：
<blockquote>`/Library/Spotlight`</blockquote>

![](http://ogudt6aal.bkt.clouddn.com/image/20170903033011_DQ0umV_epub-5.jpeg)
![](http://ogudt6aal.bkt.clouddn.com/image/20170903033011_fyYqUk_epub-6.jpeg)

复制过程中可能让你输入用户密码。

## **配置**

而后打开 Terminal 输入以下命令：
<blockquote>`cd /Library/Spotlight/`
`qlmanage -r` # 让 Mac 自动运行此插件
`mdimport -r /Library/Spotlight/epub.mdimporter` # 为 .epub 文件建立索引
</blockquote>

![](http://ogudt6aal.bkt.clouddn.com/image/20170903033822_sWyB20_epub-7.jpeg)

此后的全文检索就会包括 `.epub` 文件了。（建立索引可能需要一段时间，比如十几分钟……）

---

有全文检索的话，读书的时候就不用那么费劲去记了，用脑子只需要记住一些关键字就好，而纸笔版的笔记就可以退休了……

## 拓展

另外一个讨厌的地方在于，Kindle 上买来的书，因为 DRM 而无法被全文检索…… 我们可以用 Calibre 加上一个 DeDRM 插件，把那些 .azw 文件转换成 .epub 就好了……

Calibre 和 DeDRM Plugin 的下载地址分别是：
<blockquote>http://calibre-ebook.com/download
https://github.com/apprenticeharper/DeDRM_tools
</blockquote>
<br>
