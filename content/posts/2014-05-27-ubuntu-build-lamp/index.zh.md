---
title: 'Ubuntu 搭建 LAMP 环境'
date: 2014-05-27
tags:
- Linux
- Apache
- MySQL
- PHP
- Ubuntu
---

最近因为自己的开发环境变成了 Ubuntu ，所以研究了一下在 Ubuntu 中搭建 LAMP 的过程。

<!--more-->

## 1.安装前的准备

如果是给服务器配置 LAMP 环境，需要找一款顺手的终端模拟器，之前在另一篇文章中曾推荐过，可以点击 [这里](http://blog.iltc.io/20140112-centos-build-lamp "CentOS 搭建 LAMP") 查看。

## 2.一步完成 LAMP 配置

因为前前后后把 Ubuntu 重装了好几次，所以想要找一个更快捷配置环境的办法，结果在网上搜了一下，还真让我找到了。

直接在终端内输入以下命令，即可自动选择和安装各种软件包。

```Bash
$ sudo apt-get install lamp-server^
```

安装过程中会提示设置 MySQL 的密码，如果不想设置可以直接按回车跳过。（如果第一次没有设置密码，可能后面会再出现两次设置密码的提示，都按回车键跳过就好）

安装成功后，打开浏览器，访问 `http://localhost` （本机）或者服务器地址，看到 Apache2 的欢迎界面，即说明安装成功。也可以制作一个 `phpinfo.php` 页面，来检测一下 PHP 的安装情况，可以参考 [这里](http://blog.iltc.io/20140112-centos-build-lamp "CentOS 搭建 LAMP") 。

如果 Apache2 启动的时候提示

```Bash
apache2: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppress this message
```

可以通过在终端内输入以下内容来解决。

```Bash
$ echo "ServerName localhost" | sudo tee /etc/apache2/conf.d/fqdn sudo service apache2 reload
```

## 3.安装 PHPMyAdmin

以前我都是直接从 PHPMyAdmin 的网站下载源代码放到服务器上使用的，后来发现原来还可以直接使用终端命令来安装，这样做的好处是安装脚本可以自动安装一些扩展软件，并完成一些配置，避免后期手动配置的麻烦。

```Bash
$ sudo apt-get install libapache2-mod-auth-mysql phpmyadmin
```

安装过程中会询问一些问题，由于提示比较简单，这里就不做讲解了，有需要的朋友可以查看本文下方的参考资料，里面有更详细的安装过程。

安装完成以后还没有办法立即访问，应为 PHPMyAdmin 的代码在 /usr/share/phpmyadmin/ 中，我们需要把它映射到我们的 WEB 目录才能访问：

```Bash
$ sudo ln -s /usr/share/phpmyadmin/ /var/www/html
```

现在打开浏览器，访问 `http://localhost/phpmyadmin/` ，就可以使用了。

本文参考：
[http://os.51cto.com/art/201307/405333_all.htm](http://os.51cto.com/art/201307/405333_all.htm "如何在Ubuntu上安装LAMP服务器系统？")