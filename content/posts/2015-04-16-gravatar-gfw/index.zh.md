---
title: '解决 Gravatar 被墙问题'
date: 2015-04-15
tags:
- Gravatar
- Gfw
---

突然发现我在国内的一个博客无法加载头像了，测试了一番，发现是 WordPress 使用的 Gravatar 服务被墙了。

上网搜索了一番，找到几个比较好的解决办法，贴在这里分享一下～

我并不打算切换回本地头像，所以主要是考虑如何疏通 Gravatar 服务。

<!--more-->

WordPress 默认的 Gravatar 地址是 ```www.gravatar.com```、```0.gravatar.com```、```1.gravatar.com``` 和 ```2.gravatar.com```，但是他们都被屏蔽了，通过测试发现，更换使用其他域名可以解决问题。

## 方法一、使用 Gravatar 其他域名

在 [http://en.gravatar.com/site/translations/](http://en.gravatar.com/site/translations/) （需要翻墙）上列举了 Gravatar 各语言子站的地址，通过依次尝试，发现类似 [http://cn.gravatar.com/](http://cn.gravatar.com/) 这样的域名没有被屏蔽。于是我们可以将 WordPress 默认的 Gravatar 地址替换成可以访问的。在当前模板的 functions.php 中添加以下代码，替换默认的 Gravatar 地址：

```PHP
//替换头像地址
function unblock_gravatar( $avatar ) {
    $avatar = str_replace( array( 'www.gravatar.com', '0.gravatar.com', '1.gravatar.com', '2.gravatar.com' ), 'cn.gravatar.com', $avatar );
    return $avatar;
}

add_filter( 'get_avatar', 'unblock_gravatar' );
```

## 方法二、使用多说的镜像地址

社会化评论系统多说也使用了 Gravatar 头像，可能是早就发现官方地址的不稳定，多说专门建立了一个 Gravatar 的镜像服务，地址是 [http://gravatar.duoshuo.com](http://gravatar.duoshuo.com) ，据说访问速度很快。使用多说镜像的话上面的代码需要改成：

```PHP
//替换头像地址
function unblock_gravatar( $avatar ) {
    $avatar = str_replace( array( 'www.gravatar.com', '0.gravatar.com', '1.gravatar.com', '2.gravatar.com' ), 'gravatar.duoshuo.com', $avatar );
    return $avatar;
}

add_filter( 'get_avatar', 'unblock_gravatar' );
```

本文参考：

[http://c7sky.com/unblock-gravatar.html](http://c7sky.com/unblock-gravatar.html)