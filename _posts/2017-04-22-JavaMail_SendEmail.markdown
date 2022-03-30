---
layout: post
title: "JavaMail发送邮件"
date:   2017-04-22 +0800
toc: true
category: Email
tags: Java Email
excerpt: 本篇文章主要讲解如何利用JavaMail发送邮件
---
#### ·下载[JavaMail](http://www.oracle.com/technetwork/java/javamail/index.html)
#### ·下载[JAF](http://www.oracle.com/technetwork/articles/java/index-135046.html)
#### 解压并复制mail.jar和activation.jar到项目中

## SMTP协议(发送邮件)
#### SMTP(Simple Mail Transfer Protocol)即简单邮件传输协议，是一组用于由源地址到目的地址传送邮件的规则，由它来控制信件的中转方式。
#### SMTP协议属于TCP/IP协议簇，它帮助每台计算机在发送或中转信件时找到下一个目的地。
#### SMTP服务器默认端口25

## POP3协议
#### POP3(Post Office Protocol-Version 3)即邮局协议版本3
#### 本协议主要用于支持使用客户端远程管理在服务器上的电子邮件
#### POP3服务器默认端口110

## 以QQ邮箱为例发送邮件
#### 1.开启服务并获取授权码
![]({{site.url}}/img/javaMail01.png)
#### 2.编写Java代码
#### SendEmail.java
{% highlight java %}
import java.util.Properties;
import javax.mail.Authenticator;
import javax.mail.Message;
import javax.mail.MessagingException;
import javax.mail.PasswordAuthentication;
import javax.mail.Session;
import javax.mail.Transport;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;

public class SendEmail
{
   public static void main(String [] args)
   {
      // 收件人电子邮箱
      String to = "619***897@qq.com";

      // 发件人电子邮箱
      String from = "2811***772@qq.com";

      // 指定发送邮件的主机为 smtp.qq.com
      String host = "smtp.qq.com";  //QQ 邮件服务器

      // 获取系统属性
      Properties properties = System.getProperties();

      // 设置邮件服务器
      properties.setProperty("mail.smtp.host", host);

      //用户密码认证
      properties.put("mail.smtp.auth", "true");

      //配置SSL加密方式
      properties.put("mail.smtp.port", "587");

      //调试模式
      //properties.put("mail.debug", "true");

      // 获取默认session对象
      Session session = Session.getDefaultInstance(properties,new Authenticator(){
      public PasswordAuthentication getPasswordAuthentication()
      {
         return new PasswordAuthentication("2811***772@qq.com", "ksva********dcgj"); //发件人邮件用户名、密码
      }
      });

      try{
         // 创建默认的 MimeMessage 对象
         MimeMessage message = new MimeMessage(session);

         // Set From: 头部头字段
         message.setFrom(new InternetAddress(from));

         // Set To: 头部头字段
         message.addRecipient(Message.RecipientType.TO,
                                 new InternetAddress(to));

         // Set Subject: 头部头字段
         message.setSubject("邮件消息标题");

         // 设置消息体
         message.setText("邮件消息具体内容");

         // 发送消息
         Transport.send(message);
         System.out.println("邮件发送成功");
      }catch (MessagingException mex) {
         mex.printStackTrace();
      }
   }
}
{% endhighlight java %}

#### 3.查看邮箱可以看到收到测试的邮件信息
![]({{site.url}}/img/javaMail02.png)

#### 问题1
#### javax.mail.AuthenticationFailedException: 530 Error: A secure connection is requiered(such as ssl). More information at http://service.mail.qq.com/cgi-bin/help?id=28
#### 解决
#### 设置SSL加密方式端口号465或587
#### properties.put("mail.smtp.port", "465");

#### 问题2
#### javax.mail.AuthenticationFailedException: 535 Error: ?????????¨?????????ê?é????: http://service.mail.qq.com/cgi-bin/help?subtype=1&&id=28&&no=1001256
#### 解决
#### 检查用户名和密码是否写错(注意：密码是授权密码)

#### 问题3
#### 更多问题查询[QQ邮箱帮助中心](http://service.mail.qq.com/cgi-bin/help?id=28)

