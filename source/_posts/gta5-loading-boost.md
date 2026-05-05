---
title: GTA5在线模式加载速度加速(大幅提升)
date: 2020-09-21 13:00:00
categories: 游戏
tags:
  - GTA5
permalink: gta_boost/
---

<blockquote class="blockquote-center">可以通过右边的文章目录快速跳转哦！</blockquote>

## 前言

GTA5在线模式的加载速度一直为人诟病，最近在网上看到国外的大神(tostercx)反工程了GTA5的代码，发现在加载线上模式时调用了很多重复的if-else导致加载速度极慢。本文总结了该大神的加速方法。本文只是总结收纳，所有权还是归大神所有！

**注意：此方法会使用到DLL注入，不保证R*不会封禁，Use it under your own risk!**

<!-- more -->

## 具体注入方法

### 1. 下载我的总结包

链接：https://pan.baidu.com/s/1hGblePtpb9cOQJs153rOmg

提取码：j0zd

> 注意：每次启动游戏都要经过以下的步骤进入线上模式！

### 2. 打开Xenos

文件夹中有两个版本，根据自己的系统来选择，64位选择Xenos64.exe，32位选择Xenos.exe。如果你不知道你的系统是64还是32位，可以百度查询（现在一般都是64位）。

### 3. 打开GTA5

此时我们不选择进入GTA Online，先进入线下模式。等待游戏加载进入主菜单后，进行下一步操作。

![GTA5](https://cdn.jsdelivr.net/gh/haomingvince/ghcdn@master/GTAO_Boost/1.png)

### 4. 在Xenos中操作

- 点击Process，找到GTA5.exe
- 在下方点击add，找到总结包中的 Universal.GTAO_Booster.dll
- 点击Inject

![Xenos](https://cdn.jsdelivr.net/gh/haomingvince/ghcdn@master/GTAO_Boost/3.png)

### 5. 点击进入GTA Online

Enjoy! 本人实测加载速度大幅提升！！

## 小结

感谢大神的无私奉献。本文只是总结，有任何问题欢迎去大神的github issue提出~

## Reference

- [GTAO_Booster_PoC](https://github.com/tostercx/GTAO_Booster_PoC)
- [GTAO_Booster_PoC_Release](https://github.com/QuickNET-Tech/GTAO_Booster_PoC/releases/tag/v1.0.1)
- [Tutorial](https://github.com/tostercx/GTAO_Booster_PoC/issues/21)
