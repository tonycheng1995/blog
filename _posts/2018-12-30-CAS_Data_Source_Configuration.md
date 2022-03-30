---
layout: post
title: "CAS单点登录之数据源配置"
date:   2018-12-30 +0800
toc: true
category: CAS
tags: CAS SSO 单点登录
excerpt: 本篇文章主要讲解如何配置CAS单点登录服务器的数据源，本篇文章以是在之前的cas4.0的基础上搭建的。
---
## 1.数据源配置
#### CAS搭建完成之后只能以默认的固定账户登录，在实际开发中我们需要配置CAS的数据源，以使其能够根据我们数据库中的账户进行校验登录。
### 1.1修改cas服务端中WEB-INF下的deployerConfigContext.xml文件,添加如下配置
{% highlight xml %}
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource"  
        p:driverClass="com.mysql.jdbc.Driver"  
        p:jdbcUrl="jdbc:mysql://127.0.0.1:3306/caslogin?characterEncoding=utf8"  
        p:user="root"  
        p:password="root" />
<bean id="passwordEncoder"
        class="org.jasig.cas.authentication.handler.DefaultPasswordEncoder"  
        c:encodingAlgorithm="MD5"  
        p:characterEncoding="UTF-8" />  
<bean id="dbAuthHandler"  
        class="org.jasig.cas.adaptors.jdbc.QueryDatabaseAuthenticationHandler"  
        p:dataSource-ref="dataSource"  
        p:sql="select password from user where account = ?"  
        p:passwordEncoder-ref="passwordEncoder"/>
{% endhighlight xml %}
        
### 1.2注释固定密码登录
{% highlight xml %}
<bean id="authenticationManager" class="org.jasig.cas.authentication.PolicyBasedAuthenticationManager">
        <constructor-arg>
        <map>
                <entry key-ref="proxyAuthenticationHandler" value-ref="proxyPrincipalResolver" />
                <!-- 使用固定的用户名和密码 -->
                <!--<entry key-ref="primaryAuthenticationHandler" value-ref="primaryPrincipalResolver" />-->
        </map>
        </constructor-arg>
        <property name="authenticationPolicy">
        <bean class="org.jasig.cas.authentication.AnyAuthenticationPolicy" />
        </property>
</bean>
{% endhighlight xml %}

### 1.3添加如下配置
{% highlight xml %}
<entry key-ref="dbAuthHandler" value-ref="primaryPrincipalResolver"/>
{% endhighlight xml %}
### 1.4添加依赖
#### 在webapp\cas\WEB-INF\lib文件下添加三个jar包：c3p0-x.x.x.x.jar，cas-server-support-jdbc-x.x.x.jar,mysql-connector-java-x.x.x.jar。
### 1.5测试
#### 用数据库的用户名和密码进行测试是否配置成功。
#### 注:如果出现程序运行出错，很有可能是jar包与数据库等版本不一致造成。
## 2.服务端登录认证页修改
#### 在实际开发中我们可能需要根据我们自己的需求来修改登录页的展示，以使其与我们的项目一致。
#### 将自己的登录页拷贝到cas/WEB-INF/view/jsp/default/ui目录下，并重命名为casLoginView.jsp，将css、js等文件拷贝到cas目录下
### 2.1在casLoginView.jsp页面添加指令
{% highlight jsp %}
<%@ page pageEncoding="UTF-8" %>
<%@ page contentType="text/html; charset=UTF-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="spring" uri="http://www.springframework.org/tags" %>
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>
{% endhighlight jsp %}

### 2.2修改form表单中的标签
{% highlight html %}
<form:form method="post" id="fm1" commandName="${commandName}" htmlEscape="true" class="sui-form">
        <form:input id="username" tabindex="1"
        accesskey="${userNameAccessKey}" path="username" autocomplete="off" htmlEscape="true"
        placeholder="邮箱/用户名/手机号" class="span2 input-xfat" />
        <form:password  id="password" tabindex="2" path="password"
                accesskey="${passwordAccessKey}" htmlEscape="true" autocomplete="off"
                placeholder="请输入密码" class="span2 input-xfat"   />
        <input type="hidden" name="lt" value="${loginTicket}" />
        <input type="hidden" name="execution" value="${flowExecutionKey}" />
        <input type="hidden" name="_eventId" value="submit" />
        <input class="sui-btn btn-block btn-xlarge btn-danger" accesskey="l" value="登陆" type="submit" />
</form:form>
{% endhighlight html %}

### 2.3在表单内添加错误提示
{% highlight html %}
<form:errors path="*" id="msg" cssClass="errors" element="div" htmlEscape="false" />
{% endhighlight html %}

### 2.4国际化
#### 修改cas-servlet.xml，设置国际化为zn_CN
{% highlight xml %}
<bean id="localeResolver" class="org.springframework.web.servlet.i18n.CookieLocaleResolver" p:defaultLocale="zh_CN" />
{% endhighlight xml %}
        
### 2.5设置登录失败信息
#### 在WEB-INF/classes/messages_zh_CN.properties中添加以下信息
{% highlight properties %}
authenticationFailure.AccountNotFoundException=\u7528\u6237\u4E0D\u5B58\u5728.
authenticationFailure.FailedLoginException=\u5BC6\u7801\u9519\u8BEF.
{% endhighlight properties %}

