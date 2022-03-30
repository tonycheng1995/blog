---
layout: post
title: "Android开发中遇到的一些错误"
date:   2017-02-18 19:45:58 +0800
toc: true
category: Android
tags: Android 编程语言
excerpt: 本篇文章主要是总结一下我在Android开发中遇到的一些错误。

---
#### 错误1：尝试在空对象上调用虚拟方法“void android.widget.LinearLayout.addView(android.view.View)”
{% highlight bash %}
Caused by: java.lang.NullPointerException: Attempt to invoke virtual method 'void android.widget.LinearLayout.addView(android.view.View)' on a null object reference
{% endhighlight bash %}

#### 解决思路：

#### 查看一下上级的组件findViewById()内的值是否正确

#### 错误2：
{% highlight bash %}
android.view.ViewRootImpl$CalledFromWrongThreadException: Only the original thread that created a view hierarchy can touch its views.
{% endhighlight bash %}