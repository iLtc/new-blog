---
title: 'How to Fix "Failed to Open libnotify" Error in MentoHUST'
date: 2014-05-26
tags:
- MentoHUST
- Ubuntu
aliases:
- /article/linux/ubuntu-mentohust-libnotify.html
---

I recently started trying to do some development work on Linux. Since our campus network requires the RuiJie (锐捷) client for internet access, and RuiJie's Linux client isn't very convenient to use, I switched to an alternative solution called [MentoHUST](https://code.google.com/p/mentohust/).

<!--more-->

Previously, I had no issues using MentoHUST for network access on CentOS. However, after switching to Ubuntu, I kept getting the error "打开libnotify失败，请检查是否已安装该库文件 (Failed to open libnotify, please check if the library file is installed)" every time I tried to connect. This isn't a major issue - it just means the software can't properly show desktop notifications. Since I usually run MentoHUST in the background, it would be inconvenient not to see notifications when the connection drops occasionally, so I decided to fix this.

I searched online for many solutions and tried them one by one for hours, but nothing worked.

Finally, I decided to analyze MentoHUST's source code. Looking through the official V2 source code package in the src directory, I found the error message in dlfunc.c:

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

This shows that the program looks for `libnotify.so` and `libnotify.so.1`. After searching in `/usr/lib/x86_64-linux-gnu/`, I only found `libnotify.so.4` and `libnotify.so.4.0.0`. So I created a symbolic link:

```Bash
$ sudo ln -s /usr/lib/x86_64-linux-gnu/libnotify.so.4.0.0 /usr/lib/x86_64-linux-gnu/libnotify.so.1
```

After restarting MentoHUST, desktop notifications worked successfully when connecting to the network.
