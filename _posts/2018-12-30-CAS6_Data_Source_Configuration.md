---
layout: post
title: "CAS6.0单点登录之数据源配置"
date:   2018-12-30 +0800
toc: true
category: CAS
tags: CAS SSO 单点登录
excerpt: 本篇文章主要讲解如何配置CAS单点登录服务器的数据源，本篇文章以是在之前的cas6.0的基础上搭建的。
---
#### 参考官方说明https://apereo.github.io/cas/5.0.x/installation/Database-Authentication.html#configuration
## 1.引入依赖
#### 在/maven-overlay/pom.xml中引入以下依赖
{% highlight xml %}
<!-- 用来启用数据库身份验证 -->
<dependency>
    <groupId>org.apereo.cas</groupId>
    <artifactId>cas-server-support-jdbc</artifactId>
    <version>${casVersion}</version>
</dependency>
<!-- 引入JDBC Drivers -->
<dependency>
    <groupId>org.apereo.cas</groupId>
    <artifactId>cas-server-support-jdbc-drivers</artifactId>
    <version>${casVersion}</version>
</dependency>
<!-- 引入mysql -->
<dependency>
<groupId>mysql</groupId>
<artifactId>mysql-connector-java</artifactId>
<version>${mysqlDriverVersion}</version>
    </dependency>
{% endhighlight xml %}
## 2.配置版本号
#### 在build.properties中配置版本号
{% highlight properties %}
casVersion=6.0.0-RC1
springBootVersion=2.0.4.RELEASE
mysqlDriverVersion=8.0.13
{% endhighlight properties %}
	
## 3.打包
{% highlight bash %}
[root@VM_152_123_centos cas]# ./build.sh
{% endhighlight bash %}
	  
## 4.创建数据库表
![]({{site.url}}/img/cas13.png)

## 5.修改配置application.properties
#### 如下注释掉cas.authn.accept.users=casuser::Mellon，并修改根据自己实际情况修改数据库登录信息，修改完毕之后重启tomcat,输入数据库的用户和密码即可登录成功。
{% highlight bash %}
[root@VM_152_123_centos local]# vim tomcat/webapps/cas/WEB-INF/classes/application.properties

#cas.authn.accept.users=casuser::Mellon
cas.authn.jdbc.query[0].url=jdbc:mysql://localhost:3306/sso?useUnicode=true&characterEncoding=UTF-8&autoReconnect=true&useSSL=false
cas.authn.jdbc.query[0].user=root                                          
cas.authn.jdbc.query[0].password=Root_123
cas.authn.jdbc.query[0].sql=select * from ssoUser where ssoAccount=?
cas.authn.jdbc.query[0].fieldPassword=ssoPassword               
cas.authn.jdbc.query[0].driverClass=com.mysql.jdbc.Driver
{% endhighlight bash %}
    
