---
layout: post
title: "Struts2中Web资源的获取"
date:   2017-04-25 +0800
toc: true
category: Struts2
tags: SSH Struts2
excerpt: 本篇文章主要讲解Struts2中Web资源的获取
---
## 1.使用拦截器获取web资源
#### 1.1使用Struts2 Aware拦截器
#### FirstAction.java
{% highlight java %}
package com.cheng.action;
import javax.servlet.ServletContext;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.apache.struts2.interceptor.ServletRequestAware;
import org.apache.struts2.interceptor.ServletResponseAware;
import org.apache.struts2.util.ServletContextAware;
import com.opensymphony.xwork2.ActionSupport;
//实现拦截器接口
public class FirstAction extends ActionSupport implements ServletRequestAware,ServletResponseAware,ServletContextAware{

	private ServletRequest request;
	private ServletResponse response;
	private ServletContext context;

	@Override
	public String execute() throws Exception {
		String username=request.getParameter("username");
		System.out.println(username);
		return "success";
	}

	@Override
	//当请求进入到action之前，程序自动通过拦截器调用setServletRequest(),
	//将HttpServletRequest传递过来
	public void setServletRequest(HttpServletRequest arg0) {
		request=arg0;

	}

	@Override
	public void setServletResponse(HttpServletResponse arg0) {
		response=arg0;
	}

	@Override
	public void setServletContext(ServletContext arg0) {
		context=arg0;
	}

}
{% endhighlight java %}

#### struts.xml
{% highlight xml %}
<struts>
	<package name="test" extends="struts-default">
		<action name="firstAction" class="com.cheng.action.FirstAction">
			<result name="success">/index.jsp</result>
		</action>
	</package>
</struts>
{% endhighlight xml %}

#### index.jsp
{% highlight jsp %}
<form action="firstAction.action" method="post">
	username:<input type="text" name="username"><br>
	password:<input type="password" name="password"><br>
	<input type="submit" value="提交">
</form>
{% endhighlight jsp %}

#### 1.2使用Struts2 RequestAware拦截器
#### SecondAction.java
{% highlight java %}
package com.cheng.action;
import java.util.Map;
import javax.servlet.ServletContext;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import org.apache.struts2.StrutsStatics;
import org.apache.struts2.interceptor.RequestAware;
import com.opensymphony.xwork2.ActionSupport;
//使用RequestAware拦截器
public class SecondAction extends ActionSupport implements RequestAware{

	private ServletRequest request;
	private ServletResponse response;
	private ServletContext context;

	@Override
	public String execute() throws Exception {
		String username=request.getParameter("username");
		System.out.println(username);
		return "success";
	}

	@Override
	public void setRequest(Map<String, Object> arg0) {
		request=(ServletRequest) arg0.get(StrutsStatics.HTTP_REQUEST);
		response=(ServletResponse) arg0.get(StrutsStatics.HTTP_RESPONSE);
		context=(ServletContext) arg0.get(StrutsStatics.SERVLET_CONTEXT);
	}

}
{% endhighlight java %}

#### struts.xml
{% highlight xml %}
<struts>
	<package name="test" extends="struts-default">
		<action name="secondAction" class="com.cheng.action.FirstAction">
			<result name="success">/index.jsp</result>
		</action>
	</package>
</struts>
{% endhighlight xml %}

#### index.jsp
{% highlight jsp %}
<body>
<form action="secondAction.action" method="post">
	username:<input type="text" name="username"><br>
	password:<input type="password" name="password"><br>
	<input type="submit" value="提交">
</form>
</body>
{% endhighlight jsp %}

## 2.静态对象获取web资源
#### 2.1使用Struts2内置静态对象ActionContext
#### Thirdction.java
{% highlight java %}
package com.cheng.action;
import javax.servlet.ServletContext;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import org.apache.struts2.StrutsStatics;
import com.opensymphony.xwork2.ActionContext;
import com.opensymphony.xwork2.ActionSupport;
public class ThirdAction extends ActionSupport {

	@Override
	public String execute() throws Exception {
		ActionContext ac=ActionContext.getContext();
		ServletRequest request=(ServletRequest) ac.get(StrutsStatics.HTTP_REQUEST);
		ServletResponse response=(ServletResponse) ac.get(StrutsStatics.HTTP_RESPONSE);
		ServletContext context=(ServletContext) ac.get(StrutsStatics.SERVLET_CONTEXT);

		String username=request.getParameter("username");
		System.out.println(username);
		return "success";
	}
}
{% endhighlight java %}

