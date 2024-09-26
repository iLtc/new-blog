---
title: "为 Linux 添加软件镜像源"
date: 2014-08-14
tags:
- Linux
- Ubuntu
- Mirrors
---

使用 Linux 系统的时候总是不可避免地要为系统装一些新软件，由于系统工作机制的原因，一般来说大家都是直接通过软件源来安装软件，虽然主流的 Linux 系统都在中国大陆设立了镜像源，但因为默认镜像源访问量比较大，所以下载速度多少还是有些慢，这是可以通过添加其他镜像源来提高下载速度。

因为我的电脑平时在大学里面使用，而教育网虽然与电信网连接速度比较慢，但与教育网内的其他高校连接速度还是很快的，所以选用了中国科学技术大学的镜像源（[http://mirrors.ustc.edu.cn/](http://mirrors.ustc.edu.cn/)）。

选择中科大镜像源，除了因为速度快，还有就是帮助文档比较完善，还提供了根据不同系统不同网路环境不同需求自动生成配置代码的功能（[https://lug.ustc.edu.cn/repogen/](https://lug.ustc.edu.cn/repogen/)）。

## 手动添加

以 Ubuntu 14.04 为例，在 http 、 ipv4 条件下通过上述功能生成代码：

```
deb http://mirrors.ustc.edu.cn/ubuntu/ trusty main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu/ trusty-security main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu/ trusty-updates main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu/ trusty-proposed main restricted universe multiverse
deb http://mirrors.ustc.edu.cn/ubuntu/ trusty-backports main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu/ trusty main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu/ trusty-security main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu/ trusty-updates main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu/ trusty-proposed main restricted universe multiverse
deb-src http://mirrors.ustc.edu.cn/ubuntu/ trusty-backports main restricted universe multiverse
```

将 `/etc/apt/sources.list` 文件的内容备份后，将这段代码放到该文件的开头（需要使用 `sudo` ），保存，然后在终端中运行更新命令，即可完成更新

```bash
sudo apt-get update
```

## 自动添加

同样以 Ubuntu 14.04 为例，打开系统设置、软件和更新，在“下载自”后面的下拉菜单中选择“其他站点…”，然后在列表中选择“mirrors.ustc.edu.cn”即可。

（也可以在这里点击“选择最佳服务器”，由系统测试或选择最快的镜像源）