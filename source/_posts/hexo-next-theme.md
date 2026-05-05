---
title: hexo主题安装以及next8.0主题美化
date: 2020-09-07 20:00:00
categories: 技术
tags:
  - 杂谈
  - Hexo
permalink: hexo_2/
---

<blockquote class="blockquote-center">可以通过右边的文章目录快速跳转哦！</blockquote>

## 前言

配置完了hexo，你是否对自带的主题不满意呢？本篇文章将教你如何更改及美化hexo主题。如果你还有没有安装hexo，请移步我的[上一篇文章](https://haomingzhang.com/hexo_1/)。

<!-- more -->

## hexo主题推荐

### hexo next

[hexo-theme-next](https://github.com/next-theme/hexo-theme-next)应该是目前最广泛使用的hexo主题了，优点是简洁，定制度高，因为代码是开源的，所以有很多开发者维护。

![next themes](https://user-images.githubusercontent.com/16272760/83972923-98baae80-a915-11ea-8142-3cf875dad8bf.png)

### hexo kaze

[hexo-theme-kaze](https://github.com/theme-kaze/hexo-theme-kaze)是我在[Themes|Hexo](https://hexo.io/themes/)里面找到的一款很漂亮的主题，支持黑、白两种模式，界面简洁。

## hexo next主题安装及美化

### 更改基本站点信息

在你的博客根目录下打开 _config.yml，在site栏目下可以更改你的基本站点信息：

```yml
title: Vince's Blog
subtitle: ''
description: 'Vince的博客'
keywords:
author: Vince
language: zh-CN
timezone: ''
```

同时，记得把网站url改成自己的站点名：

```yml
url: http://www.haomingzhang.com
```

### 安装hexo next主题

如果你正在使用hexo 5.0及以上版本，可以使用以下命令安装：

```sh
$ cd your_site_dir
$ npm install hexo-theme-next
```

或者使用git submodule克隆到本地：

```sh
$ cd your_site_dir
$ git submodule add https://github.com/next-theme/hexo-theme-next themes/next
```

### 切换到next主题

在你的博客根目录下打开 _config.yml，搜索themes，将里面的值改为next：

```yml
theme: next
```

### 配置next主题

将 themes/next/_config.yml 复制到你的博客根目录，重命名为 _config.next.yml：

```sh
$ cd your_site_dir
$ cp themes/next/_config.yml _config.next.yml
```

#### 选择Schemes

打开 _config.next.yml，首先可以看到 Scheme Settings，里面提供了四种模式，我用的是muse主题：

```yml
# Schemes
scheme: Muse
#scheme: Mist
#scheme: Pisces
#scheme: Gemini
```

#### 设置站点icon

在favicon中，可以设置侧边栏头像以及站点icon。需要把你的icon放在 source/img/ 目录下：

```yml
favicon:
    small: /img/avatar.jfif
    medium: /img/avatar.jfif
    apple_touch_icon: /img/avatar.jfif
    safari_pinned_tab: /images/logo.svg
```

#### 设置菜单栏

在menu下，可以设置菜单显示内容，此版本next支持font awesome图标：

```yml
menu:
    home: / || fa fa-home
    tags: /tags/ || fa fa-tags
    categories: /categories/ || fa fa-th
    archives: /archives/ || fa fa-archive
    guestbook: /guestbook/ || fa fa-book
```

#### 开启tags/categories/留言板栏

举个栗子，开启tags：

```sh
$ cd your_site_dir
$ hexo new page tags
```

此时在你的 source/ 文件夹下会出现一个对应的tags文件夹，打开index.md并编辑为：

```yml
---
title: 标签
date: 2020-09-06 19:36:01
type: tags
comments: false
sitemap: false
---
```

即可开启tags，categories也是一样的操作，type改为categories即可。对于留言板来说，只需要将comments一栏改为true，type改为guestbook即可。

#### 边栏设置

在sidebar选项下，有多个自定义选项：

```yml
#调整边栏左右
position: right

# Sidebar Display (only for Muse | Mist)
display: post

# Sidebar padding in pixels.
padding: 18
# Sidebar offset from top menubar in pixels (only for Pisces | Gemini).
offset: 12
```

#### 添加社交帐号

在social栏目下，可以自定义在边栏显示的社交帐号。

#### 设置回到开头

将back2top的enable设置为true即可：

```yml
back2top:
    enable: true
    sidebar: false
    scrollpercent: true
```

#### 设置已读进度条

将reading_progress的enable设置为true即可：

```yml
reading_progress:
    enable: true
    position: bottom
    color: "#37c6c0"
    height: 3px
```

## 小结

到此，你已经实现了基本的next安装及配置，是不是好看了很多呢？

## Upcoming Next

下一篇文章中，我会介绍一些我使用的插件。（[已更新](https://haomingzhang.com/hexo_3/)）

## Reference

- [我的页面设置](https://github.com/haomingvince/hexo-haomingvince/)
