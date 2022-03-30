---
layout: post
title: "PHPadmin修改密码"
date: 2017-03-20 19:10:34 +0800
toc: true
category: PHP
tags: PHP
excerpt: PHPAdmin的密码修改，和密码忘记后重置
---

#### 1.登录PHPadmin，选择账户，点击‘root localhost’的‘修改权限’
![]({{site.url}}/img/PHPadmin01.png)
#### 2.点击修改密码，填写密码，点击‘执行’
![]({{site.url}}/img/PHPadmin02.png)
#### 3.刷新页面可以看到我们已经无法链接数据库了，我们需要在phpAdmin/config.inc.php中添加上刚才的密码
![]({{site.url}}/img/PHPadmin03.png)
#### 4.OK，至此我们就算修改完毕了

#### 附：PHPadmin忘记密码
#### /xampp/mysql/resetroot.bat  运行此文件root密码重置为空白
