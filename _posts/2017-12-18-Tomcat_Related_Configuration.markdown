---
layout: post
title:  "Tomcat相关配置"
date:   2017-12-18  +0800
toc: true
category: tomcat
tags: Tomcat
excerpt: Tomcat使用中的一些相关配置与操作
---

## 1.终端启动
#### 终端进入bin目录，catalina.bat run

## 2.修改访问端口
#### conf/server.xml
{% highlight xml %}
<Connector port="8080" protocol="HTTP/1.1"
        connectionTimeout="20000"
        redirectPort="8443" />
{% endhighlight xml %}
#### 重启服务器

## 3.修改部署路径
#### 将D盘下所有文件夹变成单独应用，D盘hello文件夹变成应用，访问上下文h1
#### conf/server.xml
{% highlight xml %}
<Host name="localhost"  appBase="d:\app"
    unpackWARs="true" autoDeploy="true">
<Context path="/h1" reloadable="true" docBase="D:\hello"></Context>
{% endhighlight xml %}
#### 重启服务器

## 4.修改JVM运行参数
#### 修改bin/catalinaa.bat在set "CURRENT_DIR=%cd%"下行添加如下语句
{% highlight bash %}
set JAVA_OPTS=-server -Xms1024m -Xmx1024m -Xss512m  -XX:PermSize=128m -XX:MaxPermSize=256m" -Djava.awt.handless=true
{% endhighlight bash %}

#### -server: tomcat运行在server模式
#### -Xms：java?Heap初始大小
#### -Xmx：java?heap最大值
#### -XX:MaxNewSize:新生成的池的最大大小
#### -Xss：每个线程的Stack大小
#### -XX:MaxPermSize: 设定内存的永久保存区最大大小
#### -Djava.awt.headless=true: headless模式

## 5.创建用户
#### 修改conf/tomcat-users.xml
{% highlight xml %}
<role rolename="tomcat"/>  角色
<role rolename="role1"/>
<user username="tomcat" password="<must-be-changed>" roles="tomcat"/>   用户
<user username="both" password="<must-be-changed>" roles="tomcat,role1"/>
<user username="role1" password="<must-be-changed>" roles="role1"/>
{% endhighlight xml %}
#### 重启服务器

## 6.提高并发能力
#### 修改conf/server.xml
{% highlight xml %}
<Connector port="8080" protocol="HTTP/1.1"
    maxHttpHeaderSize="8192"     http请求头的最大程度，超过此长度的部分不予处理，一般8K
    maxThreads="100"	           最多同时处理的连接数，Tomcat使用线程来处理接受的每个请求
    minSpareThreads="20"         最小空闲线程数，Tomcat初始化时创建的线程数
    maxSpareThreads="40"         最大空闲线程数，创建的线程超过这个值，Tomcat就会关闭不再需要的socket线程
    acceptCount="500"	           当所有可使用的处理请求的线程数都被使用时，可放到处理队列中的请求数
    maxIdleTime="60000"	         超过最小空闲线程数，多的线程会等待这个时间长度，然后关闭
    connectionTimeout="20000"    网络连接超时，毫秒
    redirectPort="8443" />
{% endhighlight xml %}
#### 重启服务器

## 7.Jconsole监控Tomcat
#### Jconsole所在路径jdk/bin/jconsole.exe
![](/img/jconsole01.jpg)
![](/img/jconsole02.jpg)
## 8.jvisualvm监控Tomcat
#### jvisualvm路径jdk/bin/jvisualvm.exe
![](/img/jvisualvm01.jpg)
