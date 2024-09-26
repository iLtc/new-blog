---
title: 'MySQL 使用中遇到的错误整理'
date: 2014-09-08
tags:
- MySQL
---

在使用 MySQL 的过程中经常会碰到一些奇怪的错误，决定整理一下，以便以后参考~

## InnoDB: Cannot allocate memory for the buffer pool

这个错误是在安装 MySQL 5.6 时遇到的，安装好后启动 mysqld 服务时一直报错，打开 /var/log/mysqld.log 看到里面有一句错误提示

```
[ERROR] InnoDB: Cannot allocate memory for the buffer pool
```

上网查了一下，发现是配置文件 /etc/my.cnf 中”innodb_buffer_pool_size”值没有设置或设置的不合适，于是打开该配置文件，看到有关于”innodb_buffer_pool_size”的说明，根据说明将”innodb_buffer_pool_size”前面的 # 去掉，然后给他分配一个内存值，保存退出即可。

重新启动 mysqld 服务，一切正常。
