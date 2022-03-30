---
layout: post
title: "Tomcat虚拟路径配置"
date:   2018-10-11 +0800
toc: true
category: tomcat
tags: 文件下载 TOMCAT
excerpt: 本篇文章主要讲解下载tomcat项目外的文件的虚拟路径配置。
---
#### 在文件的上传下载中,有时候文件过多等因素，我们需要将文件存储在非项目的目录下，例如项目路径“D:/tomcat/webapps/project”，文件存储路径“E:/upload”,此时需要配置虚拟路径映射来解决这个问题，否则在浏览器上是无法直接访问“E:/upload”的。
#### 在tomcat/conf/server.xml中的<Host></Host>中间添加如下配置
{% highlight xml %}
<Context path="/file" docBase="E:/upload" debug="0" reloadable="true" />
{% endhighlight xml %}

#### 配置完成后可以在浏览器中访问http://127.0.0.1/project/file/xxx.pdf访问到文件
#### 如果是在Eclipse中调试项目，则需要配置Servers中的server.xml文件。
