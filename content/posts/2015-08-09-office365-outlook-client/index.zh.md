---
title: 'Office 365 Outlook 客户端配置'
date: 2015-08-09
tags:
- Office
- Outlook
- 客户端
---

前几天拿到了学校配的邮箱，打算通过客户端 Thunderbirds 来收发邮件，但是配置收发参数的时候一直碰到问题，在这里记录一下。

<!--more-->

## 1. 收发服务器地址

根据 Office 邮件服务的不同，收发服务器地址各不相同。

### 1.1 Office 365 Outlook

如果登录 Outlook 之后，浏览器中的域名是 ```https://outlook.office365.com``` ，则说明使用的是 Office 365 Outlook 服务，收发服务器地址统一为：

协 议 |         地 址          | 端口号（加密）
---- | ---------------------- | ---
POP3 | outlook.office365.com  | 995
IMAP | outlook.office365.com  | 993
SMTP |   smtp.office365.com   | 857

### 1.2 Outlook Web App

如果登录 Outlook 之后，浏览器中的域名是 ```https://mail.yourdomain.com``` 或其他地址，比如 ```https://mail.xdf.cn``` ，则说明使用的是 Outlook Web App 服务，需要自行查找收发服务器地址。

查找方法为：在 Outlook Web App 中的工具栏上，单击“设置”（齿轮图标） > “选项” > “帐户” > “我的帐户” > “POP 和 IMAP 访问设置”，既可查看收发服务器地址。

## 2. 邮箱登录名

跟传统的邮件服务不同，Office 邮件服务的登录名和邮件地址可能是不一样的，我在这里卡了很长时间。

### 2.1 Office 365 Outlook

如果使用的是 Office 365 Outlook 服务，请先尝试访问 [https://login.microsoftonline.com](https://login.microsoftonline.com) ，并输入完整的邮箱地址（包括@后面的内容），然后点击页面空白处。

如果看到“我们正在引导你进入组织的登录页”，则需要特别小心，邮箱登录名应该是在“组织登录页”上填写的用户名（可能和邮箱不一致）+“@邮箱后缀”。例如“组织登录页”填写的登录名是 ```username``` ，邮箱后缀是 ```xdf.cn``` ，则 Office 365 Outlook 服务的登录名为 ```username@xdf.cn``` （这可能不是一个存在的邮箱地址，但这是一个正确的登录名）。

如果什么也没有看到，可以正常的输入密码并成功登录，则登录名即为完整的邮箱地址（包括@后面的内容）。

### 2.2 Outlook Web App

如果使用的是 Outlook Web App 服务，登录名即为完整的邮箱地址（包括@后面的内容）。

本文参考：
[https://support.office.com/zh-cn/article/1AC34FA0-C5BE-46F1-9C28-8622D92D766E](https://support.office.com/zh-cn/article/1AC34FA0-C5BE-46F1-9C28-8622D92D766E)