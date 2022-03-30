---
layout: post
title: "Struts2模型驱动"
date:   2017-04-25 +0800
toc: true
category: Struts2
tags: SSH Struts2
excerpt: 本篇文章主要讲解Struts2模型驱动
---
## 1.属性驱动
#### struts2属性驱动和模型驱动帮助我们完成了数据自动获取，数据自动封装
#### 表单中的name值要与action中定义的属性值一致
#### struts.xml
{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts PUBLIC
		"-//Apache Software Foundation//DTD Struts Configuration 2.5//EN"
		"http://struts.apache.org/dtds/struts-2.5.dtd">
<struts>
		<package name="userlogin" extends="struts-default">
			<action name="loginAction" class="com.cheng.action.LoginAction">
				<result name="success">/index.jsp</result>
			</action>
		</package>
</struts>
{% endhighlight xml %}

#### LoginAction.action
{% highlight java %}
package com.cheng.action;
import com.opensymphony.xwork2.ActionSupport;
public class LoginAction extends ActionSupport{

	private String username;
	private String password;

	public String getUsername() {
		return username;
	}

	public void setUsername(String username) {
		this.username = username;
	}

	public String getPassword() {
		return password;
	}

	public void setPassword(String password) {
		this.password = password;
	}

	@Override
	public String execute() throws Exception {
		System.out.println(username+password);
		return "success";
	}

}
{% endhighlight java %}

#### index.jsp
{% highlight jsp %}
<body>
<form action="loginAction.action" method="post">
	username:<input type="text" name="username"><br>
	password:<input type="password" name="password"><br>
	<input type="submit" value="提交">
</form>
</body>
{% endhighlight jsp %}

## 2.模型驱动
#### action中需要实现ModelDriven接口，接口中的泛型为我们要封装的实体类
#### 当请求发送到action之前，调用MlogionAction类中的getModel()获取要将表单数据封装到哪个实例化的对象中。获得到该对象之后，我们可以获得类类型。获得类类型后，获得类中属性。request.getParameters获得表单提交的所有数据名。从而获取值。如果表单提交的name值与实体类中属性名一致，那么我们将获得表单中的数据封装到us对象当中去
#### Users.java
{% highlight java %}
package com.cheng.pojo;
public class Users {

	private String username;
	private String password;
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}

}
{% endhighlight java %}

#### MloginAction.java
{% highlight java %}
package com.cheng.action;
import com.cheng.pojo.Users;
import com.opensymphony.xwork2.ActionSupport;
import com.opensymphony.xwork2.ModelDriven;
public class MloginAction extends ActionSupport implements ModelDriven<Users>{

	private Users us=new Users();//前台提交的数据自动封装到对象中

	@Override
	public String execute() throws Exception {
		System.out.println(us.getUsername());
		System.out.println(us.getPassword());
		return "success";
	}

	@Override
	public Users getModel() {
		return us;
	}

}
{% endhighlight java %}

#### struts.xml
{% highlight xml %}
<struts>
	<package name="userlogin" extends="struts-default">
		<action name="muserlogin" class="com.cheng.action.MloginAction">
			<result name="success">/index.jsp</result>
			<!-- 需要加入两个拦截器：模型封装拦截器，默认的拦截器 -->
			<interceptor-ref name="modelDriven"></interceptor-ref>
			<interceptor-ref name="defaultStack"></interceptor-ref>
		</action>			
	</package>
</struts>
{% endhighlight xml %}

#### index.jsp
{% highlight jsp %}
<body>
    <form action="muserlogin.action" method="post">
    	username:<input type="text" name="username"><br>
    	password:<input type="password" name="password"><br>
    	<input type="submit" value="提交">
    </form>
</body>
{% endhighlight jsp %}

## 3.标签实现模型驱动
#### Users.java
{% highlight java %}
package com.cheng.pojo;
public class Users {

	private String username;
	private String password;
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}

}
{% endhighlight java %}

#### SloginAction.java
{% highlight java %}
package com.cheng.action;
import com.cheng.pojo.Users;
import com.opensymphony.xwork2.ActionSupport;
public class SloginAction extends ActionSupport{

	private Users us;

	public Users getUs() {
		return us;
	}

	public void setUs(Users us) {
		this.us = us;
	}

	@Override
	public String execute() throws Exception {
		System.out.println(us.getUsername());
		System.out.println(us.getPassword());
		return "success";
	}

}
{% endhighlight java %}

#### struts.xml
{% highlight xml %}
<struts>
	<!-- 使表单样式不被struts标签控制，从而手动控制-->
	<constant name="struts.ui.theme" value="simple"/>
	<constant name="struts.ui.templateDir" value="template"/>
	<constant name="struts.ui.templateSuffix" value="ftl"/>

	<package name="userlogin" extends="struts-default">
		<action name="suserlogin" class="com.cheng.action.SloginAction">
			<result name="success">/index.jsp</result>
		</action>		
	</package>
</struts>
{% endhighlight xml %}

#### index.jsp
{% highlight jsp %}
<body>
<s:form action="suserlogin" method="post" namespace="">
	<!-- us.username action中定义的对象.实体属性 -->
	username:<s:textfield name="us.username"/><br>
	password:<s:password name="us.password"/><br>
	<s:submit value="提交" />
</s:form>
</body>
{% endhighlight xml %}

#### 4.源代码[下载](http://pan.baidu.com/s/1slDDmUT) 密码：yw3e
