---
layout: post
title: "Struts2类型转换器"
date:   2017-04-29 +0800
toc: true
category: Struts2
tags: SSH Struts2
excerpt: 本篇文章主要讲解Struts2的类型转换器
---
## 局部类型转换器
#### 只作用于一个action类
#### 创建自定义类型转换器，继承DefaultTypeConverter类，重写其中的convertValue方法
#### TestAction.java
{% highlight java %}
package com.cheng.action;
import java.util.Date;
import com.opensymphony.xwork2.ActionSupport;
public class TestAction extends ActionSupport {

	private Date times;

	public Date getTimes() {
		return times;
	}

	public void setTimes(Date times) {
		this.times = times;
	}

	@Override
	public String execute() throws Exception {
		System.out.println(times.toLocaleString());
		return "success";
	}
}
{% endhighlight java %}

#### struts.xml
{% highlight xml %}
<struts>
	<package name="test" extends="struts-default">
		<action name="logins" class="com.cheng.action.TestAction">
			<result name="success">/index.jsp</result>
		</action>
	</package>
</struts>
{% endhighlight xml %}

#### DateConver.java
{% highlight java %}
package com.cheng.conver;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Map;
import ognl.DefaultTypeConverter;
public class DateConver extends DefaultTypeConverter {
	//实现类型转换器有两种方式
	//1.继承ognl的DefaultType类或者实现TypeConverter接口
	//2.基于struts2库，实际上struts2库的也是基于ognl的再次封装，简化一定操作
	//区别： ognl只重写一个方法，实现双向的转换
	//		struts2要重写2个方法，一个从字符串转换成某种类型，convertFromString
	//		一个从某类型转换为字符串convertToString

	@Override
	/**
		* 参数(类型转换的上下文，前台传递过来的数据，转换后的目标类型)
		*/
	public Object convertValue(Map context, Object value, Class toType) {
		// struts2基于更安全的考虑，参数以数组的方式接受，方式用户提交的要转换的数据是多个
		Date date=null;
		String []params=(String[])value;
		SimpleDateFormat sdf=new SimpleDateFormat("yyyyMMdd");
		try {
			date=sdf.parse(params[0]);
		} catch (Exception e) {
			e.printStackTrace();
		}
		return date;
	}
}
{% endhighlight java %}


#### action同包下TestAction-conversion.properties
{% highlight properties %}
times=com.cheng.conver.DateConver
{% endhighlight properties %}

#### index.jsp
{% highlight html %}
<body>
<form action="test/logins" method="post">
	请填写日期(格式如20171206)<br>
	日期：<input type="text" name="times"/><br>
	<input type="submit" value="提交"/>
</form>
</body>
{% endhighlight html %}

#### 效果
![]({{site.url}}/img/typeConversion01.png)


## 全局类型转换器
#### 作用于所有action类
#### 创建自定义类型转换器，继承DefaultTypeConverter类，重写其中的convertValue方法
#### 创建全局转换器配置文件

#### index.jsp
{% highlight html %}
<body>
<form action="test/logins" method="post">
	请填写坐标(格式如x,y)<br>
	坐标：<input type="text" name="po"/><br>
	<input type="submit" value="提交"/>
</form>
<s:property value="po"/>
</body>
{% endhighlight html %}

#### Position.java
{% highlight java %}
package com.cheng.bean;
public class Position {
	private float x;
	private float y;
	public float getX() {
		return x;
	}
	public void setX(float x) {
		this.x = x;
	}
	public float getY() {
		return y;
	}
	public void setY(float y) {
		this.y = y;
	}

}
{% endhighlight java %}
#### TestAction.java
{% highlight java %}
package com.cheng.action;
import org.apache.struts2.ServletActionContext;
import com.cheng.bean.Position;
import com.opensymphony.xwork2.ActionSupport;
public class TestAction extends ActionSupport{

	Position po;

	public Position getPo() {
		return po;
	}

	public void setPo(Position po) {
		this.po = po;
	}

	@Override
	public String execute() throws Exception {
		System.out.println("x坐标:"+po.getX()+" y坐标:"+po.getY());
		ServletActionContext.getRequest().setAttribute("po", po);
		return SUCCESS;
	}
}
{% endhighlight java %}

#### struts.xml
{% highlight xml %}
<struts>
		<package name="test" extends="struts-default">
			<action name="logins" class="com.cheng.action.TestAction">
				<result name="success">/index.jsp</result>
			</action>
		</package>
</struts>
{% endhighlight xml %}

#### PositionConver.java
{% highlight java %}
package com.cheng.conver;
import java.util.Map;
import com.cheng.bean.Position;
import ognl.DefaultTypeConverter;
public class PositionConver extends DefaultTypeConverter {

	@Override
	public Object convertValue(Map context, Object value, Class toType) {
		//前台传递数据到后台
		if(toType==Position.class){
			Position po=new Position();
			String[] str=(String [])value;
			String zuobiao=str[0];
			String xy[]=zuobiao.split(",");
			po.setX(Float.valueOf(xy[0]));
			po.setY(Float.valueOf(xy[1]));
			return po;
		}
		//后台传递到前台
		if(toType==String.class){
			Position po=(Position)value;
			String poo=po.getX()+","+po.getY();
			return poo;
		}

		return null;
	}

}
{% endhighlight java %}

#### 根目录下创建xwork-conversion.properties
{% highlight properties %}
com.cheng.bean.Position=com.cheng.conver.PositionConver
{% endhighlight properties %}

#### 效果
![]({{site.url}}/img/typeConversion02.png)
