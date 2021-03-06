---
title: linux 服务器常用命令整理
date: 2018-07-21 21:24:15
categories: 
- linux
tags: 
- linux
- cmd
---

# linux 服务器常用命令整理

### 目录

- **网络分析 - tcpdump \ telnet \ (netstat \ ss \ lsof) \ nload**
- **网络传输 - scp \ rsync \ (rz \ sz) \ nc**
- **抓包工具 - charles**
- **内存检查 - free \ meminfo**
- **系统监控 - vmstat \ iostat \ top \ ps \ sar \ dstat**
- **系统调用追踪 - strace \ gcore**
- **文件相关 - find \ awk \ sed \ grep \ tail \ df \ du \ locate**
- **开发效率 - tmux**

### 常见命令

#### tcpdump

1. tcp:用来过滤数据报的类型
2. -i eth1 : 只抓经过接口eth1的包
3. -t : 不显示时间戳
4. -s 0 : 抓取数据包时默认抓取长度为68字节, 加上-S 0 后可以抓到完整的数据包
5. -c 100 : 只抓取100个数据包
6. dst port !22: 不抓取目标端口是22数据包
7. src net 10.99.184.0/24 : 数据包的源网络地址为10.99.184.0/24
8. -A：显示数据包内容 

示例：

`tcpdump -i any -v port 8888`

`tcpdump -i any -A port 8888  `

![](https://ws2.sinaimg.cn/large/006tNc79ly1fp6dysyqizj30lb08mt9e.jpg)

![](https://ws3.sinaimg.cn/large/006tNc79ly1fp6dzuovqqj30w6093jtc.jpg)



#### netstat

查看所有连接

`netstat -autnp`

查看监听的tcp服务

`netstat -altnp`  

看tcp端口

`netstat -ltnp`

![](https://ws2.sinaimg.cn/large/006tNc79ly1fp6e3o8h15j30ck090glz.jpg)



#### ss

- `ss -pl`   查看每个进程及其监听的端口
- `ss -t -a`  查看所有的tcp连接
- `ss -u -a`  查看所有的udp连接

#### lsof

- `lsof -i :8888`  查看端口8888进程信息
- `lsof -p 7915` 查看进程7915打开的fd信息

#### scp

- `scp -r src remote:/tmp`  本地拷贝到远端
- `scp -r remote:/tmp/src .`  远端拷贝到本地
- `scp -3  remote:/tmp/a.tar   remote2:/tmp/`  以本地为跳板机，将remote机器上文件拷贝到remote2

#### rsync

- `rsync -av /home/mail/ 192.168.11.12:/home/mail/`
- `rsync -av 192.168.11.11:/home/mail/ /home/mail/`

#### nc

- `nc -l 8888`   本地启动8888端口
- `nc -l 8888 > a.tgz`   接收文件
- `nc ali-.bj:8888   < a.tgz` 发送文件到远端

#### vmstat

- `vmstat 1 10`对内存监控，重点关注swpd、free、si、so。一般系统不繁忙的状态下，swpd、so的值不会持续很高，经常为0。如果swpd过高，那么就是系统内存经常不够用。
- 对CPU监控，我们可以查看r（运行进程数）、us、sy、id（CPU空闲），如果r的数字大于系统CPU个数，则面临CPU不够用的危险，通过id分析，如果过小，则可以判断是CPU不足。

![](https://ws1.sinaimg.cn/large/006tNc79ly1fp6e824wffj30sg09vta7.jpg)



#### iostat

- `iostat -x` 一般情况下，%util应该越小越好，10%以下正常，30%IO比较繁忙。50%以上一般是有问题的

![](https://ws2.sinaimg.cn/large/006tNc79ly1fp6e8iekncj30sg05qgm6.jpg)



#### top

- 1  按CPU核数查看
- P
- M
- c 查看完整进程命令
- top -Hp pid  查看线程数

#### ps

- `ps -eo “pid,cmd,lstart”  | grep pid`   查看进程启动时间
- `ps -ef f`  查看最近进程（常用）

#### find

- `find . -type f -mtime +3`   修改时间大于3天的文件
- `find . -type f -mtime +3 | xargs rm -rf`  查找并删除

#### du

- `du -sk * | sort -n | cut -f2 | xargs -d '\n' du -sh` 按文件大小排序显示
- `du -hs` 常用

#### awk

`grep 'update_profile.*Android' access-20180131.log |awk -F 'POST' '{print $2}'|awk -F '&' '{print $26}'|awk -F ' ' '{print $1}'|awk -F '=' '{print $2}'|sort -n|uniq -c|sort -nr|head -100`

-F 以空格分割