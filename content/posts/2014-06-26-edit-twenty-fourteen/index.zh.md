---
title: '对 Twenty Fourteen 的一些修改'
date: 2014-06-26
tags:
- WordPress
- CSS
- 主题
aliases:
- /article/wordpress/edit-twenty-fourteen.html
---

Twenty Fourteen 刚出来的时候，我只是点开看了一下——印象最深的整个页面右边有一块空白，比较奇怪——就关掉了，之后一直在研究自己制作 WordPress 主题。但是后来发现自己能力不足，于是决定先放一放，继续使用 WordPress 自带的主题，于是重新开始研究并尝试对 Twenty Fourteen 这个主题做一些修改。

建议大家通过 WordPress 的“[子主题](http://codex.wordpress.org/zh-cn:%E5%AD%90%E4%B8%BB%E9%A2%98)”功能来修改主题，这样做的好处是原来的主题如果有更新，可以毫不犹豫的去更新，而不用担心自己修改的内容丢失。有关“子主题”的使用方法，可以参考官方的[说明文档](http://codex.wordpress.org/zh-cn:%E5%AD%90%E4%B8%BB%E9%A2%98)，我以后也会特别写文章来讨论。

## 右侧空白

首先要解决的问题就是整个网站右侧的空白，在网上看到有人说这是因为 Twenty Fourteen  的 CSS 中设置了 .site 与 .site-header 属性

```css
max-width: 1260px;
```

这即意味着网页最大只能显示1260px的宽度，这显然低于一些高分辨率屏幕的尺寸，修改的办法就是在 style.css 末尾添加下面的代码，覆盖掉原来的样式：

```css
.site,
.site-header {
	max-width: 100%;
}
```

## 内容宽度

Twenty Fourteen  中留给中间正文内容的宽度比较窄，主要原因是还要为右侧边栏留出空间，但像我这样不使用用侧边栏的博客，正文缩在页面中间看起来会很奇怪，可以尝试将其加宽，网上流传的做法是：

```css
.site-content .entry-header,
.site-content .entry-content,
.site-content .entry-summary,
.site-content .entry-meta,
.page-content {
        max-width: 90%; // put the width you like here
}
```

但实际使用时发现这样还不完善，页面标签会跑偏，下方的评论框没有变宽，于是我做了更进一步的修改：

```css
.site-content .entry-header,
.site-content .entry-content,
.site-content .entry-summary,
.site-content .entry-meta,
.site-content .navigation,
.comments-area,
.page-header,
.page-content {
	max-width: 70%; // put the width you like here
}

.site-content header .entry-meta {
	max-width: 100%;
}
```

## 去除标题中的英文全部大写

Twenty Fourteen  中默认标题和标签等全部英文字母大写，有些时候看起来会不习惯，可以把它们去掉：

```css
.cat-links,
.entry-meta,
.entry-title,
.entry-meta .tag-links a,
.comments-title,
.comment-reply-title,
.widget .widget-title,
.widget_calendar caption {
	text-transform: none;
}
```

这里同时去掉了文章标题、文章标签、页面标题、小工具标题的全部大写功能，可以按需求设置

本文参考：

http://sjyf.org/2014/01/04/%E5%AF%B9wordpress%E6%96%B0%E4%B8%BB%E9%A2%98%E7%9A%84%E5%BE%AE%E8%B0%83%E6%95%B4/

http://tieba.baidu.com/p/2783383568
