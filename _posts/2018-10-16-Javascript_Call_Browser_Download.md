---
layout: post
title: "js调用浏览器下载"
date:   2018-10-11 +0800
toc: true
category: js
tags: 文件下载 javascript
excerpt: 本篇文章主要讲解js调用浏览器下载。
---
#### download.js[官网地址](http://danml.com/download.html)
#### 在做文件下载的时候需要使用js调用浏览器的下载，在编写后发现浏览器不兼容，然后找到了一个dowmload.js，可以兼容大多数浏览器。
#### download.js的引入要在jquery.js之前，否者会报错无法使用
#### 引入download.js后只需要在需要使用下载的地方调用download("文件地址","想要起的名字","文件类型");
#### domo如下
{% highlight html %}
<html>
<head>
</head>
<body>
    <div  id="oDownLoad" onclick="download('http://localhost:8080/file/348.pdf','113331.pdf','application/pdf')">下载</div>
</body>
</html>
{% endhighlight html %}