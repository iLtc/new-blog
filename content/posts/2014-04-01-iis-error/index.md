---
title: 'IIS Error: It is being used by another process'
date: 2014-04-01
tags:
- Windows Server
- IIS
---

While maintaining a server today, I tried to start a website, but I kept getting the error: "The process cannot access the file because it is being used by another process."

<!--more-->

Online sources suggested that this issue occurs because the port is being used by another program. So, I tried entering the following command in the command prompt:

```Bash
netstat -obna
```

This allowed me to check the usage of various ports on the server. I found out that an FTP service was occupying port 80. I promptly disabled this service, restarted the website, and everything worked smoothly!

Reference:
[http://www.cnblogs.com/eagle1986/archive/2010/01/17/1649908.html](http://www.cnblogs.com/eagle1986/archive/2010/01/17/1649908.html)