---
title: 'UTF-8 和 BOM'
date: 2014-02-28
tags:
- BOM
- UTF-8
- PHP
- Windows Server
---

今天在工作室的一台服务器（Windows Server 2008）上修改一个 .php 的配置文件，因为没有经验直接拿记事本打开编辑并保存了，结果再打开网站就看到错误提示：

```
错误 330 (net::ERR_CONTENT_DECODING_FAILED)：未知错误
```

<!--more-->

虽然这个网站访问量不大，但出现这个错误还是把我吓出一身汗。随后在网上搜索错误原因，大多数都是在讨论“ GZIP 跟 PHP 文件或者网页发生的冲突”，但是里面的解决办法并不适合这个网站。几番查找之后，终于发现了原因：由 UTF-8 的 BOM 头导致。

> BOM——Byte Order Mark，就是字节序标记
>
> 在UCS 编码中有一个叫做”ZERO WIDTH NO-BREAK SPACE”的字符，它的编码是FEFF。而FFFE在UCS中是不存在的字符，所以不应该出现在实际传输中。UCS规范建议我们在传输字节流前，先传输 字符”ZERO WIDTH NO-BREAK SPACE”。这样如果接收者收到FEFF，就表明这个字节流是Big-Endian的；如果收到FFFE，就表明这个字节流是Little- Endian的。因此字符”ZERO WIDTH NO-BREAK SPACE”又被称作BOM。
>
> UTF-8不需要BOM来表明字节顺序，但可以用BOM来表明编码方式。字符”ZERO WIDTH NO-BREAK SPACE”的UTF-8编码是EF BB BF。所以如果接收者收到以EF BB BF开头的字节流，就知道这是UTF-8编码了。
>
> UTF- 8编码的文件中，BOM占三个字节。如果用记事本把一个文本文件另存为UTF-8编码方式的话，用UE打开这个文件，切换到十六进制编辑状态就可以看到开 头的FFFE了。这是个标识UTF-8编码文件的好办法，软件通过BOM来识别这个文件是否是UTF-8编码，很多软件还要求读入的文件必须带BOM。可 是，还是有很多软件不能识别BOM。

找到了原因，解决方法也就有了：
将文件复制到本地，用 Dreamweaver 或其他类似工具打开（本文以 Dreamweaver 为例）：
选择 文件->另存为 ，在“保存类型”下方有一个“Unicode 选项…”，点击打开：

{{< figure src="images/2014022801.png" class="figure-center" >}}

{{< figure src="images/2014022802.png" class="figure-center" >}}

将“包括 Unicode 签名（BOM）”前面的钩去掉，重新保存文件即可。

这次事件给了我两个教训：
1. 不要使用记事本来编辑代码文件文件，使用 Notepad++ 等高级工具来编辑；
2. 编辑代码前要先备份

引用的文字来自：
[http://afericazebra.blog.163.com/blog/static/30050408201211199298711/](http://afericazebra.blog.163.com/blog/static/30050408201211199298711/)