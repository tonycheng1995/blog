---
layout: post
title: "CAS单点登录之服务器搭建"
date:   2018-09-29 +0800
toc: true
category: CAS
tags: CAS SSO 单点登录
excerpt: 本篇文章主要讲解如何搭建CAS单点登录服务器。
---
#### 搭建环境
#### ·JDK8
#### ·Tomcat9
#### ·cas-server4.0
#### ·MySQL5
#### ·c3p0-0.9.1.2.jar
#### ·cas-server-support-jdbc-4.0.0.jar
#### ·mysql-connector-java-5.1.32.jar
#### 注：如果是按照本教程学习搭建的话，最好版本与本教程中的的版本一致，如果版本不一致，很有可能使项目无法运行或达到预期的效果。

## 1.服务端配置
### 1.1.打开命令行，输入以下命令，导出证书
{% highlight bash %}
C:\Windows\system32>keytool -genkey -alias castest -keyalg RSA -keystore E:/casKey.keystore
{% endhighlight bash %}

#### 按下图配置
![]({{site.url}}/img/cas01.png)
### 1.2.输入以下命令，导出crt文件
{% highlight bash %}
C:\Windows\system32>keytool -export -file E:/casKey.crt -alias castest -keystore E:/casKey.keystore
{% endhighlight bash %}

#### 如下图
![]({{site.url}}/img/cas02.png)
### 1.3.导入证书
#### 将生成的证书导入运行的jdk中
{% highlight bash %}
C:\Windows\system32>keytool -import -keystore "D:\Java\jdk1.8.0_92\jre\lib\security\cacerts" -file E:/casKey.crt -alias castest
{% endhighlight bash %}

#### 如下图
![]({{site.url}}/img/cas03.png)
### 1.4.添加域名映射
#### 将生成的证书文件casKey.crt拷贝到JDK/bin下，在C:/Windows/System32/drivers/etc/hosts中添加域名映射，内容如下
{% highlight bash %}
127.0.0.1 sso.test.com
{% endhighlight bash %}

### 1.5.配置Tomcat环境
#### 将cas-server-4.0.0-release/cas-server-4.0.0/modules/cas-server-4.0.0.war复制到Tomcat/webapps下并重命名为cas.war
#### 打开tomcat/server.xml，增加如下内容。keystoreFile为casKey.keystore的路径，keystorePass为前面设置的口令。
{% highlight xml %}
<Connector port="8086" protocol="HTTP/1.1" SSLEnabled="true"
    maxThreads="150" scheme="https" secure="true"
    clientAuth="false" sslProtocol="TLS" keystoreFile="E:/casKey.keystore" keystorePass="cas123" />
{% endhighlight xml %}

### 1.6.启动tomcat
#### 在地址栏输入 https://localhost:8086/cas/login ，出现如下图界面。
![]({{site.url}}/img/cas04.png)
### 1.7.登录
#### 输入默认账号/密码：casuser/Mellon。用户名和密码是cas-server-4.0.0-release/cas-server-4.0.0/cas-server-webapp/src/main/webapp/WEB-INF/deployerConfigContext.xml中的 <entry key="casuser" value="Mellon"/>
![]({{site.url}}/img/cas05.png)
### 1.8.注销登录
#### 访问https://localhost:8086/cas/logout，注销登录
![]({{site.url}}/img/cas06.png)

## 2.客户端配置
### 2.1.maven新建web项目CASClient1。
#### 在pom.xml文件中导入需要的jar包
{% highlight xml %}
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.cas</groupId>
  <artifactId>CASClient1</artifactId>
  <packaging>war</packaging>
  <version>0.0.1-SNAPSHOT</version>
  <name>CASClient1 Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
      <!-- cas -->  
        <dependency>  
            <groupId>org.jasig.cas.client</groupId>  
            <artifactId>cas-client-core</artifactId>  
            <version>3.3.3</version>  
        </dependency>       
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>  
            <scope>provided</scope>
        </dependency>
  </dependencies>
  <build>
    <finalName>CASClient1</finalName>
        <plugins>
          <plugin>  
              <groupId>org.apache.maven.plugins</groupId>  
              <artifactId>maven-compiler-plugin</artifactId>  
              <version>2.3.2</version>  
              <configuration>  
                  <source>1.7</source>  
                  <target>1.7</target>  
              </configuration>  
          </plugin>
          <plugin>
                <groupId>org.apache.tomcat.maven</groupId>
                <artifactId>tomcat7-maven-plugin</artifactId>
                <configuration>
                    <!-- 指定端口 -->
                    <port>9001</port>
                    <!-- 请求路径 -->
                    <path>/</path>
                </configuration>
          </plugin>
      </plugins>  
  </build>
