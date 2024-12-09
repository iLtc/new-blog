---
title: 'UTF-8 and BOM'
date: 2014-02-28
tags:
- BOM
- UTF-8
- PHP
- Windows Server
---

Today, I modified a .php configuration file on a server (Windows Server 2008) in the studio. Since I lacked experience, I directly opened and edited it with Notepad, then saved the file. As a result, when I reopened the website, I saw an error message:

```
Error 330 (net::ERR_CONTENT_DECODING_FAILED): Unknown error
```

<!--more-->

Although the website doesn’t have much traffic, this error still scared me. I searched online for the cause of the error, and most discussions were about "GZIP conflicts with PHP files or web pages," but the solutions there weren’t applicable to this website. After some searching, I finally found the cause: it was caused by the UTF-8 BOM (Byte Order Mark).

> BOM—Byte Order Mark, is a byte-order marker.
>
> In UCS encoding, there is a character called “ZERO WIDTH NO-BREAK SPACE,” which has the encoding FEFF. FFFE is a character that doesn't exist in UCS, so it should not appear in actual transmission. The UCS standard suggests that before transmitting a byte stream, we first transmit the character “ZERO WIDTH NO-BREAK SPACE.” This way, if the receiver gets FEFF, it indicates the byte stream is Big-Endian; if it gets FFFE, it indicates the byte stream is Little-Endian. Therefore, the character “ZERO WIDTH NO-BREAK SPACE” is also known as BOM.
>
> UTF-8 does not require BOM to indicate byte order, but it can use BOM to indicate encoding format. The UTF-8 encoding of the “ZERO WIDTH NO-BREAK SPACE” character is EF BB BF. So, if a receiver gets a byte stream starting with EF BB BF, it knows the encoding is UTF-8.
>
> In UTF-8 encoded files, BOM takes up three bytes. If you use Notepad to save a text file in UTF-8 encoding, you can open this file with UE (UltraEdit), switch to hexadecimal mode, and see the starting FEFF. This is a good way to identify UTF-8 encoded files, as software can recognize a file as UTF-8 encoded by its BOM. Many programs even require that files must include BOM to be read correctly. However, there are still many programs that cannot recognize BOM.

Once I found the cause, I also found the solution:

1. Copy the file to your local machine.

2. Open it with Dreamweaver or any similar tool (this example uses Dreamweaver).

3. Select File -> Save As, and under "Save as type," click on "Unicode options..." to open the options window:

{{< figure src="images/2014022801.png" class="figure-center" >}}

{{< figure src="images/2014022802.png" class="figure-center" >}}

4. Uncheck the option “Include Unicode signature (BOM)” and then save the file again.

This incident taught me two lessons:

1. Do not use Notepad to edit code files; use advanced tools like Notepad++.

2. Always back up files before editing them.

Reference:
[http://afericazebra.blog.163.com/blog/static/30050408201211199298711/](http://afericazebra.blog.163.com/blog/static/30050408201211199298711/)