---
layout: post
title: "Java生成验证码"
date:   2018-08-30 +0800
toc: true
category: CAPTCHA
tags: 验证码 Kaptcha
excerpt: 本篇文章主要讲解Java中使用Servlet、Jcaptcha和Kaptcha生成验证码。
---
## 1.使用Servlet生成验证码
#### 1.1.建立web项目
#### 1.2.编写前端页面index.jsp
{% highlight jsp %}
<%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
<%
String path=request.getContextPath();
String basePath=request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort();
%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
<script type="text/javascript">
function reloadCode() {
        //IE缓存认为加载一个图片没有必要，不清楚是旧请求还是新请求
        var time=new Date().getTime();
        document.getElementById("imagecode").src="<%=request.getContextPath()%>/servlet/ImageServlet?d="+time;
}
</script>
</head>
<body>
<form action="<%=request.getContextPath()%>/servlet/LoginServlet" method="get">
验证码：<input type="text" name="checkcode"/>
<img alt="验证码" id="imagecode" src="<%=request.getContextPath()%>/servlet/ImageServlet"/>
<a href="javascript:reloadCode();">看不清楚</a><br>
<input type="submit" value="提交"/>
</form>
</body>
</html>
{% endhighlight jsp %}

#### 1.3.编写验证码生成类com/ImageServlet.java
{% highlight java %}
package com;
import java.awt.Color;
import java.awt.Graphics;
import java.awt.image.BufferedImage;
import java.io.IOException;
import java.util.Random;
import javax.imageio.ImageIO;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
public class ImageServlet extends HttpServlet{
        @Override
        protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
                //图像数据缓存区
                BufferedImage bi=new BufferedImage(68, 22, BufferedImage.TYPE_INT_RGB);
                //绘制图片
                Graphics g=bi.getGraphics();
                //获取颜色
                Color c=new Color(200,150,255);
                g.setColor(c);
                g.fillRect(0, 0, 68, 22);
                char[] ch="QAZWSXEDCRFVTGBYHNUJMIKOLP1234567890".toCharArray();
                Random r=new Random();
                int len=ch.length,index;
                StringBuffer sb=new StringBuffer();
                for (int i = 0; i < 4; i++) {
                        index=r.nextInt(len);
                        g.setColor(new Color(r.nextInt(88),r.nextInt(188),r.nextInt(255)));
                        g.drawString(ch[index]+"", (i*15)+3, 18);
                        sb.append(ch[index]);
                }
                req.getSession().setAttribute("piccode", sb.toString());
                //输出图片
                ImageIO.write(bi, "JPG", resp.getOutputStream());
        }
}
{% endhighlight java %}

#### 1.4.编写验证类com/LoginServlet.java
{% highlight java %}
package com;
import java.io.IOException;
import java.io.PrintWriter;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
public class LoginServlet extends HttpServlet{
        @Override
        protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
                String piccode=(String) req.getSession().getAttribute("piccode");
                resp.setContentType("text/html;charset=utf8");
                String checkcode=req.getParameter("checkcode");
                checkcode=checkcode.toUpperCase();
                PrintWriter out=resp.getWriter();
                if(checkcode.equals(piccode)) {
                        out.println("验证码输入正确！");
                }else {
                        out.println("验证码输入错误！！！！");
                }
                out.flush();
                out.close();
        }
}
{% endhighlight java %}

