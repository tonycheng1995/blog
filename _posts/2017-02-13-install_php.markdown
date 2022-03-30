---
layout: post
title: "PHP开发环境安装"
date: 2017-02-13 22:16:58 +0800
toc: true
category: PHP
tags: PHP 编程语言 后端
excerpt: 工欲善其事，必先利其器。在开始学习PHP之前，我们先来安装一下PHP的开发环境。
---
#### 这里我们安装的工具为XAMPP+PhpStorm

## 安装XAMPP

#### 1.进入[XAMPP中文官网](https://www.apachefriends.org/zh_cn/index.html)，下载相应平台下的安装包，这里我们选择了XAMPP for Windows

#### 2.点击安装，Next→选择组件`Apache`、`MySQL`、`PHP`、`phpMyAdmin`,Next→安装文件夹，默认就行→Next→Next→Finish。完成后如下
![]({{site.url}}/img/xampp.png)

#### 3.点击`start`,点击`Admin`,看到如下图说明安装运行成功
![]({{site.url}}/img/xampp2.png)

## 安装phpStorm

#### 进入[phpStorm官网](http://www.jetbrains.com/phpstorm/)，并下载安装。Nextf→更改安装路径，Next→Next→Finish。

#### 开始第一个PHP程序
![]({{site.url}}/img/php_test.png)

#### 在浏览器输入http://localhost/Test/ ,即可看到页面效果
![]({{site.url}}/img/hello_php.png)

## 更改web根目录

#### 点击Appache的配置文件httpd.config,找到并更改DocumentRoot和Directory
{% highlight bash %}
DocumentRoot "E:\PWorkspace"
<Directory "E:\PWorkspace">
{% endhighlight bash %}

#### 修改phpStorm的默认端口63342

#### ![]({{site.url}}/img/port.png)
