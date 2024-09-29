---
title: '通过 Let''s Encrypt 申请免费的 SSL 证书'
date: 2016-01-05
tags:
- SSL
- HTTPS
- 证书
---

最近需要为几个域名申请 SSL 证书，正好免费证书签发商 [Let's Encrypt](https://letsencrypt.org/) 开始公测，于是就尝试了一下自助申请。

由于 [Let's Encrypt](https://letsencrypt.org/) 官方只提供了基于命令行和接口的申请渠道，并没有提供图形化的申请界面，我就在网上搜索了一番，发现有几个大牛已经写出了图形化的在线申请系统，于是今天的申请就基于图形化界面来做吧。

<!--more-->

经过比较，我选择了 [Get HTTPS for free!](https://gethttpsforfree.com/) 这个系统，主要原因是这个系统是基于 JavaScript 的开源静态申请系统，这意味着申请系统的所有代码我们都能看到，不用担心申请系统私藏我们的证书或者密钥文件。

整个申请共分为5步，申请页面上也给除了详细的操作提示，申请起来十分方便。

## 1 验证账户信息

在自己的电脑或服务器上运行以下代码来生成账户私钥 `account.key` ：

```Bash
$ openssl genrsa 4096 > account.key
```

在运行以下代码来产生公钥：

```Bash
$ openssl rsa -in account.key -pubout
writing RSA key
-----BEGIN PUBLIC KEY-----
MIICIjANBgkqhkiG9w0BAQEFAAOCAg8AMIICcgKCAgEAvGN3oA1L4d+OwewOII9h
H8gXKfWo3tS6IO741Pj5aRrf675DyZ4v4tTYp21gHLe0vhImjphMHH778L6aywvJ
rm0qx3C4D+ihKv7S0XI2vH9cerpVRtY7wAicJvBfW+VKiNLvkBlipmbCOiI+4DT7
/WdrmbhK6Ngmf3bc0iNZ/aqx05YjkILaOF3Hs+0x6j0q57o+qhzF9f+fHXDQTcCe
lpH0qyXPgNauqcgek5DIRKCfvUgfsiKnwPKvKbWL6pZA7acDWr0WtDB22z6kHCqG
tTRPNASJC+lAGh40OdFdAkCnQqtp0+0fBHh2Bx1P6O80pbeEedn86O6c9WGvfeNX
HyN5ndU5r2V81Y/CpcIA7521tvI3JrmJmykZUBlDWRjVjHIRIxP0cOodBa8IF3Eq
0g/LzKvwe3909h8TV4SHNRStMgVeevgbl0K0gJejf1krQa7bGnWMzaCTMvMRqkID
4hEO4GA/HYw5/jCZM0RV3U0fahpx+3KxokaOkpw8Fi83niRSv0btbRnkuJ9qKq1l
zCETTl0/NGDNnu3g04Rg3Il4C71Hxy2S/M8rm8YCfpbyyltoi8/ofhRheErZwHgY
chMnYTYfjbNrw8VmwzkFUjVEYdUefE+FoPFcX2QwyN7pFG/jAFCpDWZVTErBc8aG
KK1/cDR7fdtMQvkCpG+KSxkCAwEAAQ==
-----END PUBLIC KEY-----
```

在申请页面 `Account Email` 里填入申请人邮箱，在 `Account Public Key` 填入上面产生的公钥（需要包含公钥起始标识符 `-----BEGIN PUBLIC KEY-----` 和结束标识符 `-----END PUBLIC KEY-----` ）。

点击 `Validate Account Info` ，看到 `Looks good! Proceed to Step 2!` 既可进行下一步操作。

## 2 提交证书签发申请

运行以下代码来生成域名私钥 `domain.key` ：

```Bash
$ openssl genrsa 4096 > domain.key
```

再运行以下代码来生成证书签发申请文件 `domain.csr` ：

```Bash
$ openssl req -new -key domain.key -out domain.csr
```

此时系统会询问一些问题，作为个人申请，除了 `Common Name` 表示需要申请签发证书的域名，需要准确填写，其他大部分问题可以简单回答：

```Bash
Country Name (2 letter code) [AU]:
#请输入2位数的国家代码

State or Province Name (full name) [Some-State]:
#请输入州或省的全名

Locality Name (eg, city) []:
#请输入城市名称

Organization Name (eg, company) [Internet Widgits Pty Ltd]:
#请输入组织名称

Organizational Unit Name (eg, section) []:
#请输入部门名称

Common Name (e.g. server FQDN or YOUR name) []:
#请输入需要申请签发证书的域名，请准确填写

Email Address []:
#请输入申请人邮箱地址

A challenge password []:
#请输入申请文件密码，这个一般可以忽略

An optional company name []:
#请输入一个可选的公司名称
```

回答了所有问题之后即可获得证书签发申请文件 `domain.csr` ，打开该文件，复制其中的内容到申请页面的 `Certificate Signing Request` 框中（需要包含起始标识符 `-----BEGIN CERTIFICATE REQUEST-----` 和结束标识符 `-----END CERTIFICATE REQUEST-----` ）。

点击 `Validate CSR` ，看到 `Found domains! Proceed to Step 3! (www.domain.com)` ，确认一下括号里面的域名就是我们申请的域名，既可进入下一步。

## 3 为 API 请求声明

这一步的算法比较复杂，幸运的是，申请系统已经帮我们生成了需要执行的代码，将 `Run these signature commands in your terminal` 下面提供的三条命令在我们的电脑或者服务器上运行，然后将输出的结果粘贴到对应的框里面去既可。

点击 `Validate Signatures` ，看到 `Step 3 complete! Please proceed to Step 4.` 的提示既可进入下一步。

## 4 验证域名所有权

再次确认页面上 **加粗** 显示的域名没有错误，然后在电脑或者服务器上运行下面给出的命令，将结果粘贴到页面上。

接下来就是验证服务器，页面上提供了两种办法，运行 Python 脚本或者放置验证文件，二者的作用都是确保访问指定的服务器路径可以返回验证代码。

### 4.1 Python 脚本

复制页面上的脚本，并在服务器上运行。注意如果之前服务器上有其他程序占用了 80 端口，要先把该程序暂停，否则会因为端口冲突而无法启动。

### 4.2 放置文件

根据提示在 `http://www.domain.com/.well-known/acme-challenge` 目录下面建立一个文件（没有后缀名），再将验证码放入其中既可。

### 4.3 提交验证

注意提交验证只有一次机会，一旦验证失败需要从第一部开始重新操作，因此建议在提交验证之前自己访问一次地址，确保可以看到正确的验证码。

点击这一步的 `(how do I do this?)` ，里面有提供一个验证地址，在新窗口中访问这个地址，能看到正确的验证码既可。

点击 `I'm now running this command on www.dimain.com` 或 `I'm now serving this file on nomo.iltcapp.net` ，看到 `Domain verified!` 既可进入最后一步。

### 4.4 停止 Python 脚本

如果是通过 Python 脚本完成验证的，在这里可以按 `Ctrl + C` 来停止 Python 脚本。

## 5 安装证书

申请页面第5步的框中会提供已签发的域名证书和中间证书。将这两个证书复制到同一个文件里（域名证书在前，中间证书在后），将文件保存为 `domain.crt` ，连同 `domain.key` 一起安装到服务器上既可。

## 6 后记

给好几个域名都签发过证书以后，我才发现 [Let's Encrypt](https://letsencrypt.org/) 签发的证书有效期只有3个月。网上面的说法是这家证书签发商仍在试运营阶段，正式运营之后会适当延长证书有效期。