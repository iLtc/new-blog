---
title: 修改 Docker 默认网桥地址
date: 2016-06-01
tags:
- Docker
- 网络
---

学校工作室的同学反映使用 Docker 之后，其默认的 172 网桥地址和某个宿舍区的网段冲突了，导致该区的同学没有办法正常访问 Docker 应用。翻了一下官方的帮助文档，找到了修改默认网桥地址的办法。

首先停止正在使用的 Docker 服务：

```Bash
$ sudo service docker stop
```

接着删除 Docker 默认网桥 `docker0` ：

```Bash
$ sudo ip link set dev docker0 down
$ sudo brctl delbr docker0
$ sudo iptables -t nat -F POSTROUTING
```

<!--more-->

然后创建一个新的网桥，注意 `bridge0` 可以换成其他名称， `192.168.5.1/24` 也可以换成你喜欢的其它网段：

```Bash
$ sudo brctl addbr bridge0
$ sudo ip addr add 192.168.5.1/24 dev bridge0
$ sudo ip link set dev bridge0 up
```

此时一下新网桥运新是否正常：

```Bash
$ ip addr show bridge0
4: bridge0: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state UP group default
    link/ether 66:38:d0:0d:76:18 brd ff:ff:ff:ff:ff:ff
    inet 192.168.5.1/24 scope global bridge0
       valid_lft forever preferred_lft forever
```

将新的网桥写入 Docker 默认配置文件，并启动 Docker ：

```Bash
$ echo 'DOCKER_OPTS="-b=bridge0"' >> /etc/default/docker
$ sudo service docker start
```

本文参考： [https://docs.docker.com/v1.8/articles/networking/#bridge-building](https://docs.docker.com/v1.8/articles/networking/#bridge-building)