---
{"dg-publish":true,"permalink":"/demo/obsidian/demo1/"}
---


# 前言
> 在从事与计算机相关的工作时，我们有很多触手可及的工具可以帮助我们更高效的解决问题。 但是我们中的大多数人实际上只利用了这些工具中的很少一部分，我们常常只是死记硬背一些如咒语般的命令， 或是当我们卡住的时候，盲目地从网上复制粘贴一些命令。
# Shell
## Shell是什么
Shell是文字接口，Shell有Bash、zsh、tcsh等等，但其核心功能都是一样的:`它允许你执行程序，输入并获取某种半结构化的输出` 。本文以及相关笔记默认使用的是Bash。
## 使用Shell
1. 提示符
```Bash
[xiaoran@cjdll ~]$
```
- `xiaoran` 当前用户名
- `cjdll` 当前主机名
- `$` 表示非`root`用户，`#` 表示`root`用户
2. date：打印当前日期和时间
```Bash
[xiaoran@cjdll ~]$ date
2022年 12月 05日 星期一 08:41:34 UTC
```
3. 环境变量`$PATH`;当Shell执行某条命令时，会在`$PATH`列出的路径进行查找相关命令
```Bash
# 查看$PATH
# 1.echo
[xiaoran@cjdll ~]$ echo $PATH
/usr/local/bin:/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/xiaoran/.local/bin:/home/xiaoran/bin
# 2. which查看某条命令所在的路径
[xiaoran@cjdll ~]$ which echo
/bin/echo
```
>我们执行 `echo` 命令时，shell 了解到需要执行 `echo` 这个程序，随后它便会在 `$PATH` 中搜索由 `:` 所分割的一系列目录，基于名字搜索该程序。当找到该程序时便执行（假定该文件是 _可执行程序_，后续课程将详细讲解）。
## 在Shell中导航
这一部分主要介绍了目录切换命令`cd`、查看目录下内容信息`ls`命令、文件基本属性以及其他的一些基础命令
1. `cd`命令
```Bash
#切换到上一级目录
$ cd ..
# 切换到指定目录
$ cd /path/to
# 切换到上一次所在的目录
$ cd -
```
2. `ls`命令
```Bash
#查看当前目录下所有文件并以列表形式显示
[xiaoran@cjdll ~]$ ls -al
总用量 16
drwx------. 2 xiaoran xiaoran  83 12月  5 09:02 .
drwxr-xr-x. 4 root    root     35 12月  5 06:48 ..
-rw-------. 1 xiaoran xiaoran  41 12月  5 10:52 .bash_history
-rw-r--r--. 1 xiaoran xiaoran  18 11月 24 2021 .bash_logout
-rw-r--r--. 1 xiaoran xiaoran 193 11月 24 2021 .bash_profile
-rw-r--r--. 1 xiaoran xiaoran 231 11月 24 2021 .bashrc
# 指定目录显示文件信息
[xiaoran@cjdll ~]$ ls -al /home
总用量 4
drwxr-xr-x.  4 root    root      35 12月  5 06:48 .
dr-xr-xr-x. 17 root    root     224 10月 30 2020 ..
drwx------.  2 xiaoran xiaoran   83 12月  5 09:02 xiaoran
```
3. 文件基本属性；这里涉及了Linux中用户、用户组的概念
```Bash
[xiaoran@cjdll ~]$ ls -al
...
drwxr-xr-x. 4 root    root     35 12月  5 06:48 ..
...
```
>本行第一个字符 `d` 表示 `..` 是一个目录。然后接下来的九个字符，每三个字符构成一组。 （`rwx`）. 它们分别代表了文件所有者（`root`），用户组（`root`） 以及其他所有人具有的权限。其中 `-` 表示该用户不具备相应的权限。

用户群组相关权限具体信息可以查看鸟哥的私房菜[# Linux 的檔案權限與目錄配置](https://linux.vbird.org/linux_basic/centos7/0210filepermission.php)
4. 其他的一些命令
>还有几个趁手的命令需要掌握的，例如 `mv`（用于重命名或移动文件）、 `cp`（拷贝文件）以及 `mkdir`（新建文件夹）
## 在程序中创建链接
这一小节，主要介绍了Shell中两个主要的"流"：输入流和输出流。涉及的命令主要是`>` 、`>>` 、`|`。
1. `>`命令可以将程序的输入输出流分别重定向到文件;每次重定向时会删除文件原有的内容
```Bash
[xiaoran@cjdll ~]$ echo "Hello xiaoran ." > hello.txt
[xiaoran@cjdll ~]$ cat hello.txt 
Hello xiaoran .
```
2. `>>` 向文件追加内容
```Bash
[xiaoran@cjdll ~]$ echo "Hi xiaoran." >>hello.txt 
[xiaoran@cjdll ~]$ cat hello.txt 
Hello xiaoran .
Hi xiaoran.
```
3. 管道符命令`|`，允许我们将一个程序的输出和另外一个程序的输入连接起来
```Bash
# 查看根目录下最后一行的文件
[xiaoran@cjdll ~]$ ls -l / |tail -n1
drwxr-xr-x.  19 root root  267 11月 24 14:47 var
```
## 功能全面又强大的工具
这一小节主要介绍了切换当前用户命令`su`和可以执行root权限命令`sudo`
1. `su` 切换用户
```Bash
[xiaoran@cjdll ~]$ su -l root # -l 切换用户后，环境变量会变为切换后的用户的环境变量
Password: 
Last login: Tue Dec  6 05:32:12 UTC 2022 on pts/6
```
2. `sudo`命令
当执行某些命令提示`permission denied`时，可以在该命令前加上`sudo`,当然也有例外的时候。比如
```Bash
# 课程中的例子，改变屏幕亮度
$ sudo find -L /sys/class/backlight -maxdepth 2 -name '*brightness*'
/sys/class/backlight/thinkpad_screen/brightness
$ cd /sys/class/backlight/thinkpad_screen
$ sudo echo 3 > brightness
An error occurred while redirecting file 'brightness'
open: Permission denied
```
出现上面错误的原因在于重定向命令`>`是由Shell执行时，并非被各个单独程序执行。意思是说当输出流重定向到brightness文件时Shell并没有root权限，可以用`$ echo 3 | sudo tee brightness`命令解决当前问题。

# 课后练习
...