</project>
{% endhighlight xml %}

#### 在web.xml中加入需要的监听
{% highlight xml %}
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns="http://java.sun.com/xml/ns/javaee"
xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
id="WebApp_ID" version="2.5">
  <display-name>Archetype Created Web Application</display-name>
<!-- 用于单点退出,该过滤器用于实现单点登出功能,可以选配 -->
<listener>
  <listener-class>org.jasig.cas.client.session.SingleSignOutHttpSessionListener</listener-class>
</listener>
<!-- 该过滤器用于实现单点登出功能,可选配 -->
<filter>
  <filter-name>CAS Single Sign Out Filter</filter-name>
  <filter-class>org.jasig.cas.client.session.SingleSignOutFilter</filter-class>
</filter>
<filter-mapping>
  <filter-name>CAS Single Sign Out Filter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
<!-- 该过滤器负责用户的认证工作,必须启用 -->
<filter>
  <filter-name>CASFilter</filter-name>
  <filter-class>org.jasig.cas.client.authentication.AuthenticationFilter</filter-class>
  <init-param>
    <param-name>casServerLoginUrl</param-name>
    <param-value>https://sso.test.com:8086/cas/login</param-value>
    <!-- 这里的Server是服务端的IP -->
  </init-param>
  <init-param>
    <param-name>serverName</param-name>
    <param-value>http://localhost:9100</param-value>
  </init-param>
</filter>
<filter-mapping>
  <filter-name>CASFilter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
<!-- 该过滤器负责对Ticket的校验工作,必须启用 -->
<filter>
  <filter-name>CAS Validation Filter</filter-name>
  <filter-class>org.jasig.cas.client.validation.Cas20ProxyReceivingTicketValidationFilter</filter-class>
  <init-param>
    <param-name>casServerUrlPrefix</param-name>
    <param-value>https://sso.test.com:8086/cas</param-value>
  </init-param>
  <init-param>
    <param-name>serverName</param-name>
    <param-value>http://localhost:9100</param-value>
  </init-param>
</filter>
<filter-mapping>
  <filter-name>CAS Validation Filter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
<!-- 该过滤器负责实现HttpServletRequest请求的包裹,比如允许开发者通过 HttpServletRequest的getRemoteUser()方法获得SSO登录用户的用户名，可选配-->
<filter>
  <filter-name>CAS HttpServletRequest Wrapper Filter</filter-name>
  <filter-class>org.jasig.cas.client.util.HttpServletRequestWrapperFilter</filter-class>
</filter>
<filter-mapping>
  <filter-name>CAS HttpServletRequest Wrapper Filter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
<!-- 该过滤器使得开发者通过org.jasig.cas.client.util.AssertionHolder来获取用户的登录名，
  比如AssertionHolder.getAssertion().getPrincipal().getName() -->
<filter>
  <filter-name>CAS Assertion Thread Local Filter</filter-name>
  <filter-class>org.jasig.cas.client.util.AssertionThreadLocalFilter</filter-class>
</filter>
<filter-mapping>
  <filter-name>CAS Assertion Thread Local Filter</filter-name>
  <url-pattern>/*</url-pattern>
</filter-mapping>
</web-app>
{% endhighlight xml %}

#### 编写index.jsp
{% highlight jsp %}
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>小软认证</title>
</head>
<body>
欢迎登录小软二手市场平台
<%=request.getRemoteUser() %>
</body>
</html>
{% endhighlight jsp %}

### 2.2.按照以上步骤新建CASClient2。
#### 运行服务端
#### 将客户端部署到服务器，运行客户端(注意:客户端和服务端分别在两个服务器上)
#### 在地址栏输入http://localhost:9100/CASClient1/ ,页面跳转到服务端登录页，输入用户名/密码，页面进入客户端1欢迎页面。
#### 在地址栏输入http://localhost:9100/CASClient2/ ，页面直接进入客户端2欢迎页面。
## 3.单点退出
#### 修改配置文件使单点退出后跳转到指定页面，修改cas系统的配置文件cas-servlet.xml文件
{% highlight xml %}
<bean id="logoutAction" class="org.jasig.cas.web.flow.LogoutAction"
        p:servicesManager-ref="servicesManager"
        p:followServiceRedirects="${cas.logout.followServiceRedirects:true}"/>
{% endhighlight xml %}

#### 将参数改为true后，可以在退出时跳转到目标页面，修改index.jsp的退出链接
{% highlight jsp %}
<a href="http://localhost:9100/cas/logout?service=http://www.baidu.com">退出登录</a>
{% endhighlight jsp %}

#### 至此，CAS单点登录就搭建完成了。
