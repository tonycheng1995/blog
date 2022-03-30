---
layout: post
title: "post请求字符乱码"
date: 2017-02-17 13:29:58 +0800
toc: true
category: Java
tags: Java 字符乱码
excerpt: 在工作中难免遇到乱码的问题，这里我们就讲一下如何解决这种问题。
---
#### 问题描述：提交请求获取目标网页的内容，但是目标网页是gb2312的，以致获取到的字符变成了乱码

#### 如何解决这种问题呢？

#### Java的java.lang包下的String类为我们提供了解决方法。通过使用指定的charset解码指定的byte数组，构造一个新的String。新String的长度是字符集的函数，因此可能不等于byte数组的长度。
{% highlight java %}
public String(byte[] bytes,Charset charset)
{% endhighlight java %}

#### 所以我们可以将原本的代码改为如下的代码
{% highlight java %}
byte[] b=response.body().bytes();
String responseData=new String(b,"gb2312");
{% endhighlight java %}