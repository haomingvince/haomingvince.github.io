---
title: 为hexo next8.0主题添加一个可以切换的黑色/夜间模式
date: 2020-09-08 10:00:00
categories: 技术
tags:
  - 杂谈
  - Hexo
permalink: hexo_3/
---

## 前言

虽然next 8.0 主题已经支持原生黑色模式，只需要在 _config.next.yml 文件中，将相应开关打开即可：

```yml
darkmode: true
```

但是这个黑色模式是不可以切换的，本文将介绍如何实现一个按钮来切换黑/白模式。如果你还没有自己的博客，可以参考我的前两篇文章：[hexo基本安装及配置](https://haomingzhang.com/hexo_1/)，[hexo主题安装以及next8.0主题美化](https://haomingzhang.com/hexo_2/)。

<!-- more -->

## 具体步骤

### 1. 添加darkmode.js到你的next主题中

你可以下载 [Darkmode.js](https://github.com/sandoche/Darkmode.js) 或者直接添加他的cdn到你的vendors文件。

我的方法是直接在 themes/next/_vendors.yml 中添加 darkmode的 cdn：

```yml
darkmode_js:
  name: darkmode-js
  version: 1.5.7
  file: lib/darkmode-js.min.js
```

直接加在文件末尾即可。

### 2. 在你的 _config.next.yml 添加darkmode_js的相关开关：

```yml
vendors:
  darkmode_js:

# darkmode.js
darkmode_js:
  enable: true
```

注意缩进！第一个darkmode_js是在vendors栏目下，第二个darkmode_js是一个单独的栏目。

### 3. 开启darkmode_js

打开 themes/next/layout/_scripts/vendors.njk，配置以下代码：

```njk
{%- if theme.canvas_ribbon.enable %}
  <script src="{{ theme.vendors.canvas_ribbon }}"></script>
{%- endif %}

{# Customize darkmode.js - Declaration #}
{%- if theme.darkmode_js.enable %}
  <script src="{{ theme.vendors.darkmode_js }}"></script>
{%- endif %}

{%- for name in js_vendors() %}
  <script src="{{ url_for(theme.vendors[name]) }}"></script>
{%- endfor %}

{# Customize darkmode.js - Invokation #}
{%- if theme.darkmode_js.enable %}
<script>
var options = {
  bottom: '64px',
  right: 'unset',
  left: '32px',
  time: '0.5s',
  mixColor: '#fff',
  backgroundColor: '#fff',
  buttonColorDark: '#100f2c',
  buttonColorLight: '#fff',
  saveInCookies: true,
  label: '🌓',
  autoMatchOsTheme: true
}
const darkmode = new Darkmode(options);
darkmode.showWidget();
</script>
{%- endif %}
```

因为此版本的动画特效已经精简为只有canvas_ribbon，更改后的配置文件就如上所示。

### 4. 重新生成即可开启

```sh
$ hexo clean
$ hexo g -d
```

## 小结

因为next团队在8.0版本更改了比较多的代码，网上既有的教程并不适用于8.0版本，如果有这个按钮需要的话，可以按照本文配置哦！

最后效果：

![darkmode效果](https://cdn.jsdelivr.net/gh/haomingvince/ghcdn@master/hexo_pic/darkmode.gif)

## Reference

- [我的页面设置](https://github.com/haomingvince/hexo-haomingvince/)
- [sandoche/Darkmode.js](https://github.com/sandoche/Darkmode.js)
- [Hexo（Next 主题）实现可切换的 Dark Mode](https://dog.wtf/tech/hexo-dark-mode-note/)
