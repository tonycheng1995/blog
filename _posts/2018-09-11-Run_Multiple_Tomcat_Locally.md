---
layout: post
title: "本地运行多台Tomcat"
date:   2018-09-11 +0800
toc: true
category: tomcat
tags: Tomcat
excerpt: 本篇文章主要讲解如何配置以使多台Tomcat在同一电脑上运行。
---
#### 因为项目需求，需要在本地安装两个Tomcat，下面将介绍如何在本地安装两个及多个服务器。
#### 第一个Tomcat我是通过安装完成的。
#### 第二个Tomcat从网站下载压缩包，并解压到要安装的位置，重命名为Tomcat2。
#### 修改Tomcat2中的某些参数，以避免启动Tomcat时出现冲突，编辑bin/startup.bat文件。
#### 将文件中的参数修改为如下
{% highlight bash %}
set "CATALINA_HOME=D:\Java\jdk1.8.0_92"(JDK安装路径)
set "CATALINA_HOME=D:\Tomcat 2"(Tomcat安装路径)
{% endhighlight bash %}

#### 修改conf/server.xml文件，需要修改三处。
{% highlight xml %}
<Server port="8006" shutdown="SHUTDOWN">
<Connector port="9100" protocol="HTTP/1.1"
        connectionTimeout="20000"
        redirectPort="8443" />
<Connector port="8010" protocol="AJP/1.3" redirectPort="8443" />
{% endhighlight xml %}

#### 至此，已修改完毕，可以运行测试一下是否成功。
