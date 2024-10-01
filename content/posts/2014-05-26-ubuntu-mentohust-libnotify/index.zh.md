---
title: 'Mentohust 提示“打开libnotify失败”解决办法'
date: 2014-05-26
tags:
- MentoHUST
- Ubuntu
aliases:
- /article/linux/ubuntu-mentohust-libnotify.html
---

最近开始尝试在 Linux 系统上做一些开发工作，因为校园网需要使用锐捷客户端来联网，但是锐捷提供的 Linux 版客户端用起来不是很方便，于是转而使用网上的一个替代产品 [MentoHUST](https://code.google.com/p/mentohust/) 。

<!--more-->

之前在 CentOS 上用 MentoHUST 联网没有什么问题，但后来换到 Ubuntu 上的时候每次联网都会提示“打开libnotify失败，请检查是否已安装该库文件”。其实这也不是什么大问题，只是软件没有办法正确弹出桌面通知，又因为我一般是让 MentoHUST 在后台运行，偶尔掉线时看不到桌面通知会比较奇怪，于是还是决定修复一下。

在网上所搜了很多解决办法，一个一个尝试了半天，仍然解决不了。

最后决定分析一下 MentoHUST 的源代码，在官方提供的 V2 源代码包的 src 源代码目录里面翻了一下，发现上面的提示在 dlfunc.c 中：

```C
#ifdef MAC_OS
    char *file[] = {"libnotify.dylib", "libnotify.1.dylib"};
    int i, count = 2;
#else
    char *file[] = {"libnotify.so", "libnotify.so.1"};
    int i, count = 2;
#endif
    for (i=0; i<count && !libnotify; i++) {
        libnotify = dlopen(file[i], RTLD_LAZY);
        error = dlerror();
    }
    if (libnotify == NULL) {
        printf("!! 打开libnotify失败，请检查是否已安装该库文件。\n");
        return -1;
    }
```

也就是说程序会去寻找 ```libnotify.so```, ```libnotify.so.1``` ，在 /usr/libx86_64-linux-gnu/ 中搜索了一下，只找到 ```libnotify.so.4```, ```libnotify.so.4.0.0```两个文件，于是给他们做个软链接：

```Bash
$ sudo ln -s /usr/lib/x86_64-linux-gnu/libnotify.so.4.0.0 /usr/lib/x86_64-linux-gnu/libnotify.so.1
```

重新启动 MentoHUST ，联网成功后可以弹出桌面通知了。