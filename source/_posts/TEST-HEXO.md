---
title: TEST HEXO
date: 2019-07-15 22:36:49
tags:
---


# Shell常用命令学习
---

## xargs : execute arguments

    用法 ： 将后续内容（通常管道传递）转成后续命令的参数

#### examples
```sh
## 普通使用
ls *.js | xargs ls -al

## 参数替换 -I
## 把所有jce文件替换为
ls *.jce | xargs -I name mv name name.bak

## 参数分组 -nx
## 针对不支持参数过长的命令
ls * | xargs -t -n2 ls -al
```

1. 统计目录下所有go文件代码行数
    `find ./ -type f -name "*.go" -print0 | xargs -0 wc -l`
    

## find

```
1. 权限过滤 
    find . -type f -perm 0777 
2. 非逻辑
    find . -type f ! -name "*.go"
3. 对结果执行（除了xargs）
    find . -type f -name "*.log" -exec rm -rf {} \; # 分条传递
    find . -type f -name "*.log" -exec rm -rf {} +  # 一起传递
4. 查找空文件
    find . -type f -empty
5. 文件操作时间查询
    find . -mtime 50 # 查找50天修改文件（mmin按分钟）
    find . -atime +50 -atime -100 # 查找50-100天访问文件（amin按分钟）
6. 文件大小查询
    find / -size +50M -size -100M -exec rm -rf {} \;
```

## strace

集诊断调试与统计于一体的工具，可跟踪应用的系统调用和信号传递来进行应用分析。

```sh
1. 追踪程序运行过程
    strace [cmd] 
2. 统计系统调用的时间，调用量及错误等
    strace -c [cmd]
3. 追踪现有进程
    strace -p pid
```

macos下对应使用dtruss命令

## lsof

列出进程打开的所有文件信息。包括普通文件，块文件，字符文件，共享库，管道，socket等。

```
1. 列出使用文件的进程
    lsof [file1] [file2]
2. 查找进程打开的所有文件
    lsof -c follow # 查找follow开头的进程
    lsof -c "/.*video/" # 查找满足正则的进程
3. 查找特定进程打开文件
    lsof -p 123,456,789 
4. 列出所有打开了网络套接字的进程
    lsof -i [tcp|udp|tcp:80]
5. 列出所有内存映射文件
    lsof -d [FD|FDSET] # FDSET : mem/txt/mmap/...
6. 逻辑关联性
    lsof -a # 默认多条件为或 -a会改变成与
```
example ： 每秒打印一次root用户使用网络文件
`lsof -r 1 -u root -i -a`

## cut

切割字串

```
1. 切割范围
    cut -c[LIST] # 固定列
    cut -f[LIST] # 固定字段
2. 分割控制
    cut -d [delimiter] 分割符
    cut -s 无分割符不返回
3. LIST写法
    [N|N1,N2|N1-|N1-N2|-N2]
```

## strings

打印文件中可打印的字符
可搜索库或二进制中的字符。如在libc.so.6是c标准库，而这个标准库的制作者为了让库的使用者知道该库兼容哪些版本的标准库，就在这个库中定义了一些字符串常量。可通过`strings`查看兼容版本。

```
strings /lib64/libc.so.6 | grep GLIBC
```

## sed

```
1. sed 's/[ ][ ]*/,/g' 正则替换
```


