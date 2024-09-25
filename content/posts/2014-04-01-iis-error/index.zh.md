---
title: 'IIS 错误:另一个程序正在使用此文件'
date: 2014-04-01
tags:
- Windows Server
- IIS
---

今天在维护服务器的时候，启动了一个网站，却一直报错：“另一个程序正在使用此文件，进程无法访问”。

<!--more-->

网上说这是因为端口被其他程序占用，于是尝试在命令提示符中输入

```Bash
netstat -obna
```

查看一下服务器各个端口的使用情况，发现原来是一个FTP服务占用了80端口，果断禁用这个服务，再启动网站，一切顺利！

本文参考：
[http://www.cnblogs.com/eagle1986/archive/2010/01/17/1649908.html](http://www.cnblogs.com/eagle1986/archive/2010/01/17/1649908.html)