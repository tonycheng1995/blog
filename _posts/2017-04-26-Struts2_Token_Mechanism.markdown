---
layout: post
title: "Struts2令牌机制"
date:   2017-04-26 +0800
category: Struts2
toc: true
tags: SSH Struts2
excerpt: 本篇文章主要讲解Struts2令牌机制防止表单重复提交
---
#### 解决重复提交表单问题，利用token拦截器实现
#### ·令牌生成流程
#### 浏览器发出请求，服务器检查界面上是否有token标签。如果有，向session中添加一个struts.token属性(如果已经有则覆盖)，来保存token id，并将jsp页面的token转换成token id回发到浏览器,在jsp页面也生成一个token id，这两个值是相同的唯一的值，通过验证这两个值是否相同来判断是否重复提交
#### ·令牌验证流程
#### 浏览器触发action发出请求，服务器根据struts2配置文件判断是否拦截该方法，拦截该方法后，判断提交的token Id和session中保存的token id是否相等，如果相等，则清空session中的token id，如果不同，根据配置跳转到指定页面
#### ·实现令牌验证的步骤
#### 1.jsp页面加入标签支持
{% highlight jsp %}
<%@ taglib prefix="s" uri="/struts-tags" %>
{% endhighlight jsp %}
#### 2.表单中加入\<s:token/>
#### 3.struts.xml中需要验证重复提交的action中加入验证拦截器
{% highlight xml %}
<interceptor-ref name="token" />
<result name="invalid.token">error.jsp</result>
{% endhighlight xml %}

#### index.jsp
{% highlight jsp %}
<body>
<s:form action="loginAction.action" method="post">
	<s:token></s:token>
	<s:textfield name="username" key="login.username"/>
	<s:textfield name="password" key="login.password"/>
	<s:submit key="login.submit"/>
</s:form>
</body>
{% endhighlight jsp %}

#### error.jsp
{% highlight jsp %}
<body>
	表单已经提交过了
</body>
{% endhighlight jsp %}

#### LoginAction.java
{% highlight java %}
package com.cheng.action;
import com.opensymphony.xwork2.ActionSupport;
public class LoginAction extends ActionSupport {
	@Override
	public String execute() throws Exception {
		System.out.println("进入action中！");
		return SUCCESS;
	}
}
{% endhighlight java %}

#### struts.xml
{% highlight xml %}
<struts>
	<package name="test" extends="struts-default">
		<action name="loginAction" class="com.cheng.action.LoginAction">
			<interceptor-ref name="token"></interceptor-ref>
			<interceptor-ref name="defaultStack"></interceptor-ref>
			<result name="success">/index.jsp</result>
			<result name="invalid.token">/error.jsp</result>
		</action>
	</package>
</struts>
{% endhighlight java %}

#### 在连续提交或者回退到之前重复提交时，便会触发拦截跳转到error页面
