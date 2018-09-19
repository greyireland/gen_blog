---
title: go-文件io
date: 2018-09-19 10:47:35
categories: 
- go
tags:
- go
---
# 底层读写
### 底层IO
~~~
file=open(path)
file.read(buf)
file.write(buf)
file.readAt(buf,offset)
file.writeAt(buf,offset)
~~~

### 缓冲IO
~~~
bufio.Reader/Writer
file=open(path)
bufFile=bufio.NewReader(file)
bufFile.read(buf)

file=open(path)
bufFile = bufio.NewWriter(file)
bufFile.Write([]byte("haha"))
w.Flush()//将bufFile里面的数据刷到file里面去，操作系统可能还有一层buf！
~~~
>  标准IO操作数据流向路径：数据—>进程缓冲（用户态）—>内核缓存区（内核态）—>磁盘
> 为什么包一层buf，buf读的时候读一大块，给你读取的时候，你只需要从buf里面去读一点数据，下次再读一点数据，不用每次读取都去调用系统库，buf写的时候，当写满一大块的时候，才真正调用系统写，因为不用每次写都去调用系统写，这样会提高性能，但数据可能丢失或是不一致的情况
> 

#### 任务
1.  给10G的文件排序？

