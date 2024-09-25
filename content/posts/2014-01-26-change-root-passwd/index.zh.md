---
title: 'CentOS 更改 root 用户密码'
date: 2014-01-26
tags:
- Root
- Password
- CentOS
- Linux
---

以前不记得是在哪里看到的文章，提到了 CentOS 更改 root 用户密码的方法。当时描述的办法非常复杂，以至于很长一段时间我都不敢尝试，而是使用第三方的软件（比如 Webmin 或者服务器提供商的控制台）来修改 root 用户密码。

直到后来无意中看到一篇文章，才发现原来 CentOS 更改 root 用户密码“就像呼吸一样简单”。

<!--more-->

首先通过 SSH 使用 root 帐号登录服务器，然后输入

```Bash
$ passwd
```

根据提示操作，即可完成密码的修改：

```Bash
Changing password for user root.

New password:
Retype new password:
```

输入新密码的时候文字会被隐去，所以不用担心被别人看到。

另外，如果第一遍输入的新密码过于简单，则可能会看到类似于这样的提示：

```Bash
BAD PASSWORD: it is WAY too short BAD PASSWORD: is too simple
```

需要注意的是，出现这种提示后系统仍会提示重复输入密码，如果希望换用更高强度的密码则需要重新操作。

本文参考：
[http://lxy.me/centos-change-the-root-user-password.html](http://lxy.me/centos-change-the-root-user-password.html)