---
title: Modify Docker Default Bridge Address
date: 2016-06-01
tags:
- Docker
- Networking
---

A colleague from the school studio reported that after using Docker, its default 172 bridge address conflicted with a subnet in one of the dorm buildings, preventing students in that area from accessing Docker applications properly. After going through the official documentation, I found a way to modify the default bridge address.

First, stop the Docker service:

```Bash
$ sudo service docker stop
```

Next, delete Docker's default bridge `docker0`:

```Bash
$ sudo ip link set dev docker0 down
$ sudo brctl delbr docker0
$ sudo iptables -t nat -F POSTROUTING
```

<!--more-->

Then, create a new bridge. Note that you can replace `bridge0` with any other name, and `192.168.5.1/24` can also be replaced with another subnet of your choice:

```Bash
$ sudo brctl addbr bridge0
$ sudo ip addr add 192.168.5.1/24 dev bridge0
$ sudo ip link set dev bridge0 up
```

Check if the new bridge is functioning correctly:

```Bash
$ ip addr show bridge0
4: bridge0: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state UP group default
    link/ether 66:38:d0:0d:76:18 brd ff:ff:ff:ff:ff:ff
    inet 192.168.5.1/24 scope global bridge0
       valid_lft forever preferred_lft forever
```

Now, write the new bridge configuration into Docker’s default configuration file and start Docker:

```Bash
$ echo 'DOCKER_OPTS="-b=bridge0"' >> /etc/default/docker
$ sudo service docker start
```

Reference: [https://docs.docker.com/v1.8/articles/networking/#bridge-building](https://docs.docker.com/v1.8/articles/networking/#bridge-building)