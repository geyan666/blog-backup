---
title: SEO 搜索引擎优化
date: 2017-08-04 4:36:21
categories: 编程那点事儿
tags:
  - SEO
  - 前端
---
<blockquote class="blockquote-center">任何一个网站想在推广中成功，搜索引擎优化都是至为关键的一个环节。
</blockquote>

<!--more-->

## 概述

**SEO** （Search Enginer Optimization - 搜索引擎优化），是一种通过了解搜索引擎的运作规则来调整网站、提高目标网站在有关搜索引擎内排名的方式。对于任何一个网站来说，要想在网站推广中获取成功，搜索引擎优化都是至为关键的一个环节。

针对搜索引擎 **最优化处理**，是指为了要让网站更容易被搜索引擎接受。搜索引擎会将网站彼此间的内容做一些相关性的数据比对，然后再由浏览器将这些内容以最快速且接近最完整的方式呈现给搜索者。

## 搜索引擎原理

所谓的搜寻引擎是这样运作的：

- 搜索引擎派出 bot （机器人）到处去爬（下载）网页
- 搜索引擎透过内部算法，将这些网页中的文字做 **有效的关键字索引**

一般来说，人类看到的网页是这样的（已透过浏览器翻译渲染）：

![](http://ogudt6aal.bkt.clouddn.com/image/20170804042533_tmZqAi_mysite.jpeg)

在搜寻引擎的 bot 眼中，网页是长这个样子：

![](http://ogudt6aal.bkt.clouddn.com/image/20170804042804_5Fl9sy_mysitecode.jpeg)

对！就是一堆代码而已。

所以搜索引擎需要透过代码，知道这个网页上的「重点」究竟是什么。

## HTML

很多人初学 HTML ，往往以为这只是构建网页的标记语言而已。事实上，HTML 当初被设计的时候，是带有「语义」的。

很好理解：
<blockquote>
H1 是大标（ Heading）
H2 , H3 是中标（ Heading）
H4, H5, H6 是小标
`<P>` 是指 paragraph (段落)
`<strong>` 是重要粗体
`<img>` 中的 `alt` 是指替代文字，`title` 是标题
...
</blockquote>

所以，如果开发者希望产品中的关键字被搜索引擎检索到，就应该尽量使用「正确的 HTML 标签」。比如说商品页面就该这样：

``` bash
<h1> 美国原装进口人体工学舒适 Aeron </h1>

<p> <h2>工作椅的全新演绎</h2> 独特的外观、先进的<strong>人体工学</strong>理念、94%的材料可降解回收，Aeron座椅彻底改变了人们关于办公椅的思维定式。 针对人们每天的各种坐姿随时进行调节，能够使人们获得健康的舒适感和平衡的身体支撑。创新的悬浮式设计及便捷的调节控制，无需费力，Aeron座椅即可随着全身的活动进行调整。</p>

<img src="aeron.jpg" title="Aeron 人体工学座椅">
```

SEO 技术，本质就是在 **「帮网页画重点」**，最大化的让搜寻引擎了解你的网站内容。

### HTML 使用注意事项

- div 与 span 无意义，只是容器
- 不要滥用 H1, H2, H3 的数量  
 - H1 一页只能有一个
 - H2 一页只能有 2-3 个（太多的话搜索引擎会认为你在作弊）
 - H3 一页可以有 6-10 个

### meta

HTML 里的 meta，也有助于搜索引擎了解网站里面的内容。

- title - “建立独特、准确的网页标题”
- meta description - “叙述本页内容”

### 权重

下面是关键字在网页当中的权重优先顺序：

``` bash
title > mete > h1 > h2 > h3 > strong > p
```

所以有的网站是这样干的：

![](http://ogudt6aal.bkt.clouddn.com/image/20170804044149_JjuiPd_seo.jpeg)

### 秘诀

- 多把关键字放在标题与叙述上
- 每一网页的标题与叙述都要「不同」（否则反而会被搜索引擎认为作弊）

## 网站常见的缺点

### 一模一样的叙述

常见到网站最大的缺点是：

- titile 都一样
- description 都一样

如果你不懂 SEO 技术会觉得这是一个不起眼的细节，上线部署完成后有空再改。但事实上，许多人的网站不被收录，正是因为这个小细节。

如果整个网站都是一样的 title 与 description，搜索引擎「分不出来谁是谁」，自然「全部都不收录」。

### 滥用 H

另外一个就是 H 家族。

有些人认为既然 H 容易被收录，那么我们就加多一点 h1, h2 吧。殊不知 h1, h2 是有「限额」的。如果一个网页里面的 h1,h2 出现太多。搜索引擎就会认为站长试图作弊，直接不收录这个网站。

一般 h1,h2, h3 原则是这样的：

- h1 一页只能有一个
- h2 一页只能有 2-3 个
- h3 一页只能有 4-7 个

### SOP(standard operating procedure)

网站上线前，最好做以下检查：

- 检查网页「语意结构」是否合理（比如说 h 家族要正确，图片要加叙述）
- 每一页的标题与叙述要「不一样」
- 产生 `sitemap`
- `robots.txt` 里面要开放检索
- 使用 Google webmaster 彻底扫描一下，自己网站上的连结是否有不利因素

## 拓展

### ruby 上用到的 seo gem

- https://github.com/techbang/seo_helper

#### 效果

- 变换每篇文章的「标题」
- 变换每篇文章的「叙述」

#### 步骤

- **Step 1: 修改 Gemfile**

``` bash
gem "seo_helper"
```

- **Step 2: 新增 seo_helper 配置文件**

``` bash
touch config/initializers/seo_helper.rb
```

内容如下：

``` bash
SeoHelper.configure do |config|
  config.site_name = "My Awesome Web Application"
end
```

`site_name` 就是你想要出现的站名，如果是 `小小食杂铺` 就是 `config.site_name = "小小食杂铺"`

重开：

``` bash
rails s
```

- **Step 3: 将 render_page_title_tag 放在 application.html.erb 的 head 中**

将 `<%= render_page_title_tag %>` 放在 `<head>` 与 `</head>` 中。

- **Step 4: 修改 controller 中的 action**

``` bash
def show
  # ...
  set_page_title @product.title
  set_page_description “#{@product.description}”
end
```

每一页的 title 和叙述就会变得不一样：

![](http://ogudt6aal.bkt.clouddn.com/image/20170804045641_ISopdL_seoset.jpeg)

如果叙述过长，可以这样做

``` bash
 def show
  page_description = view_context.truncate(@product.description, :length => 100)
end
```

### sitemap

Google 的算法是基于 PageRank，运作原理是派「爬虫」来抓取网页上的链接。也就是网站上「相通」的链接，基本上都有办法抓完。

但是还是会冒出一些问题：

- 有些链接是孤岛，这一辈子从来不可能被「被动收录」进去。
- 有些网站才刚建立，要等 Google 逛到它，然后被收录进去。
- 有些大网站，链接太多了。如果 Google 每一个网页都「重新收录」，光爬虫流量，就会打垮这个网站。

这些问题要怎么解决呢？

**Sitemap** 指的不是一般人理解的「网站地图」。它是一种通用格式(xml），用来产生网站的「链接结构」。一般网站，可以透过提交自己的 sitemap 给搜索引擎。让搜索引擎知道：

- 网站内有哪些链接；
- 哪些连结是「相对重要的」；
- 哪些连结是需要每天更新「快照」，哪些连结则一个月更新一次就好了。

Google 提供了一套 SEO 工具，可以更强化你的 SEO 效果，并且提供网站健康检查报告，叫做 webmaster （不管你的网站是在国内或者是国外，建议使用）：

https://www.google.com/webmasters/

你可以在这个网站上登记你的网站，并且使用这个网站进行健康检查，并且提交 sitemap。

---

（Rails 里面有一个 `gem sitemap_generator` 就是在做这些事。这里提供 Logdown 这个网站（一个博客平台）的 sitemap 配置文件，大家可以试着理解下：）

config/sitemap.rb：

``` bash
# -*- encoding : utf-8 -*-

# Set the host name for URL creation

SitemapGenerator::Sitemap.default_host = "http://logdown.com"
SitemapGenerator::Sitemap.create_index = true

# Generate Sitemaps on read only filesystems like Heroku

# Reference:

# https://github.com/kjvarga/sitemap_generator/wiki/Generate-Sitemaps-on-read-only-filesystems-like-Heroku#using-carrierwave

# pick a place safe to write the files

SitemapGenerator::Sitemap.public_path = 'tmp/'
# store on S3 using CarrierWave

SitemapGenerator::Sitemap.adapter = SitemapGenerator::WaveAdapter.new
# inform the map cross-linking where to find the other maps

SitemapGenerator::Sitemap.sitemaps_host = "http://#{Setting.aws.bucket}.s3.amazonaws.com/"
# pick a namespace within your bucket to organize your maps

SitemapGenerator::Sitemap.sitemaps_path = 'sitemap/'

SitemapGenerator::Sitemap.create do

  add "/pages/about"
  add "/pages/pricing"
  add "/pages/privacy"
  add "/tours"

  # Put links creation logic here.

  #

  # The root path '/' and sitemap index file are added automatically for you.

  # Links are added to the Sitemap in the order they are specified.

  #

  # Usage: add(path, options={})

  #        (default options are used if you don't specify)

  #

  # Defaults: :priority => 0.5, :changefreq => 'weekly',

  #           :lastmod => Time.now, :host => default_host

  #

  # Examples:

  #

  # Add '/articles'

  #

  #   add articles_path, :priority => 0.7, :changefreq => 'daily'

  #

  # Add all articles:

  #

  #   Article.find_each do |article|

  #     add article_path(article), :lastmod => article.updated_at

  #   end

  Blog.where("fqdn is NULL").find_in_batches do |blogs|
    blogs.each do |blog|
      add "/", :host => "#{blog.domain}", :priority => 0.7, :changefreq => "daily", :lastmod => blog.updated_at

      blog.posts.published.find_in_batches do |posts|
        posts.each do |post|
          add post.relative_url, :host => "#{blog.domain}", :lastmod => blog.updated_at
        end
      end

    end
  end
end


Blog.find_in_batches do |blogs|
  blogs.each do |blog|
    SitemapGenerator::Sitemap.default_host = "#{blog.domain}"
    SitemapGenerator::Sitemap.filename = "#{blog.subdomain}"
    SitemapGenerator::Sitemap.public_path = 'tmp/'
    SitemapGenerator::Sitemap.adapter = SitemapGenerator::WaveAdapter.new
    SitemapGenerator::Sitemap.sitemaps_host = "http://#{Setting.aws.bucket}.s3.amazonaws.com/"
    SitemapGenerator::Sitemap.sitemaps_path = 'sitemap/'
    SitemapGenerator::Sitemap.create do
      blog.posts.published.find_in_batches do |posts|
        posts.each do |post|
          add post.relative_url, :host => "#{blog.domain}", :lastmod => blog.updated_at
        end
      end
    end
  end
end

```
