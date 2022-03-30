---
layout: post
title: "MyBatis初识"
date:   2017-03-09 22:23:58 +0800
toc: true
category: MyBatis
tags: MyBatis
excerpt: MyBatis 是支持定制化 SQL、存储过程以及高级映射的优秀的持久层框架。MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。MyBatis 可以对配置和原生Map使用简单的 XML 或注解，将接口和 Java 的 POJOs(Plain Old Java Objects,普通的 Java对象)映射成数据库中的记录。所以学习MyBatis的使用对我们来说是一个必不可少的技能。
---
## MyBatis工作流程
#### 1.读取(基本)配置文件
#### 2.生成SqlSessionFactory(建立与数据库之间的会话)
#### 3.建立SqlSession(执行SQL语句)
#### 4.调用MyBatis提供的API
#### 5.查询MAP配置(放置SQL语句)
#### 6.返回结果
#### 7.关闭SqlSession

## MyBatis搭建
#### 1.jar包下载地址：[MyBatis下载](https://github.com/mybatis/mybatis-3/releases)
![]({{site.url}}/img/mybatis00.png)
#### 2.下载解压后目录如下，lib文件夹下是一些辅助包
![]({{site.url}}/img/mybatis01.png)
#### 3.导入mybatis-x.x.x.jar、lib下的所有jar包、MySQL驱动包mysql-connector-java-x.x.x-bin.jar
#### 4.添加日志配置文件log4j.properties
{% highlight properties %}
#日志输出级别设置为debug(SQL语句在debug级别才能输出,inform级则不能输出)
log4j.rootLogger=DEBUG,Console

#Console
log4j.appender.Console=org.apache.log4j.ConsoleAppender
log4j.appender.Console.layout=org.apache.log4j.PatternLayout
log4j.appender.Console.layout.ConversionPattern=%d [%t] %-5p [%c] - %m%n

log4j.logger.java.sql.ResultSet=INFO
log4j.logger.org.apache=INFO
log4j.logger.java.sql.Connection=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.PreparedStatement=DEBUG
{% endhighlight properties %}

#### 5.新建数据库表user,并添加数据
{% highlight sql %}
id,int(11),no null,primary key
userName,varchar(20)
password,varchar(20)
{% endhighlight sql %}

#### 6.建立POJO类User.java
{% highlight java %}
package cheng.book.pojo;

public class User {

private int id;
private String userName;
private String password;
public int getId() {
  return id;
}
public void setId(int id) {
  this.id = id;
}
public String getUserName() {
  return userName;
}
public void setUserName(String userName) {
  this.userName = userName;
}
public String getPassword() {
  return password;
}
public void setPassword(String password) {
  this.password = password;
}

}
{% endhighlight java %}

#### 7.建立基本配置文件MyBatisConfig.xml
{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" ?>

<!DOCTYPE configuration
    PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>

  <!-- 定义别名 -->
  <typeAliases>
    <typeAlias alias="User" type="cheng.book.pojo.User"/>
  </typeAliases>

  <!-- 连接数据库的信息 -->
  <environments default="development">
    <environment id="development">
    <!-- 配置事务处理 -->
      <transactionManager type="JDBC">
      </transactionManager>
      <dataSource type="POOLED">
        <property name="driver" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/book"/>
        <property name="username" value="root"/>
        <property name="password" value="root"/>
      </dataSource>
    </environment>
  </environments>

  <!-- map配置文件位置 -->
  <mappers>
      <mapper resource="cheng/book/map/User.xml"/>
  </mappers>

</configuration>
{% endhighlight xml %}
#### 附：MyBatisConfig.xml配置文件引入map文件的3种方式：
#### 1.相对路径引用
{% highlight xml %}
<mapper>
  <mapper resource=”cheng/book/map/User.xml” />
</mapper>
{% endhighlight xml %}

#### 2.绝对路径引用
{% highlight xml %}
<mapper url=”file:///var/sqlmaps/AuthorMapper.xml” />
{% endhighlight xml %}
#### 3.包路径引用
{% highlight xml %}
<mapper name=”com.mybatis.mapperinterface” />
{% endhighlight xml %}
#### 8.建立map配置文件User.xml
{% highlight xml %}
<?xml version="1.0" encoding="UTF-8" ?>

<!DOCTYPE mapper
    PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="/">

  <select id="findById"  parameterType="int"  resultType="User">
      select * from User where id=#{id}
  </select>

</mapper>
{% endhighlight xml %}
#### 9.建立测试类
{% highlight java %}
package cheng.boot.test;

import java.io.IOException;
import java.io.Reader;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import cheng.book.pojo.User;

public class Test {

  public static void main(String[] args) {

    String resource="cheng/book/map/MyBatisConfig.xml";
    Reader reader=null;
    SqlSession session;
    try {
      reader=Resources.getResourceAsReader(resource);
    } catch (IOException e) {
      e.printStackTrace();
    }

    SqlSessionFactory ssf=new SqlSessionFactoryBuilder().build(reader);
    session=ssf.openSession();
    User u=session.selectOne("findById", 1);
    System.out.println(u.getUserName());
    session.close();
  }

}
{% endhighlight java %}
#### 10.运行测试类,控制台输出查询的内容
#### 代码下载：[百度云盘](http://pan.baidu.com/s/1mhEkAAs) 密码：sxbm
