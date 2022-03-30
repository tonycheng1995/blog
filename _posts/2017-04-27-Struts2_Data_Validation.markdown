---
layout: post
title: "Struts2数据校验"
date:   2017-04-27 +0800
toc: true
category: Struts2
tags: SSH Struts2
excerpt: 本篇文章主要讲解Struts2数据校验
---
#### ·JavaScript校验局限性
#### 用户可以绕过表单通过直接在URL地址拼接数据传到后台
#### ·Struts2提供了两种较为简易的校验方式：
#### 硬编码方式--易理解、不易维护
#### xml配置方式--易维护、易管理、不侵入源代码(推荐)

## 硬编码方式校验
#### 在jsp中加入struts2校验框架提供的两种校验级别错误
#### 属性级错误：\<s:fielderror cssStyle="color:red"/>
#### Action级错误：\<s:actionerror cssStyle="color:red"/>
#### Action类中创建校验方法
#### 方法命名规则：validate+要验证的方法名(首字母大写)
#### 校验错误信息被默认放入struts2默认的栈队中，Map集合errors(可能只适合2.5之前)
#### UsersAction.java
{% highlight java %}
package com.cheng.action;
import java.util.regex.Pattern;
import com.opensymphony.xwork2.ActionSupport;
public class UsersAction extends ActionSupport {

	private String username;
	private String password;
	private String repassword;
	private String email;
	private String phone;
	private int age;

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

	public String getRepassword() {
		return repassword;
	}

	public void setRepassword(String repassword) {
		this.repassword = repassword;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	public String getPhone() {
		return phone;
	}

	public void setPhone(String phone) {
		this.phone = phone;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}

	@Override
	public String execute() throws Exception {
		return super.execute();
	}

	public void validateExecute(){
		if(null==username||username.length()<6||username.length()>10){
			this.addFieldError("username", "用户名格式有误！");
		}
		if(null==password||password.length()<6||password.length()>10){
			this.addFieldError("password", "密码格式有误！");
		}else if(null==password||password.length()<6||password.length()>10){
			this.addFieldError("repassword", "re密码格式有误！");
		}else if(!password.equals(repassword)){
			this.addFieldError("password", "两次密码不一致！");
		}

		if(age<=0||age>120){
			this.addFieldError("age", "年龄超出范围！");
		}

		Pattern pattern=Pattern.compile("^([a-zA-Z0-9_-])+@([a-zA-Z0-9_-])+(\\.([a-zA-Z0-9_-])+)+$");
		if(null==email||!pattern.matcher(email).matches()){
			this.addFieldError("emali", "邮箱验证失败！");
		}
		Pattern p=Pattern.compile("^(((13[0-9])|(15([0-3]|[5-9]))|(18[0,5-9]))\\d{8})|(0\\d{2}-\\d{7,8})|(0\\d{3}-\\d{7,8})$");
		if(null==phone||!p.matcher(phone).matches()){
			this.addFieldError("phone", "电话号码格式错误！");
			this.addActionError("action级别错误！");
		}

	}
}
{% endhighlight java %}

#### struts.xml
{% highlight xml %}
<struts>
	<package name="test" extends="struts-default">
		<action name="registerAction" class="com.cheng.action.UsersAction">
			<result name="success">/success.jsp</result>
			<result name="input">/index.jsp</result>
		</action>
	</package>
</struts>
{% endhighlight xml %}

#### index.jsp
{% highlight jsp %}
<body>
    <!-- 属性级错误 -->
    <s:fielderror cssStyle="color:red"/>
    <br>
    <!-- Action级错误 -->
    <s:actionerror cssStyle="color:red"/>
    <form action="test/registerAction" method="post">
    用户名：<input type="text" name="username"/><s:fielderror fieldName="username"/><br><br>
    密码：<input type="password" name="password" /><br><br>
    确认密码：<input type="password" name="repassword"/><s:fielderror fieldName="repassword"/><br><br>
    年龄：<input type="text" name="age"/>${errors.age}<br><br><!-- 2.5之后此方法无法使用 -->
    邮箱：<input type="text" name="email"/><s:fielderror fieldName="email" /><br><br>
    电话：<input type="text" name="phone"/><s:fielderror  fieldName="phone"/><br><br>
    <input type="submit" value="提交"/>
    </form>
 </body>
{% endhighlight jsp %}


## xml配置方式校验
#### 在action类同包下创建一个xml配置文件，命名规则：Action名-validation.xml
#### 单属性方式页面错误信息:${errors.username[0]}
#### 对象方式页面错误信息:${errors["user.username"][0]}
#### Users.Java
{% highlight java %}
package com.cheng.bean;
public class Users {
	private String username;
	private String password;
	private String repassword;
	private String email;
	private String phone;
	private int age;
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
	public String getRepassword() {
		return repassword;
	}
	public void setRepassword(String repassword) {
		this.repassword = repassword;
	}
	public String getEmail() {
		return email;
	}
	public void setEmail(String email) {
		this.email = email;
	}
	public String getPhone() {
		return phone;
	}
	public void setPhone(String phone) {
		this.phone = phone;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}

}
{% endhighlight java %}

#### RegisterAction.Java
{% highlight java %}
package com.cheng.action;
import com.cheng.bean.Users;
import com.opensymphony.xwork2.ActionSupport;
public class RegisterAction extends ActionSupport {
	private Users user;

