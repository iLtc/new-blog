---
title: 'Installing Development Environment on CentOS'
date: 2015-12-26
tags:
- Linux
- CentOS
---

Although I haven't used the CentOS development environment much recently, a friend previously asked about what needs to be installed for a CentOS compilation environment. After researching, I'm making a note of it here.

<!--more-->

## 1. Complete Command

```Bash
yum install gcc gcc-c++ gcc-g77 flex bison autoconf automake bzip2-devel zlib-devel ncurses-devel libjpeg-devel libpng-devel libtiff-devel freetype-devel pam-devel openssl-devel libxml2-devel gettext-devel pcre-devel
```

## 2. Simplified Command

```Bash
yum groupinstall "Development tools"
```

Reference:
[http://blog.chinaunix.net/uid-26204366-id-3202688.html](http://blog.chinaunix.net/uid-26204366-id-3202688.html)