## 以Gmail邮箱为例
#### 1.首先你要在[谷歌账户安全页面](https://myaccount.google.com/lesssecureapps)允许账号在不够安全的应用上登录
![]({{site.url}}/img/javaMail03.png)
#### Java代码
{% highlight java %}
import java.util.Properties;
import javax.mail.Authenticator;
import javax.mail.Message;
import javax.mail.MessagingException;
import javax.mail.PasswordAuthentication;
import javax.mail.Session;
import javax.mail.Transport;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;

public class SendEmail
{
   public static void main(String [] args)
   {
      // 收件人电子邮箱
      String to = "619***897@qq.com";

      // 发件人电子邮箱
      String from = "cheng@gmail.com";

      // 指定发送邮件的主机为 smtp.gmail.com
      String host = "smtp.gmail.com";  //gmail 邮件服务器

      // 获取系统属性
      Properties properties = System.getProperties();

      // 设置邮件服务器
      properties.setProperty("mail.smtp.host", host);

      //用户密码认证
      properties.put("mail.smtp.auth", "true");

      //配置SSL加密方式
      properties.put("mail.smtp.port", "587");

      properties.put("mail.smtp.starttls.enable", "true");

      //调试模式
      properties.put("mail.debug", "true");

      // 获取默认session对象
      Session session = Session.getDefaultInstance(properties,new Authenticator(){
      public PasswordAuthentication getPasswordAuthentication()
      {
         return new PasswordAuthentication("cheng@gmail.com", "1234567890"); //发件人邮件用户名、密码
      }
      });

      try{
         // 创建默认的 MimeMessage 对象
         MimeMessage message = new MimeMessage(session);

         // Set From: 头部头字段
         message.setFrom(new InternetAddress(from));

         // Set To: 头部头字段
         message.addRecipient(Message.RecipientType.TO,
                                 new InternetAddress(to));

         // Set Subject: 头部头字段
         message.setSubject("邮件消息标题");

         // 设置消息体
         message.setText("邮件消息具体内容");

         // 发送消息
         Transport.send(message);
         System.out.println("邮件发送成功");
      }catch (MessagingException mex) {
         mex.printStackTrace();
      }
   }
}
{% endhighlight java %}
## 以outlook为例
#### SendEmail.java
{% highlight java %}
import java.util.Properties;
import javax.mail.Authenticator;
import javax.mail.Message;
import javax.mail.MessagingException;
import javax.mail.PasswordAuthentication;
import javax.mail.Session;
import javax.mail.Transport;
import javax.mail.internet.InternetAddress;
import javax.mail.internet.MimeMessage;

public class SendEmail
{
   public static void main(String [] args)
   {
      // 收件人电子邮箱
      String to = "619***897@qq.com";

      // 发件人电子邮箱
      String from = "cheng@outlook.com";

      // 指定发送邮件的主机为 smtp.outlook.com
      String host = "smtp.outlook.com";  //outlook邮件服务器

      // 获取系统属性
      Properties properties = System.getProperties();

      // 设置邮件服务器
      properties.setProperty("mail.smtp.host", host);

      //用户密码认证
      properties.put("mail.smtp.auth", "true");

      //配置SSL加密方式
      properties.put("mail.smtp.port", "587");

      properties.put("mail.smtp.starttls.enable", "true");

      //调试模式
      properties.put("mail.debug", "true");

      // 获取默认session对象
      Session session = Session.getDefaultInstance(properties,new Authenticator(){
      public PasswordAuthentication getPasswordAuthentication()
      {
         return new PasswordAuthentication("cheng@outlook.com", "1234567890"); //发件人邮件用户名、密码
      }
      });

      try{
         // 创建默认的 MimeMessage 对象
         MimeMessage message = new MimeMessage(session);

         // Set From: 头部头字段
         message.setFrom(new InternetAddress(from));

         // Set To: 头部头字段
         message.addRecipient(Message.RecipientType.TO,
                                 new InternetAddress(to));

         // Set Subject: 头部头字段
         message.setSubject("邮件消息标题");

         // 设置消息体
         message.setText("邮件消息具体内容");

         // 发送消息
         Transport.send(message);
         System.out.println("邮件发送成功");
      }catch (MessagingException mex) {
         mex.printStackTrace();
      }
   }
}
{% endhighlight java %}