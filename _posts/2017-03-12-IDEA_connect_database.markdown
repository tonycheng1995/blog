---
layout: post
title: "IDEA连接数据库"
date: 2017-03-12 12:11:34 +0800
toc: true
category: IDEA
tags: IDEA
excerpt: 编写项目我们难免要用到数据库，在我们用IDEA的时候是否可以直接用IDEA来连接编辑呢？答案肯定是可以的。但具体应该怎么做呢？那么看了这篇文章我想你一定就会了。
---
#### 1.引入数据库连接jar包
#### 你可以从官网下载并导入或者直接在maven配置文件里配置
#### 2.打开数据库视图
#### View>Tools Windows>Database
![]({{site.url}}/img/database01.png)
#### 3.新建数据库源
![]({{site.url}}/img/database02.png)
#### 4.配置数据库连接信息
#### 点击此图标![]({{site.url}}/img/database03.png) 进入数据信息配置界面
#### 填写数据库连接用户名，密码
![]({{site.url}}/img/database04.png)
#### 填写完毕后可以点击‘Test Connection’测试一下，如果显示‘Successful Details’则表明填写的信息都正确，点击‘OK’
![]({{site.url}}/img/database05.png)
#### 5.新建SQL文件，并编写相应的SQl语句
#### 当然你也可以：右键点击数据库>New>Table 建立数据库表
![]({{site.url}}/img/database06.png)
#### 6.右键>Run 'xxx.sql',运行SQl文件
#### 7.相应的点击数据库>表你就可以看到生成的数据了
![]({{site.url}}/img/database07.png)

