---
title: hexo + next8.0开启Valine评论系统及添加留言板功能
date: 2020-09-13 13:34:29
categories: 技术
tags:
  - 杂谈
  - Hexo
permalink: hexo_4/
---

<blockquote class="blockquote-center">可以通过右边的文章目录快速跳转哦！</blockquote>

## 前言

经过前三轮的折腾，你已经拥有一个自己定义的静态页面了，是时候给它添加一个留言板功能了。next主题自带了多种留言功能的插件，比如畅言，disqus，disqusjs，gitalk，livere，valine。 valine是其中相对简洁并且限制较少的一款轻量型博客评论系统。本文将教你如何开启并美化valine评论系统。如果你还没有配置你自己的博客，可以参考我的[前几篇文章](https://haomingzhang.com/tags/Hexo/)。

<!-- more -->

## 开启valine评论系统

### 1. 注册 Leancloud 账号

在leancloud官网注册一个账号，海外同学推荐使用leancloud国际版，国内同学可以使用华东或华北节点。

![leancloud注册](https://cdn.jsdelivr.net/gh/haomingvince/ghcdn@master/hexo_pic/14.png)

注册完成后进入app控制面板 -> 创建应用 -> 创建开发版应用。

![创建应用](https://cdn.jsdelivr.net/gh/haomingvince/ghcdn@master/hexo_pic/15.png)

接下来我们配置一下我们的leancloud。点击设置 -> 安全中心 -> Web 安全域名，输入你的域名来保证其他人就算获取了你的appid也没办法操作你的数据库。

![安全域名](https://cdn.jsdelivr.net/gh/haomingvince/ghcdn@master/hexo_pic/16.png)

接下来点击应用keys获取你的appid和appkey。

![app keys](https://cdn.jsdelivr.net/gh/haomingvince/ghcdn@master/hexo_pic/17.png)

### 2. 在next主题中配置valine

打开 _config.next.yml，找到comments栏目并开启valine：

```yml
comments:
  active: valine
```

往下滑动进入valine设置栏，开启valine并填入你的appid和appkey：

```yml
valine:
  enable: true
  appId:  # Your leancloud application appid
  appKey:  # Your leancloud application appkey
  placeholder: "输入你的评论\n昵称为必填项目，输入QQ号可以直接获取用户名和头像！\n填写了email可以收到推送通知哦！"
  avatar: ""
  meta: [nick, mail]
  pageSize: 10
  lang:
  visitor: true
  comment_count: true
  recordIP: true
  serverURLs:
  enableQQ: true
  requiredFields: [nick]
```

### 3. 在菜单栏添加留言板

添加一个新的page：

```sh
$ hexo new page guestbook
```

进入 source/guestbook/index.md，加入你想显示的内容。图标支持font awesome，可以按自己喜好修改。

### 4. 重新生成页面即可

```sh
$ hexo clean
$ hexo g -d
```

## 小结

valine比较简洁、配置简单且免费，非常推荐作为个人博客的评论系统来使用。

## Reference

- [我的页面设置](https://github.com/haomingvince/hexo-haomingvince/)