	public Users getUser() {
		return user;
	}

	public void setUser(Users user) {
		this.user = user;
	}
	@Override
	public String execute() throws Exception {

		return SUCCESS;
	}

}
{% endhighlight java %}

#### RegisterAction-validation.xml
{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE validators PUBLIC
	"-//Apache Struts//XWork Validator 1.0.3//EN"
	"http://struts.apache.org/dtds/xwork-validator-1.0.3.dtd">

<validators>
	<field name="user.username">
		<field-validator type="requiredstring">
			<!-- 是否格式化空格 -->
			<param name="trim">true</param>
			<message>用户名不能为空</message>
		</field-validator>
		<!-- 验证用户名只能是字母或数字，长度在6-25之内 -->
		<field-validator type="regex">
			<!-- 表达式 -->
			<param name="expression">
				<![CDATA[(\w{6,25})]]>
			</param>
			<message>用户名格式错误！</message>
		</field-validator>
	</field>

	<field name="user.password">
		<field-validator type="requiredstring">
			<message>密码格式有误</message>
		</field-validator>
		<!-- 验证密码长度在6-25 -->
		<field-validator type="stringlength">
			<param name="minLength">6</param>
			<param name="maxLength">25</param>
			<message>密码长度有误！</message>
		</field-validator>
		<!-- 验证密码是否一致 -->
		<field-validator type="fieldexpression">
			<param name="expression">
				<![CDATA[(user.password==user.repassword)]]>
			</param>
			<message>两次密码不一致</message>
		</field-validator>
	</field>

		<field name="user.age">
		<field-validator type="int">
			<param name="min">0</param>
			<param name="max">120</param>
			<message>年龄范围有误！</message>
		</field-validator>
	</field>

	<field name="user.email">
		<field-validator type="email">
			<message>邮箱格式有误</message>
		</field-validator>
	</field>

	<field name="user.phone">
		<field-validator type="regex">
			<param name="expression">
				<![CDATA[(^(((13[0-9])|(15([0-3]|[5-9]))|(18[0,5-9]))\d{8})|(0\d{2}-\d{7,8})|(0\d{3}-\d{7,8})$)]]>
			</param>
			<message>电话格式有误</message>
		</field-validator>
	</field>
</validators>
{% endhighlight xml %}

#### index.jsp
{% highlight jsp %}
<body>
    <!-- 属性级错误 -->
    <s:fielderror cssStyle="color:red"/>
    <br>
    <!-- Action级错误 -->
    <s:actionerror cssStyle="color:red"/>
    <form action="test/registerAction" method="post">
    用户名：<input type="text" name="user.username"/><br><br>
    密码：<input type="password" name="user.password" /><br><br>
    确认密码：<input type="password" name="user.repassword"/><br><br>
    年龄：<input type="text" name="user.age"/>${errors["user.age"][0]}<br><br><!-- 2.5之后可能不再适用 -->
    邮箱：<input type="text" name="user.email"/><s:fielderror fieldName="user.email" /><br><br>
    电话：<input type="text" name="user.phone"/><br><br>
    <input type="submit" value="提交"/>
    </form>
</body>    
{% endhighlight jsp %}
#### 源码[下载](http://pan.baidu.com/s/1dE9bRGH) 密码：7eiv
