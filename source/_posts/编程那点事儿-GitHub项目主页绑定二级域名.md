---
title: GitHub Pages 项目主页绑定二级域名
date: 2017-09-12 10:36:21
categories: 编程那点事儿
tags:
  - git
  - GitHub
---
<blockquote class="blockquote-center">一个域名（同一个世界），多个网站（不同的梦想）。
</blockquote>

<!--more-->

## 写在前面

我猜看此文章的人，大多数都已经有了自己在 GitHub 上的独立博客了，而且这群人中的大多数人又绑定了自己的域名。那么，你可能会遇到下面这个问题：

- 除了 `个人主页` 绑定了自定义域名外，还想给自己账号下其他 `项目主页` 绑定二级域名。

本文就要解决这个问题。

## 教程

**场景重现**：
<blockquote>以我为例，我在 GitHub 上有了一个 **和用户名同名**（这点很重要） 的 GitHub Pages 当作个人主页（即个人博客）：
[taketimeasafriend.github.io](http://51world.win/ "时间的朋友博客")
但是我现在又有了几个想法:

1. 在 GitHub 上再建一个 GitHub Pages 当作我项目的 demo 页面展示，而不是在原先的 blog 下加子目录子页面(一来访问 url 不好看不专业，二来很多主题会强制对所有项目内的文件添加样式，影响展示效果)
2. demo 页面的访问链接是我自定义的一个域名
(如：是
  [testpages.51world.win](http://testpages.51world.win/)
而不是
  [51world.win/testpages](http://51world.win/testpages/)(会跳转)
或
  [taketimeasafriend.github.io/testpages](http://taketimeasafriend.github.io/testpages/)(会跳转)
)
</blockquote>

### 步骤一 - 新建项目主页

新建项目仓库，这里不赘述。直接到 `Settings` ➡️ `GitHub Pages`：

![](http://ogudt6aal.bkt.clouddn.com/image/20170912143150_wmrrG7_erjiyuming-1.jpeg)

我们就选个 `Time Machine` 主题（让我想起了《当幸福来敲门》电影台词... ）吧，点击 `Commit changes`，仓库就有文件了，供我们下一步使用。

![](http://ogudt6aal.bkt.clouddn.com/image/20170912143520_B6lEN7_erjiyuming-2.jpeg)

### 步骤二 - 填写自定义域名

再回到 `Settings` ➡️ `GitHub Pages`，填写自定义二级域名，比如：`testpages.51world.win`，保存：

![](http://ogudt6aal.bkt.clouddn.com/image/20170912144108_yTc2nn_erjiyuming-3.jpeg)

保存后我们回到 `code` 下会发现，多了个 `CNAME` 文件：

![](http://ogudt6aal.bkt.clouddn.com/image/20170912144413_A68aNC_erjiyuming-4.jpeg)

### 步骤三 - 解析域名

这里我用的是 DNSPOD（其他服务商应该类似，没有亲测，有疑问可以留言互动），添加记录，保存：

``` bash
主机记录：* # 泛解析，匹配其他所有域名： *.你的顶级域名
记录类型：CNAME # 如果需要将域名指向另一个域名，再由另一个域名提供ip地址，就需要添加CNAME记录。这里我们把自定义域名指向 GitHub 默认域名
线路类型：默认
记录值：taketimeasafriend.github.io. # CNAME记录。这里填写空间商（GitHub）提供的域名
# 其他随意
```
![](http://ogudt6aal.bkt.clouddn.com/image/20170912145426_QrLGg7_erjiyuming-5.jpeg)

### 步骤四 - 验证

浏览器输入刚才自定义的二级域名，确认下是否成功：

![](http://ogudt6aal.bkt.clouddn.com/image/20170912145807_f7ps3k_erjiyuming-6.jpeg)

最后你可以将默认 index 文件删去，给仓库添加自己需要的 demo 文件。

大功告成。


## 拓展

看看类似操作后大师的开源项目，一本看上去很舒服的开源图书：

- 源码：https://github.com/ruanyf/es6tutorial
- 书的域名：http://es6.ruanyifeng.com/

---

参考链接：

1. https://help.github.com/articles/about-supported-custom-domains/
2. https://help.github.com/articles/user-organization-and-project-pages/
3. https://help.github.com/articles/adding-or-removing-a-custom-domain-for-your-github-pages-site/
4. https://help.github.com/articles/using-a-custom-domain-with-github-pages/
5. http://chitanda.me/2015/11/03/multiple-git-pages-in-one-github-account/