#### struts.xml
{% highlight xml %}
<struts>
	<package name="test" extends="struts-default">
		<action name="thirdAction" class="com.cheng.action.ThirdAction">
			<result name="success">/index.jsp</result>
		</action>
	</package>
</struts>
{% endhighlight xml %}

#### index.jsp
{% highlight jsp %}
<body>
<form action="thirdAction.action" method="post">
	username:<input type="text" name="username"><br>
	password:<input type="password" name="password"><br>
	<input type="submit" value="提交">
</form>
</body>
{% endhighlight jsp %}

#### 2.2使用Struts2内置静态对象ServletActionAcontext(推荐使用)
#### FourthAction.java
{% highlight java %}
package com.cheng.action;
import javax.servlet.ServletContext;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import org.apache.struts2.ServletActionContext;
import com.opensymphony.xwork2.ActionSupport;
public class FourthAction extends ActionSupport {

	@Override
	public String execute() throws Exception {

		ServletRequest request=ServletActionContext.getRequest();
		ServletResponse response=ServletActionContext.getResponse();
		ServletContext context=ServletActionContext.getServletContext();

		String username=request.getParameter("username");
		System.out.println(username);
		return super.execute();
	}
}
{% endhighlight java %}

#### struts.xml
{% highlight xml %}
<struts>
	<package name="test" extends="struts-default">
		<action name="fourthAction" class="com.cheng.action.FourthAction">
			<result name="success">/index.jsp</result>
		</action>
	</package>
</struts>
{% endhighlight xml %}

#### index.jsp
{% highlight jsp %}
<body>
<form action="fourthAction.action" method="post">
	username:<input type="text" name="username"><br>
	password:<input type="password" name="password"><br>
	<input type="submit" value="提交">
</form>
</body>
{% endhighlight jsp %}

## 3.登录实例
#### servletContext，也就是application对象，
#### 它是一个服务器对象,只要服务器不关闭，那么信息永远存在。该信息是存储在服务器内存中的，一般数据我们严禁向application对象当中放入,因为容易服务器内存溢出，程序崩溃
#### 驾校考试系统，只需要注册用户就可以免费使用，用户量庞当。每次只出现一个题，然后做完本道题自动跳转到下一题，这个系统就可以用到application。application对象，程序一启动，立即从数据库取出考试题目，放到服务器application对象中，考试的时候就不需要再经过数据库层获取了
#### LoginAction.java
{% highlight java %}
package com.cheng.action;
import javax.servlet.ServletRequest;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpSession;
import org.apache.struts2.ServletActionContext;
import com.cheng.pojo.Users;
import com.opensymphony.xwork2.ActionSupport;
public class LoginAction extends ActionSupport {

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
		if(username.equals("admin")&&password.equals("123")){
			Users us=new Users();
			us.setUsername(username);
			us.setPassword(password);

			//将User信息放到Session中以便在其他地方使用
			ServletRequest request=ServletActionContext.getRequest();
			HttpServletRequest req=(HttpServletRequest) request;
			HttpSession session=req.getSession();
			session.setAttribute("us", us);

			return "success";
		}else{
			return "fail";
		}
	}

}
{% endhighlight java %}

#### struts.xml
{% highlight xml %}
<struts>
	<package name="test" extends="struts-default">
		<action name="loginAction" class="com.cheng.action.LoginAction">
			<result name="success">/success.jsp</result>
			<result name="fail">/index.jsp</result>
		</action>
	</package>
</struts>
{% endhighlight xml %}

#### index.jsp
{% highlight jsp %}
<form action="loginAction.action" method="post">
	username:<input type="text" name="username"><br>
	password:<input type="password" name="password"><br>
	<input type="submit" value="提交">
</form>
</body>
{% endhighlight jsp %}

#### success.jsp
{% highlight jsp %}
<body>
<%Users us=(Users)session.getAttribute("us"); %>
登录成功！欢迎<%=us.getUsername() %>用户登录！
</body>
{% endhighlight jsp %}

#### 4.源代码[下载](http://pan.baidu.com/s/1o7OY3Ku) 密码：6wb4
