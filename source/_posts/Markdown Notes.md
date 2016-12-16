---
title: Markdown Notes
tags:
  - Markdown
  - Notes
---
This is a smart aticle about how to write in Markdown for this blog,just like CheatSheet for Mac.

# Primer

1. **标题:** \#的数目后空格(1~6)
1. **斜体 / 粗体:** \*夹 / \*\*夹 
1. **划分段落:** 宽-空白行; 窄-2空格后回车(块注释里同样适用)
1. **块注释:** >(5空格后会框起来)(可嵌套)
1. **有序列表 / 无序列表:** number.空格(可嵌套) / -空格
1. **图片:** !\[替换文本](链接 "tips文本") (用<img\>标签可指定图片大小)
1. **网址:** \[要加链接的内容](链接 "tips文本")
1. **邮箱 / 网址本身链接:** <内容> 
1. **代码:** 小段-`夹; 大段-4空格
1. **水平线:** 3个-
1. **转义:** 用\来转义表示文本中的Markdown符号 

# Advance

1. **删除:** ~~夹
1. **表格:** 详见下面[Advance Test Below]
1. **流程图:** 待更新
1. **时序图:** 待更新
1. **LaTeX公式:** 待更新
1. **脚注:** 待更新
1. **引用目录:** 待更新
1. **语言代码块:** 待更新
1. **ToDo列表:** 待更新

# Attention:

1. **文首输入tags添加标签，categories添加分类(有层次)**
1. **可兼容html标签**
1. 不同Markdown环境可能有细微差异，记得预览后微调(如:注意前后空行)
1. 有些高级功能属于衍生功能, 可能不支持
---

# [Primer Test Below]

# 标题
*斜体*  
<font color=#DC143C size=5 face="黑体">猩红色5号黑体字</font>  
**粗体**

上面划分了一个宽段落  
这行上面分了一个窄段落
>块注释
>>而且可嵌套
>这样不能换行
>
>这个才能

1. 有序列表条目1
1. 有序列表条目2
 1. 嵌套的有序列表条目1(前面1空格)
 1. 嵌套的有序列表条目2
1. 有序列表条目3

这是分隔列表的文字

- 无序列表
- 无序列表
 - 嵌套的无序列表(前面1空格)
 - 嵌套的无序列表
- 无序列表

图片:  
![加载失败时替换文本](http://obqtodoqr.bkt.clouddn.com/wp-content/uploads/2016/08/renleishi.png "这是测试用的tip文本,比如“你会说一句傻X”,而且是图片下方的标题")  
这里用一下全能的<img\>标签(可实现对大小等不同操作),把上面的图缩小了一半:  
<img src="http://obqtodoqr.bkt.clouddn.com/wp-content/uploads/2016/08/renleishi.png" width="300" height="167" alt="加载失败时替换文本">  
网址:  
[要加链接的内容,比如时间的朋友](http://51world.pub "tips文本,这里是时间的朋友")  
邮箱 / 网址本身就是链接的例子:  
我的邮箱是<331718489@qq.com>  
时间的朋友的网址是<http://51world.pub>  
代码: `我是小不点代码`

    我是一大段代码,不管多长照着这样写会自动调整换行
    主动换行要像这样
水平线如下:

---
转义需要用 \ ,比如\#我不是标题  \*我不是斜体\*  \*\*我不是粗体\*\*

# [Advance Test Below]

~~我被删除了~~  
表格:

dog  | bird | cat
-----|:----:|----:
默认 | 这个  | 这个
左   | 居中  | 右
对齐 |  了   | 对齐
其实html表格也很方便，有专门的在线生成器，比如<http://www.tablesgenerator.com/html_tables>(需要注意平台是否支持)