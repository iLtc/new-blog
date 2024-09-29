---
title: 'CentOS 安装编译环境'
date: 2015-12-26
tags:
- Linux
- CentOS
---

虽然最近没怎么用过 CentOS 的开发环境，但是之前有朋友问过 CentOS 编译环境需要安装哪些东西，查询之后在这里做个记录。

<!--more-->

## 1. 完整命令

```Bash
yum install gcc gcc-c++ gcc-g77 flex bison autoconf automake bzip2-devel zlib-devel ncurses-devel libjpeg-devel libpng-devel libtiff-devel freetype-devel pam-devel openssl-devel libxml2-devel gettext-devel pcre-devel
```

## 2. 简化命令

```Bash
yum groupinstall "Development tools"
```

本文参考：
[http://blog.chinaunix.net/uid-26204366-id-3202688.html](http://blog.chinaunix.net/uid-26204366-id-3202688.html)