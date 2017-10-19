---
title: git 及 GitHub 使用
date: 2016-09-09 10:36:21
categories: 编程那点事儿
tags:
  - git
  - GitHub
---
<blockquote class="blockquote-center">身为程序员，对一些命令必须形成条件反射进而进行肌肉记忆。
</blockquote>

<!--more-->

## git 初始化

``` bash
# 1. 进入文件夹内：
 $ git config --global user.name "xxx"
 $ git config --global user.email xxx@gmail.com
# 2. git 初始化
 $ git init(或新建好工程后，再初始化也行)
# 3. 文件夹添加文件后：
 $ git add .
 $ git commit -m "add a file to test"
```

## GitHub仓库和上面的工程及联操作

``` bash
# 1. 新建仓库
# 2. 复制如下命令在命令行输入：
 $ git remote add origin git@github.com:taketimeasafriend/gallery.git
 $ git push -u origin master
```

PS:  
如果第2步出现：
``` bash
fatal: remote origin already exists.
```
删除远程Git仓库：
``` bash
$ git remote rm origin
```
再从第 2 步开始继续操作。

## 本地继续操作

``` bash
 $ git push origin master (将本地推上GitHub仓库)
 $ git pull origin master（合作的时候养成先pull再操作的好习惯）
```

## 处理已知的工程

``` bash
# 1. clone到本地（如果是别人的，可以fork到自己账户中再操作）
 $ git clone git@github.com:xxx/yyy.git
# 2. 切换分支
 $ git checkout -b version-x
# 3. 编辑后直接 add-commit-push
```

### 和另一个账号合作（对方同意你有权操作）

- 别人的工程: pull request

- 我另一个 GitHub 账号的 Hexo 博客:  
 - 1-本地 $ git clone 仓库
 - 2- $ hexo init / install hexo-deployer-git --save / g / s
 - 3-_config 配置 repo: https://github.com/另一个GitHub...
 - 4- $ hexo d
 - 5-CNAME 文件等
