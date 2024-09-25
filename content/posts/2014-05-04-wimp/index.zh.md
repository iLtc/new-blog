---
title: 'Windows Server 搭建 PHP + MySQL 环境'
date: 2014-05-04
tags:
- Windows Server
- IIS
- MySQL
- PHP
---

这几天在练习给一台 Windows Server 2008 服务器配置 PHP + MySQL 环境，把过程简单记录下来，方便以后查询。

<!--more-->

## 一、添加 Web 服务器

首先进入“服务器管理器”，看到左侧“角色”节点下没有角色，点击“添加角色”。

在弹出的“添加角色向导”对话框中勾选“Web 服务器(IIS)”，两次点击“下一步”。

{{< figure src="images/2014050401.png" class="figure-center" >}}

在“角色服务”中额外勾选“CGI”，再次点击“下一步”和“安装”以启动安装。

{{< figure src="images/2014050402.png" class="figure-center" >}}

完成安装后，可以看到左侧“角色”节点下多了一个“Web 服务器(IIS)”。

{{< figure src="images/2014050403.png" class="figure-center" >}}

此时打开浏览器，输入 `http://localhost` ，按回车，如果能看到“IIS欢迎界面”，说明“Web 服务器(IIS)”添加成功。

{{< figure src="images/2014050404.png" class="figure-center" >}}

## 二、安装 PHP

考虑到这台服务器以后主要是用来运行一个 Discuz! 论坛，在这里选择 PHP5.3 。

进入 PHP for Windows 的专题下载页面( http://windows.php.net/download/ )，可以看到左侧有一些安装建议。

> If you are using PHP as FastCGI with IIS you should use the Non-Thread Safe (NTS) versions of PHP. TS refers to multithread capable builds.NTS refers to single thread only builds. Use case for TS binaries involves interaction with a multithreaded SAPI and PHP loaded as a module into a web server. For NTSbinaries the widespread use case is interaction with a web server through the FastCGI protocol, utilizing no multithreading (but also for example CLI).
>  根据建议，我们应该选择 PHP 5.3 VC9 x86 Non Thread Safe 版本，并应当先安装 Microsoft Visual C++ 2008 Redistributable Package （页面左侧有下载地址，选择32位或者64位版本）。

页面上还可以选择下载 PHP 的 ZIP 压缩包还是 Windows Installer 安装程序，考虑到安装的便捷性，同时服务器配置好后一般不用更新软件，所以这里选择使用 Windows Installer 安装程序来进行安装。

下载并运行安装程序，注意在“Web Server Setup”中选择“IIS FastCGI”。

{{< figure src="images/2014050405.png" class="figure-center" >}}

安装完成之后，将以下代码保存为 info.php ，并放到 C:\inetpub\wwwroot 目录下，然后在浏览器中访问 http://localhost/info.php ，若能看到“phpinfo()”页面，则说明 PHP 安装成功。

```PHP
<?php phpinfo();
```

{{< figure src="images/2014050406.png" class="figure-center" >}}

## 三、安装MySQL

从 MySQL 官网下载社区版的 MySQL ( http://dev.mysql.com/downloads/ )。

如果安装程序提示缺少“.NET Framework 4”，可以到[这里](http://www.microsoft.com/zh-cn/download/details.aspx?id=17718)下载“Microsoft .NET Framework 4（独立安装程序）”。

在安装程序的欢迎界面选择“Install MySQL Products”，同意一段协议后，会询问是否需要联网更新，这里可以选择跳过。

{{< figure src="images/2014050407.png" class="figure-center" >}}

{{< figure src="images/2014050408.png" class="figure-center" >}}

接下来选择安装类型，由于这台服务器并不是用来开发的，所以没有必要把一些开发工具都装上，选择“Server Only”就好，在右边设置安装路径，建议不要把 MySQL 安装在系统盘，这样以后重装比较容易维护。

{{< figure src="images/2014050409.png" class="figure-center" >}}

接下来一路“Next”，到配置界面时再注意设置一下，“Config Type”选择“Server Machine”。

{{< figure src="images/2014050410.png" class="figure-center" >}}

在接下来的界面设置 Root 用户的密码，其他用户的信息可以以后再配置。

继续一路“Next”即可完成安装。

{{< figure src="images/2014050411.png" class="figure-center" >}}

接下来随便用一个 MySQL 客户端链接一下成功即可。