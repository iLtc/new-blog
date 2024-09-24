---
title: 'CentOS 搭建 LAMP 环境'
date: 2014-01-12
tags:
- Linux
- Apache
- MySQL
- PHP
- CentOS
- yum
slug: centos-build-lamp
aliases: /zh/centos-build-lamp.html
---

在 CentOS 上搭建 LAMP （ Linux + Apache + MySQL + PHP ）环境的方法有很多，一键安装包、编译、 yum ，各种方法让人眼花缭乱，经过几番尝试，我觉得还是 yum 方法跟简单而且更灵活一些。

网上面通过 yum 方法搭建 LAMP 环境的教程有很多，几番尝试和整理后，自己编写了这份教程。

<!--more-->

## 1. 安装前的准备

其实也没有什么特别要准备的，不过建议准备一款顺手的终端模拟器，在 Windows 上可以使用 [Xshell](https://www.netsarang.com/products/xsh_overview.html)。

如果使用 Xshell 的话注意在安装时在 `Setup Type` 中选择 `Free for Home/School` ，这样以后就不会收到购买提示了。

在 MacOS 上可以直接使用自带的终端，或者 [iTerm](https://www.iterm2.com/) 。

另外建议在搭建LAMP环境前先更新一下Centos自带的组件，避免安装时出现错误：

```Bash
$ yum update
```

还有需要注意的是，搭建 LAMP 环境一定要注意一下组件的安装顺序，如果颠倒了顺序可能会导致安装失败。

## 2. 安装MySQL

由于 yum 库里自带的 MySQL 版本比较低，所以在这里先安装 MySQL 官方的 yum 源（可以在 [这里](http://dev.mysql.com/downloads/repo/yum/) 找到），再安装 MySQL

```Bash
$ rpm -Uvh http://dev.mysql.com/get/mysql-community-release-el6-5.noarch.rpm
```

用下列命令安装MySQL：

```Bash
$ yum install mysql-server
```

接下来需要创建MySQL系统的启动键链接启动MySQL服务器，目的是使MySQL在每次系统启动时自动启动：

```Bash
$ chkconfig --levels 235 mysqld on
```

现在就可以启动 MySQL 服务器了：

```Bash
$ /etc/init.d/mysqld start
```

安装好MySQL后还要为它的默认账户root设置密码：

```Bash
$ mysql_secure_installation
```

屏幕上会滚出大量文字，根据提示操作：

```Bash
Enter current password for root (enter for none)：
#请输入root账户的密码
#由于root账户没有密码，所以什么都不用输入，直接按回车跳过

Set root password? [Y/n]
#设置root的密码吗？
#输入Y，并按回车继续

New password:
#新密码
#输入新密码（密码不会出现在屏幕上），并按回车继续

Re-enter new password:
#再次输入新密码

Remove anonymous users? [Y/n]
#删除匿名访问账户吗？
#输入Y，并按回车继续

Disallow root login remotely? [Y/n]
#不允许远程root登录?(“远程登陆”指从其它服务器上登陆)
#输入Y，并按回车继续

Remove test database and access to it? [Y/n]
#删除测试数据？
#输入Y，并按回车继续

Reload privilege tables now?
#重新加载权限表?
#输入Y，并按回车继续

All done! If you've completed all of the above steps, your MySQL installation should now be secure. Thanks for using MySQL!
#看到这个提示代表MySQL设置完毕。
```

至此，MySQL的安装工作结束。

## 3. 安装Apache

直接通过 yum 安装：

```Bash
$ yum install httpd
```

然后设置设置系统启动时自启动 Apache ，并启动 Apache ：

```Bash
$ chkconfig --levels 235 httpd on
$ /etc/init.d/httpd start
```

现在， Apache 也安装成功，在地址栏里输入 IP 地址，即可看到 Apache2 的测试页面。

## 4. 安装 PHP

通过下面的命令来安装 PHP 和 Apache PHP 模块：

```Bash
$ yum install php
```

然后需要重启一下 Apache 服务器：

```Bash
$ /etc/init.d/httpd restart
```

此时如果想看一下服务器上有关 PHP 的信息，可以将以下代码保存为 `info.php`，然后放到服务器 `/var/www/html` 目录下，直接在浏览器中访问 `http://IP/info.php` 来查看：

```PHP
<?php phpinfo();
```

## 5. 使 PHP 支持 MySQL

到目前为止，虽然我们已经成功地安装了 MySQL 和 PHP ，但 PHP 程序还不能访问 MySQL 数据库，我们还要安装一些模块来使 PHP 支持 MySQL ：

```Bash
$ yum install php-mysql php-common php-mbstring php-gd php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc
```

再次重启 Apache ：

```Bash
$ /etc/init.d/httpd restart
```

此时在浏览器中访问 `http://IP/info.php` ，可以发现页面上多了 MySQL 一项

到这里，我们的 LAMP 环境基本上就搭建完成了，之后就是对服务器一些配置文件的针对性修改和部署网站，在以后的文章中会有所提及……

本文参考：

[http://www.centos.bz/2011/04/centos-yum-install-lamp-apache-mysql-php/](http://www.centos.bz/2011/04/centos-yum-install-lamp-apache-mysql-php/)

[http://server.zol.com.cn/279/2795606.html](http://server.zol.com.cn/279/2795606.html)