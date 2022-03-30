---
layout: post
title: "高德地图创建应用获取SHA1"
date: 2017-06-01 +0800
toc: true
category: android 
tags: Android 高德地图
excerpt: 本篇文件讲解在开发高德地图时如何获取SHA1
---
#### 1.获取发布版SHA1
#### 获取发布版SHA1，我们需要用到jks文件
    keytool -list -v -keystore E:\androidspace\5210blog.jks

![](/img/keystore02.png)
#### 2.获取测试版SHA1
    keytool -v -list -keystore debug.keystore


![](/img/keystore03.png)
#### 3.创建应用
![](/img/keystore04.png)
