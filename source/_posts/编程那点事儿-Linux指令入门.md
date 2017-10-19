---
title: 给小白看的 Linux 指令
date: 2017-08-05 19:36:21
categories: 编程那点事儿
tags:
  - CLI
  - Linux
---
<blockquote class="blockquote-center">UNIX-like 操作系统大行其道，可惜微软在 2014 年换 CEO 才意识到这个问题...</blockquote>

<!--more-->

## 写在前面

不同操作系统有不同的 CLI 指令。

由于 Mac 和 Linux 都是 [UNIX-like](https://zh.wikipedia.org/wiki/%E7%B1%BBUnix%E7%B3%BB%E7%BB%9F "UNIX-like") 操作系统，因此可以运行在 Linux 上的开源软件(特别是 Web 后端的软件，例如各种数据库、网站服务器、Ruby/Python/PHP 编程语言环境等) 也都支持 Mac，反而 Windows 支持比较差，CLI 指令也完全不同。这也是为什么 Web 开发者爱用 Mac 的原因，因为 Mac 接近 Linux 服务器环境。

可惜微软在 2014 年换 CEO 才意识到这个问题，在 Windows 10 上推出了 [Linux Subsystem 和 Bash on Ubuntu on Windows](https://msdn.microsoft.com/en-us/commandline/wsl/about "Linux Subsystem 和 Bash on Ubuntu on Windows") 来试图力挽狂澜...

以下介绍 Linux 相关的指令。Linux 的文件系统和 Mac 很像。

- 以阿里云为例（云计算基础服务 - 弹性计算 - 云服务器 ECS）
- 建好实例，登入主机：`ssh root@xx.xx.xx.xx`（IP）
- 登出：输入 `exit`

![](http://ogudt6aal.bkt.clouddn.com/image/20170806031728_ZVzLxo_2017-08-05-6.jpeg)

## Linux 文件操作

``` bash
pwd # 显示当前目录
cd # 切换工作目录
```

在 Linux 中有哪些目录呢？大概说明一下：

``` bash
/ # 根目录
/home # 用户目录，每个用户会有（Mac 上是 /Users）
/etc # 软件的配置文件，例如 mysql, nginx
/var # log 日志
/usr # 应用程序的安装目录（user program）
/tmp # 暂存目录，操作系统会定期清除，重开机也会清除
~ # 这个符号是用户根目录的意思，如果目前登入的用户是 root，那个 cd ~ 会前往 /root。如果目前是用 xxx 帐号登入，cd ~ 就会前往 /home/xxx/
```

### 管理文件

``` bash
ls -la # 显示所有文件，包含 . 开头的隐藏文件
mkdir # 新增目录
touch # 创建一个空文件
cp # 复制文件
mv # 移动文件或目录(也可以更名)
ln -s # 建立捷径别名
```

![](http://ogudt6aal.bkt.clouddn.com/image/20170806020719_flEDZy_2017-08-05-1.jpeg)

在截图中，先建立一个空目录是 `test`，然后创建一个文件 `README.txt`，接着复制和移动。最后用 `ln -s` 建立一个捷径别名(可以看到 abc 指向 todo.txt)。

``` bash
rm # 删除文件
rm -r # 删除整个目录和以下的文件
```

![](http://ogudt6aal.bkt.clouddn.com/image/20170806021223_nuhvBy_2017-08-05-2.jpeg)

截图中用 `rm` 指令删除了 `abc` 和 `README.txt`。`rm` 不能删除目录，而 `rmdir` 指令只能 **删除空目录**，要用 `rm -r` 指令才可以整个目录含以下的文件都删除掉。

### 编辑和浏览文件

CLI 中没有像 Atom 的 GUI 编辑器，操作系统内建的是 nano 或 vi，让我们练习一下：

输入 `nano abc.txt`：

![](http://ogudt6aal.bkt.clouddn.com/image/20170806021917_cCv8K0_2017-08-05-3.jpeg)

按 `control + X` 再按 `Y` 再按 `Enter` 就可以存盘离开。

我们会需要时常在服务器上直接编辑各种配置文件。

vi 和 vim 是另一套更为 geek 的文档编辑器，功能强大但比较难上手。

### 传输文件

SSH 通信协议除了可以让我们登入远端服务器，也可以用来传输文件，又叫做 SFTP。CLI 的指令是 `scp`。

![](http://ogudt6aal.bkt.clouddn.com/image/20170806023423_fELjsm_2017-08-05-4.jpeg)

截图中，第一个指令将本机的 `test.txt` 传到服务器上 root 帐号的 `~/` 目录。

第二个指令则是将服务器上的 `~/text.txt` 传回本机。

(PS: 可以用 FTP 软件如 [Cyberduck](https://cyberduck.io/ "Cyberduck")，通讯协议选 SFTP。)

![](http://ogudt6aal.bkt.clouddn.com/image/20170806025019_PVaPmI_2017-08-05-5.jpeg)

### 压缩和解压文件

如果要和服务器传输很多文件，先压缩打包成一个文件再传，速度会快很多。

压缩和打包是可以分开的：`gzip` 只能压缩一个文件，`tar` 可以打包整个目录顺便压缩。

``` bash
gzip # 压缩文件
gzip -d # 解压文件
tar zcvf xxx.tar.gz xxx # 将 xxx 目录打包并压缩成 xxx.tar.gz
tar zxcf xxx.tar.gz # 将 xxx.tar.gz 解压
```

## Linux 权限管理

### 增加新用户

登入服务器用的 `root` 帐号拥有最高权限，通常我们会另建一个帐号作为平常登陆管理之用。有了这个管理帐号后，就可以让 `root` 无法远端登入。（不然每台服务器默认都有 root 帐号，要破解密码只要破解 root 密码即可，很危险。）

执行 `adduser miaoxiaoge`，设定密码后，按 Enter...

![](http://ogudt6aal.bkt.clouddn.com/image/20170806032744_ScK8Bi_2017-08-05-7.jpeg)

换身份登入后，你会发现多了个目录：

![](http://ogudt6aal.bkt.clouddn.com/image/20170806033334_lPvEi8_2017-08-05-8.jpeg)

用现在的身份无法进入 `/root` 下，会出现 `Permission denied`，如果想进去需要切换身份。

### 切换身份

`su` 可以切换身份，例如切换成 `root` 帐号，输入密码即可。

切换后，如果再输入 `exit` 则会返回之前一层的身份。

`su - ` 的 `-` 的意思是可以把切换前的用户的环境变量带过去。

![](http://ogudt6aal.bkt.clouddn.com/image/20170806034344_jQJOH5_2017-08-05-9.jpeg)

很多时候我们只想执行一个需要 root 权限的指令，如果每次都要用 `su` 切换身份输入 root 密码太麻烦了。Linux 上有一种机制是 `sudo` 权限，可以暂时提升权限。

新建的帐号没有 `sudo` 权限，下面将它加到 `sudo` 清单。

新增 `/etc/sudoers.d/miaoxiaoge` 这个文件，内容是：

``` bash
miaoxiaoge  ALL=(ALL:ALL) ALL
```

（或执行 `gpasswd -a miaoxiaoge sudo` 也可以让 miaoxiaoge 新增 sudo 权限）

存盘离开。

编辑 `root` 的 `/root/abc.txt`，之前如果以 `miaoxiaoge` 身份直接编辑是没有权限的。现在在指令前加上 `sudo` 即可。(`sudo` 后要再次输入自己密码，不是 `root` 的密码)

### 文件和目录的权限说明

Linux 对于每个文件和目录，依据不同用户有着严格的权限控制：可否读、可否写、可否执行。

![](http://ogudt6aal.bkt.clouddn.com/image/20170806042544_pA7AxJ_2017-08-05-10.jpeg)

- 第一列：字段共 10 位，第一位表示是文件还是目录，`d` 是目录、`-` 是文件。剩下 9 位，每3位一组，第一组是文件拥有者的权限、第二组是群组(group)群限、第三组是其他用户。（ `r` 表示读、`w` 表示写入、`x` 表示可以执行，`-` 表示没有权限）
- 第二列的 `root` 表示这个文件属于 `root` 用户，也就是文件拥有者。
- 第三列的 `root` 表示这个文件属于 `root 群组(group)`。(默认每个用户会属于跟自己同名的群组)
- 截图中 abc.txt 是 `-rw-r--r--` 表示这是一个文件、文件拥有者是 `rw-` 可以读写、无法执行、群组和其他人是 `r--` 表示可以读不能写入。

### 修改文件权限

``` bash
 # 将文件 abc.txt 的拥有者设定为 miaoxiaoge，群组设定为 miaoxiaoge。如果要修改整个目录下文件，则可以用 chown -R
chown miaoxiaoge:miaoxiaoge abc.txt
```

``` bash
# 将文件 abc.txt 设定成 rw-r--r--
chmod 644 abc.txt
```

这需要了解一下什么是 Octal(八进制) Permissions 表示方式：
- `r` 是 4
- `w` 是 2
- `x` 是 1

你要的权限就是把数字加起来，例如 `rw-` 为 4+2 = 6，`r--` 是 4。于是 `rw-r--r--` 就是 644 。

## 关于免密码登陆

远端登入除了输入密码之外，SSH 还可以用非对称加密(Public Key Encryption)的方式来登入。

在非对称加密算法中，会有两把钥匙(key)，一把叫做公钥(public key)、一把叫做密钥(private key)。

- 透过公钥加密的密文，只有密钥能够解开。
- 透过密钥加密的密文，只有公钥能够解开。

于是我们就可以用这个机制来做登入：首先我们把自己的公钥先放在服务器上，之后登入时，服务器会送一个乱字符串给用户，用户用密钥加密后返回给服务器，如果服务器可以用公钥解回来，就表示认证成功。

设定步骤如下：

``` bash
# 登入后建立文件：
mkdir ~/.ssh
touch ~/.ssh/authorized_keys

# 回到本地终端把公钥(id_rsa.pub)输出来，执行：
cat ~/.ssh/id_rsa.pub
# 就会输出在画面上

# 回到远端服务器继续：
nano ~/.ssh/authorized_keys
# 把公钥复制粘贴进去后保存

# 修改权限
chmod 700 ~/.ssh
chmod 644 ~/.ssh/authorized_keys
```

这样就好了，你可以试试看再次登入就不用密码了。效果如下图：

![](http://ogudt6aal.bkt.clouddn.com/image/20170806203753_19GhBa_2017-08-05-11.jpeg)

### 本机的 id_rsa

因为 Github 也是用一样的认证机制，所以操作 git 时你应该已经产生过这个 SSH key，并且在 Github 后台贴上你自己的公钥，所以在 git push 时不需要输入密码。

![](http://ogudt6aal.bkt.clouddn.com/image/20170806210610_d7n2Pu_2017-08-05-12.jpeg)

如果本机真的没有 `~/.ssh/id_rsa.pub` 公钥的话，需要先产生你自己的公钥私钥，请执行 `ssh-keygen -t rsa` 就会产生公钥 `~/.ssh/id_rsa.pub` 以及密钥 `~/.ssh/id_rsa` 这两个文件。请一直按 Enter，不需要设定 `passphrse` 密码(不然每次用 key 都要输入一次密码)。

公钥可以公开给别人没关系，密钥就千万要保管好。

一台电脑可以生成多个 共钥-私钥 对。

### 加密原理

[关于非对称加密原理，戳这里。](http://51world.win/2017/07/02/%E7%BC%96%E7%A8%8B%E9%82%A3%E7%82%B9%E4%BA%8B%E5%84%BF-%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%9A%84%E5%8A%A0%E5%AF%86%E4%B8%8E%E9%80%9A%E4%BF%A1/ "关于非对称加密原理，戳这里。")