#### 1.5.编写配置文件web.xml
{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd" id="WebApp_ID" version="3.0">
        <display-name></display-name>    
        <!-- 配置默认首页 -->
        <welcome-file-list>
        <welcome-file>/WEB-INF/index.jsp</welcome-file>
        </welcome-file-list>
        <servlet>
        <servlet-name>ImageServlet</servlet-name>
        <servlet-class>com.ImageServlet</servlet-class>
        </servlet>  
        <servlet-mapping>
        <servlet-name>ImageServlet</servlet-name>
        <url-pattern>/servlet/ImageServlet</url-pattern>
        </servlet-mapping>
        <servlet>
        <servlet-name>LoginServlet</servlet-name>
        <servlet-class>com.LoginServlet</servlet-class>
        </servlet>
        <servlet-mapping>
        <servlet-name>LoginServlet</servlet-name>
        <url-pattern>/servlet/LoginServlet</url-pattern>
        </servlet-mapping>
</web-app>
{% endhighlight xml %}

#### 1.6.运行程序如页面所示,输入验证码返回信息
![]({{base.url}}/img/checkCode01.png)


## 2.使用Jcaptcha生成验证码
#### 2.1.Jcaptcha是一个用来生成图形验证码的Java开源组件，使用起来也是非常的简单方便。与Spring结合使用，可产生多种形式的验证码。
#### 2.2.jcaptch因与JDK8以上版本不兼容等问题，所以这里不采用此方法。


## 3.使用Kaptcha生成验证码
#### 3.1.Kaptcha是一个非常实用的验证码生成工具，可以生成各种样式的验证码，它是可配置的。
#### 3.2.下载导入kaptcha-2.3.jar
#### 3.3.配置web.xml
{% highlight xml %}
        <?xml version="1.0" encoding="UTF-8"?>
        <web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns="http://java.sun.com/xml/ns/javaee"
        xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
        id="WebApp_ID" version="2.5">
            <display-name></display-name>    
          <!-- 配置默认首页 -->
          <welcome-file-list>
            <welcome-file>/WEB-INF/index.jsp</welcome-file>
          </welcome-file-list>
        <servlet>
                <servlet-name>kaptcha</servlet-name>
                <servlet-class>com.google.code.kaptcha.servlet.KaptchaServlet</servlet-class>
        </servlet>
        <servlet-mapping>
                <servlet-name>kaptcha</servlet-name>
                <url-pattern>/randomcode.jpg</url-pattern>
        </servlet-mapping>
        </web-app>
{% endhighlight xml %}

#### 3.4.编写验证码页面webapp/WEB-INF/index.jsp
{% highlight jsp %}
<%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<form action="WEB-INF/check.jsp">
        <img src="randomcode.jpg" />
        <input type="text" name="r" value="" />
        <input type="submit" value="提交">
</form>
</body>
</html>
{% endhighlight jsp %}

#### 3.5.编写校验页面webapp/check.jsp，(这里为了演示，所采用了在jsp校验)
{% highlight jsp %}
<%@page import="com.google.code.kaptcha.Constants"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
<%
        String k=(String)session.getAttribute(com.google.code.kaptcha.Constants.KAPTCHA_SESSION_KEY);
        String str=request.getParameter("r");
        if(k.equals(str)){
                out.println("true");
                out.print(k+"---"+str);
        }
%>
</body>
</html>
{% endhighlight jsp %}

#### 3.6.运行程序，输入正确的验证码，页面输出true和验证码信息。
#### 3.7.配置参数
#### 编写web.xml为验证码图片加边框
{% highlight xml %}
<servlet>
        <servlet-name>kaptcha</servlet-name>
        <servlet-class>com.google.code.kaptcha.servlet.KaptchaServlet</servlet-class>
                <init-param>
                        <description>是否有边框，合法值：yes|no</description>
                        <param-name>kaptcha.border</param-name>
                        <param-value>yes</param-value>
                </init-param>
</servlet>
{% endhighlight xml %}

#### 验证码边框颜色
{% highlight xml %}
<init-param>
        <description>边框颜色，合法值：white|blue等</description>
        <param-name>kaptcha.border.color</param-name>
        <param-value>blue</param-value>
</init-param>
{% endhighlight xml %}

#### 验证码边框厚度
{% highlight xml %}
<init-param>
        <description>边框厚度，合法值：>0</description>
        <param-name>kaptcha.border.thickness</param-name>
        <param-value>5</param-value>
</init-param>
{% endhighlight xml %}

#### 验证码宽度
{% highlight xml %}
<init-param>
        <description>验证码宽度</description>
        <param-name>kaptcha.image.width</param-name>
        <param-value>200</param-value>
</init-param>
{% endhighlight xml %}

#### 验证码高度
{% highlight xml %}
        <init-param>
          <description>验证码高度</description>
          <param-name>kaptcha.image.height</param-name>
          <param-value>50</param-value>
        </init-param>
{% endhighlight xml %}

#### 验证码实现类
{% highlight xml %}
        <init-param>
          <description>验证码实现类</description>
          <param-name>kaptcha.producer.impl</param-name>
          <param-value>com.google.code.kaptcha.impl.DefaultKaptcha</param-value>
        </init-param>
{% endhighlight xml %}

#### 文本实现类
{% highlight xml %}
<init-param>
        <description>文本实现类</description>
        <param-name>kaptcha.textproducer.impl</param-name>
        <param-value>com.google.code.kaptcha.impl.DefaultTextCreator</param-value>
</init-param>
{% endhighlight xml %}

#### 验证码值配置
{% highlight xml %}
<init-param>
        <description>文本集合，验证码值从此集合中获取</description>
        <param-name>kaptcha.textproducer.char.string</param-name>
        <param-value>qweruyatsudjhsajdsjah154163163</param-value>
</init-param>
{% endhighlight xml %}

#### 验证码字体配置
{% highlight xml %}
<init-param>
        <description>字体，Arial,Courier</description>
        <param-name>kaptcha.textproducer.font.names</param-name>
        <param-value>Arial,Courier</param-value>
</init-param>
{% endhighlight xml %}

#### 验证码字体大小
{% highlight xml %}
<init-param>
        <description>字体大小</description>
        <param-name>kaptcha.textproducer.font.size</param-name>
        <param-value>40</param-value>
</init-param>
{% endhighlight xml %}

#### 验证码字体颜色
{% highlight xml %}
<init-param>
        <description>字体颜色</description>
        <param-name>kaptcha.textproducer.font.color</param-name>
        <param-value>black</param-value>
</init-param>
{% endhighlight xml %}

#### 验证码长度
{% highlight xml %}
<init-param>
        <description>验证码长度 5</description>
        <param-name>kaptcha.textproducer.char.length</param-name>
        <param-value>2</param-value>
</init-param>
{% endhighlight xml %}

#### 验证码字体间隔
{% highlight xml %}
<init-param>
        <description>字体间隔</description>
        <param-name>kaptcha.textproducer.char.space</param-name>
        <param-value>2</param-value>
</init-param>
{% endhighlight xml %}

#### 验证码干扰线实现
{% highlight xml %}
<init-param>
        <description>干扰线实现类</description>
        <param-name>kaptcha.noise.impl</param-name>
        <!-- com.goole.code.kaptcha.impl.NoNoise -->
        <param-value>com.goole.code.kaptcha.impl.DefaultNoise</param-value>
</init-param>
{% endhighlight xml %}

#### 验证码干扰线颜色配置
{% highlight xml %}
<init-param>
        <description>干扰线颜色，rgb或white,blue</description>
        <param-name>kaptcha.noise.color</param-name>
        <param-value>black</param-value>
</init-param>
{% endhighlight xml %}

####  验证码图片样式
{% highlight xml %}
<init-param>
        <description>图片样式：
        水纹:com.goole.code.kaptcha.impl.WaterRipple、
        鱼眼:com.goole.code.kaptcha.impl.FishEyeGimpy、
        阴影:com.goole.code.kaptcha.impl.ShadowGimpy
        </description>
        <param-name>kaptcha.obscurificator.impl</param-name>
        <param-value>com.goole.code.kaptcha.impl.WaterRipple</param-value>
</init-param>
{% endhighlight xml %}

#### 验证码背景实现类
{% highlight xml %}
<init-param>
        <description>背景实现类</description>
        <param-name>kaptcha.background.impl</param-name>
        <param-value>com.goole.code.kaptcha.impl.DefaultBackground</param-value>
</init-param>
{% endhighlight xml %}

#### 验证码背景颜色渐变开始颜色
{% highlight xml %}
<init-param>
        <description>背景渐变开始颜色</description>
        <param-name>kaptcha.background.clear.from</param-name>
        <param-value>blue</param-value>
</init-param>
{% endhighlight xml %}

#### 验证码背景颜色渐变结束颜色
{% highlight xml %}
<init-param>
        <description>背景渐变结束颜色</description>
        <param-name>kaptcha.background.clear.to</param-name>
        <param-value>red</param-value>
</init-param>
{% endhighlight xml %}

#### 验证码文字渲染器
{% highlight xml %}
<init-param>
        <description>文字渲染器</description>
        <param-name>kaptcha.word.impl</param-name>
        <param-value>com.goole.code.kaptcha.text.impl.DefaultWordRenderer</param-value>
</init-param>
{% endhighlight xml %}

#### 验证码在session中存放验证码的key键配置
{% highlight xml %}
<init-param>
        <description>session中存放验证码的key键</description>
        <param-name>kaptcha.session.key</param-name>
        <param-value>KAPTCHA_SESSION_KEY</param-value>
</init-param>
{% endhighlight xml %}

#### 3.8.中文验证码实现
#### 编写中文验证码实现类com/ChineseText.java
{% highlight java %}
package com;
import java.util.Random;
import com.google.code.kaptcha.text.TextProducer;
import com.google.code.kaptcha.util.Configurable;
public class ChineseText extends Configurable implements TextProducer {
        public String getText() {
                int length = getConfig().getTextProducerCharLength();
                String[] s = new String[]{"如","若","阿","都","是","下","才","在","啥","不"};
                Random rand = new Random();
                StringBuffer sb = new StringBuffer();
                for(int i = 0; i < length; i++){
                        int ind = rand.nextInt(s.length);
                        sb.append(s[ind]);
                }
                return sb.toString();
        }
}
{% endhighlight java %}

#### 在web.xml中配置实现类
{% highlight xml %}
<init-param>
        <description>文本实现类</description>
        <param-name>kaptcha.textproducer.impl</param-name>
        <param-value>com.ChineseText</param-value>
</init-param>
<init-param>
        <description>字体，Arial,Courier</description>
        <param-name>kaptcha.textproducer.font.names</param-name>
        <param-value>微软雅黑</param-value>
        </init-param>
{% endhighlight xml %}

#### 运行程序，验证码如图
![]({{base.url}}/img/kaptcha01.png)
#### 3.9算式验证码
#### 重写com/KaptchaServlet.java
{% highlight java %}
package com;
import java.awt.image.BufferedImage;
import java.io.IOException;
import java.util.Enumeration;
import java.util.Properties;
import javax.imageio.ImageIO;
import javax.servlet.Servlet;
import javax.servlet.ServletConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletOutputStream;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import com.google.code.kaptcha.Producer;
import com.google.code.kaptcha.util.Config;
public class KaptchaServlet extends HttpServlet implements Servlet{
private Properties props;
private Producer kaptchaProducer;
private String sessionKeyValue;
public KaptchaServlet() {
        this.props = new Properties();
        this.kaptchaProducer = null;
        this.sessionKeyValue = null;
}
public void init(ServletConfig conf) throws ServletException {
        super.init(conf);
        ImageIO.setUseCache(false);
        Enumeration initParams = conf.getInitParameterNames();
        while (initParams.hasMoreElements()) {
                String key = (String) initParams.nextElement();
                String value = conf.getInitParameter(key);
                this.props.put(key, value);
        }
        Config config = new Config(this.props);
        this.kaptchaProducer = config.getProducerImpl();
        this.sessionKeyValue = config.getSessionKey();
}
public void doGet(HttpServletRequest req, HttpServletResponse resp)
                throws ServletException, IOException {
        resp.setDateHeader("Expires", 0L);
        resp.setHeader("Cache-Control", "no-store, no-cache, must-revalidate");
        resp.addHeader("Cache-Control", "post-check=0, pre-check=0");
        resp.setHeader("Pragma", "no-cache");
        resp.setContentType("image/jpeg");
        String capText = this.kaptchaProducer.createText();
        String s1 = capText.substring(0, 1);
        String s2 = capText.substring(1, 2);
        int r = Integer.valueOf(s1).intValue() + Integer.valueOf(s2).intValue();
        req.getSession().setAttribute(this.sessionKeyValue, String.valueOf(r));
        BufferedImage bi = this.kaptchaProducer.createImage(s1+"+"+s2+"=?");
        ServletOutputStream out = resp.getOutputStream();
        ImageIO.write(bi, "jpg", out);
        try {
                out.flush();
        } finally {
                out.close();
        }
}
}
{% endhighlight java %}

#### 配置web.xml
{% highlight xml %}
<servlet>
        <servlet-name>kaptcha</servlet-name>
        <servlet-class>com.KaptchaServlet</servlet-class>
        <init-param>
                <description>验证码长度 5</description>
                <param-name>kaptcha.textproducer.char.length</param-name>
                <param-value>2</param-value>
        </init-param>
</servlet>
{% endhighlight xml %}
#### 运行程序，效果如图
![]({{base.url}}/img/kaptcha02.png)
