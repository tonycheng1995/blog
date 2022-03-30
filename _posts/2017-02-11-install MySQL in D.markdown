---
layout: post
title:  "将MySQL安装到D盘"
date:   2017-02-11 21:53:58 +0800
toc: true
category: MySQL
tags: MySQL 数据库
excerpt: 对于“强迫症”的人来说，喜欢将东西分类处理，比如C是系统盘，软件全部安装的D盘。 然而最近新下的MySQL on Windows却不再征询我的意愿直接将软件安装在了C盘下，不能忍。怎么办呢？我们只能下载安装MySQL压缩包了，沮丧~
---
#### 对于“强迫症”的人来说，喜欢将东西分类处理，比如C是系统盘，软件全部安装的D盘。 然而最近新下的[MySQL on Windows](https://dev.mysql.com/downloads/windows/)却不再征询我的意愿直接将软件安装在了C盘下，不能忍。怎么办呢？我们只能下载安装[MySQL压缩包](https://dev.mysql.com/downloads/mysql/)了，沮丧~

#### 1.将MySQL解压到D盘

#### 2.初始化data文件
{% highlight bash %}
mysqld --initialize-insecure
{% endhighlight bash %}

#### 3.管理员身份运行命令行，运行如下命令安装
{% highlight bash %}
D:\MySQL\bin>mysqld install
Service successfully installed.
{% endhighlight bash %}

#### 4.启动MySQL服务
{% highlight bash %}
D:\MySQL\bin>net start mysql
MySQL 服务正在启动 .
MySQL 服务已经启动成功。
{% endhighlight bash %}

#### 5.配置环境变量

#### 变量：MYSQL_HOME，值：D:\MySQL，Path增加`%MYSQL_HOME%\bin`

#### 6.登陆MySQ并修改密码为root
{% highlight bash %}
D:\MySQL\bin>mysql -u root -p
Enter password:
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 3
Server version: 5.7.17 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> set password for root@localhost= password('root');
Query OK, 0 rows affected, 1 warning (0.00 sec)
{% endhighlight bash %}