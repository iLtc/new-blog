---
title: 'Linux 实现 SSH 证书登录'
date: 2016-03-17
tags:
- Linux
- SSH
- 证书
---

这两天发现自己开发服务器上的登录失败次数高达一千多，登录失败的 IP 来自印度，估摸着是有人在尝试破解我的服务器。虽然开发服务器上没什么重要的数据，但老是被人惦记着总觉得不舒服，于是决定把服务器 SSH 登录切换到证书登录。

```
Last failed login: Thu Mar 17 12:38:23 EDT 2016 from 59.165.162.78 on ssh:notty
There were 1764 failed login attempts since the last successful login.
```

<!--more-->

## 1 客户端操作

### 1.1 生成私钥和公钥

``` Bash
$ ssh-keygen -t rsa
```

```
Generating public/private rsa key pair.

Enter file in which to save the key (/Users/iLtc/.ssh/id_rsa): # 密钥存放位置，一般来说默认位置就好，但是如果之前生成过其他密钥，则注意避免冲突

Enter passphrase (empty for no passphrase): # 可以留空

Enter same passphrase again:

Your identification has been saved in /Users/iLtc/.ssh/id_rsa.
Your public key has been saved in /Users/iLtc/.ssh/id_rsa.
The key fingerprint is:
SHA256:Z1C2jmXFA+mk/UVC5GPq0gFapJ10LAG3E0HNMzKWfnA iLtc@iLtcMac
The key's randomart image is:
+---[RSA 2048]----+
|      .oOB=*+    |
|       *BBE++ .  |
|      .oO@+o++   |
|       ooO+o ..  |
|      . S.*. .   |
|         = ..    |
|        . o      |
|         .       |
|                 |
+----[SHA256]-----+
```

### 1.2 将公钥上传到服务器

``` Bash
# 注意替换公钥路径
$ scp ~/.ssh/id_rsa.pub username@<ssh_server_address>:~
```

## 2 服务端操作

### 2.1 将公钥写入授权文件内

``` Bash
# 注意替换公钥名称
$ cat id_rsa.pub >> ～/.ssh/authorized_keys
```

### 2.2 配置 SSH 服务使其支持证书登录

``` Bash
$ sudo vim /etc/ssh/sshd_config
```

```
# 是否允许用户自行使用成对的密钥系统进行登入行为
RSAAuthentication yes
PubkeyAuthentication yes

# 这一行不用修改，留意一下默认的授权文件地址无误即可
AuthorizedKeysFile .ssh/authorized_keys
```

### 2.3 重启 SSH 服务

``` Bash
$ sudo service sshd restart
```

## 3 测试

退出服务器，然后尝试用密钥登录。

``` Bash
# 注意替换公钥路径
$ ssh -i ~/.ssh/id_rsa username@<ssh_server_address>
```

登录成功即可

## 4 关闭 SSH 密码登录（可选）

因为我不希望有人继续尝试破解我的服务器密码，于是决定关闭 SSH 密码登录。在执行这个操作前请务必确认已经开启密钥登录并能够成功登录。

### 4.1 配置 SSH 服务关闭密码登录

``` Bash
$ sudo vim /etc/ssh/sshd_config
```

```
PasswordAuthentication no
```

### 4.2 重启 SSH 服务

``` Bash
$ sudo service sshd restart
```

## 4 测试

``` Bash
$ ssh username@<ssh_server_address>
```

```
Permission denied (publickey,gssapi-keyex,gssapi-with-mic).
```

本文参考：
[http://www.cnblogs.com/ggjucheng/archive/2012/08/19/2646346.html](http://www.cnblogs.com/ggjucheng/archive/2012/08/19/2646346.html)