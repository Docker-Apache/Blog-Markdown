---
title: shell 程序设计
date: 2021-01-17 15:13:12
tags:
    - shell
category:
    - Linux
---
## shell 的语法
* 变量：字符串、数字、环境和参数
* 条件：shell中的布尔值
* 程序控制：if、elif、for、while、until、case
* 命令列表
* 函数
* shell 内置命令
* 获取命令的执行结果
* here 文档

## 变量
> 须知
* 默认情况下，所有变量都被看作字符串并以字符串来存储，即使它们被赋值为数值时也是如此
* shell 和一些工具程序会在需要时把数值型字符串转换为对应的数值以对它们进行操作
* Linux 是一个区分大小写的系统，因此 shell 认为变量 foo 与 Foo 是不同的

> 在变量名前加一个 `$` 符号来访问它的内容
```shell
$ var=hello
$ echo $var
hello
$ var="hello world"
$ echo $var
hello world
$ var=7+5
$ echo $var
7+5
```
* 为变量赋值时，只需要使用变量名，该变量会根据需要被自动创建
* 若字符串里包含空格，则必须用引号把它们括起来
* 等号两边不能有空格

> 使用 `read` 命令将用户的输入赋值给一个变量
```shell
$ read var
my name is Ken.
$ echo $var
my name is Ken.
```

### 一个实例
> 创建一个文件，并使用 `vi` 命令进入 `vim` 编辑器
```shell
$ touch useVar.sh   # linux 中文件后缀并不重要，而指明后缀只是让用户清楚文件类型
$ vi useVar.sh
```

> 在 `vim` 中编辑文件
```shell
#！/bin/sh
myvar="Hi there"

echo $myvar
echo "$myvar"
echo '$myvar'
echo \$myvar

echo Enter some text:
read myvar

echo '$myvar' == $myvar
exit 0
```
* 第一行的 `#!/bin/sh` 是一种特殊的注释，`#!` 字符后面紧跟的参数用于指明执行本文件的程序
* 本例中，`/bin/sh` 是默认的 shell 程序

> 两种执行脚本的方法
```shell
$ /bin/sh useVar.sh     # 调用 shell 程序，并将文件名作为一个参数
```
```shell
$ chmod +x useVar.sh    # 将文件模式修改为 -> 可执行
$ ./useVar.sh           # 执行文件。注意是当前目录 ./useVar.sh，而不是根目录 /usevar.sh
```

> 输出结果
```shell
Hi there
Hi there
$myvar
$myvar
Enter some text:
Hello World
$myvar == Hello World
```

### 环境变量
* `$HOME` 当前用户的家目录
* `$PATH` 以冒号分隔的用来搜索命令的目录列表
* `$PS1` 命令提示符。通常是 `$` 字符
* `$PS2` 二级提示符。用来提示后续的输入，通常是>字符
* `$IFS` 输入域分隔符。通常是空格、制表符和换行符
* `$0` shell 脚本的名字。默认是 bash
* `$#` 传递给脚本的参数个数
* `$$` shell 脚本的进程号。可以用 `ps` 命令查看当前运行的所有进程

### 参数变量
* `$1, $2, ...` 传递给脚本的参数
* `$*` 在一个变量中列出所有的参数，各个参数之间用环境变量IFS中的第一个字符分隔开
* `$@` 不使用 `IFS` 环境变量，所以即使 `IFS` 为空，参数也不会挤在一起。访问脚本参数时，应使用 `$@`