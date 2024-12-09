---
title: 'How to Change the Root User Password in CentOS'
date: 2014-01-26
tags:
- Root
- Password
- CentOS
- Linux
---

I don't remember exactly where I saw an article that mentioned how to change the root user password in CentOS. The method described was so complicated that I didn’t dare to try it for a long time. Instead, I used third-party software (like Webmin or the control panel provided by the server host) to change the root user password.

It wasn’t until I later came across another article that I realized changing the root user password in CentOS is "as simple as breathing."

<!--more-->

First, log into the server via SSH using the root account, and then enter:

```Bash
$ passwd
```

Follow the prompts to change the password:

```Bash
Changing password for user root.

New password:
Retype new password:
```

When entering the new password, the characters will be hidden, so there’s no need to worry about anyone seeing it.

Additionally, if the new password is too simple, you might see a message like this:

```Bash
BAD PASSWORD: it is WAY too short BAD PASSWORD: is too simple
```

Note that even if this message appears, the system will still prompt you to re-enter the password. If you wish to use a stronger password, you will need to perform the operation again.

Reference:
[http://lxy.me/centos-change-the-root-user-password.html](http://lxy.me/centos-change-the-root-user-password.html)