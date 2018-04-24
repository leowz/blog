---
title: Mac terminal commands
date: 2016-05-01 21:39:59
tags:
---

# 基本操作
- ls(list): 显示目录内容

- cd: 到用户根目录；cd /uer :到相应目录； cd .. cd../.. ：返回上级

- pwd(present working directory)：本命令用于显示当前的工作目录

- mkdir／rmdir：本命令用于建立／删除目录

- cp(copy): 拷贝

- mv(move): mv [-f] [-i] 文件1 [文件2...] 目标;将文件移动至目标，若目标是文件名，则相当于文件改名

- rm(remove):用来删除文件或目录

- diff(differentiate) 本命令比较两个文本文件，将不同的行列出来

- echo: 回音，输出
 
- cat(catenate)：显示编辑文件，或创建文件；显示和连接一个或多个文件至标准输出

- sort: 以字母顺序、数字大小排序。

- cut: 从标准输入中切出字段。

- sed(substitution edite): 用正则表达式从标准输入替换内容。

- tr(translate characters): 从给定字符或字符集给标准输入替换指定字段。 

- ifconfig／ping : 显示网络信息。

- bc(basic calculator): 小计算器

- find: 在当前目录树下迭代的寻找文件，文件名用正则表达式表示

- env(environment variable): 显示环境变量

# 文件权限设置
用`ls -la`命令后会显示文件夹内文件具体信息，会得到如下显示：
`-rw-r--r-- 1 root root 302108 11-13 06:03 log2012.log`
drwxrwxrwx表示:第一个字符表示是否是文件夹(d/-),后9个每三个一组，第一组是用户(u)的读(r/-)、写(w/-)、执行(x/-)权限,第二组是组用户(g)的权限，第三组是其他用户(o)的权限。
r：读权限，用数字4表示
w：写权限，用数字2表示
x：执行权限，用数字1表示
-：删除权限，用数字0表示

- chmod(change mode):

1. 数字表示法: chmod 444 filename; 对file设置权限，file为u,g,o用户可读。
2. 文字设定法: chmod ug+w,o-x filename; 对u,g用户增加写权限，对o用户减少执行权限。

# 标准输出重定向
默认情况下，标准输出、标准错误定向到屏幕，标准输出链接到键盘。

- >/>>: ls -la >> ls-out.tex; 输出重定向到指定文件,`>>`不覆盖原文件内容。

- 2>> : 标准错误重定向。shell用0，1，2表示标准输入、标准输出、标准错误。

# 管道
管道用`|`符把一个命令的标准输出传递到下一个命令的标准输入。

- |: Command 1 | Command 2;

管道过滤器命令：

- uniq: 删除列表中重复的行。

- wc(word counter): 输出输入文件的 行数 字数 字符数

- grep(grab regular expression) :  grep pattern filename; 用正则表达式从标准输入筛选内容。

- head / tail : 默认情况 输出 标准输入的前／后 10行。用`-n number` 来改变输出行数。

# 进程控制

- ps: 查看运行的进程信息

- top: 动态显示进程信息

- [命令]& : 使程序后台运行

- jobs : 显示用户的正在运行的程序

- fg %[pid]: 让后台程序进入前台

- bg %[pid] : 让前台程序进入后台

- kill [pid] : 杀死进程
