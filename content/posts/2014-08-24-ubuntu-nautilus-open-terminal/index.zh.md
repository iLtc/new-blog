---
title: 'Ubuntu 添加右键菜单“在终端中打开”功能'
date: 2014-08-24
tags:
- Ubuntu
- Nautilus
- Linux
- Terminal
---

之前使用 CentOS 桌面版时，发现右键菜单中的“在终端中打开”功能非常方便，可以省去打开终端以后定位文件夹的麻烦。

但是换到 Ubuntu 系统时发现找不到这个功能了，在网上搜索了一下，发现可以通过安装软件来添加这个功能：

```
sudo apt-get install nautilus-open-terminal
```

安装好后重启系统，就可以在右键菜单中找到相应的功能了。

（如果安装软件时提示找不到软件包，可能是软件源里面没有这个安装包，请参考 [这篇文章](http://blog.iltc.io/article/linux/add-mirrors-for-linux.html) 为系统添加一个新的镜像源再试。)

本文参考

http://www.linuxidc.com/Linux/2012-05/59565.htm
