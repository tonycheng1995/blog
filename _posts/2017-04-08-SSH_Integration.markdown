---
layout: post
title: "SSH整合"
date:   2017-04-08 22:00:58 +0800
toc: true
category: SSH
tags: SSH
excerpt: 本篇文章主要是总结Struts2+Hibernate+Spring框架的整合流程
---
#### 本篇文章环境：Eclipse(neno.3)+Tomcat8.5
#### 本篇文章框架：Struts2.5.10.1+Hibernate5.0.12+Spring4.3.7
## 一、基本环境搭建
#### [Struts官网](http://struts.apache.org/)
* commons-fileupload-x.x.x.jar            #上传文件的jar包
* commons-io-x.x.jar                      #对本地文件流读取操作
* commons-lang3-x.x.jar                   #基础文件包
* commons-logging-x.x.x.jar               #日志包
* freemarker-x.x.x.jar                    #生成html、xml、java源代码等文本集合工具类包
* javassist-x.x.x-GA.jar                  #编译java字节码的类库，使java字节码操作更加简便
* log4j-api-x.x.jar                       #日志
* ognl-x.x.x.jar                          #struts2独有的标签库
* struts2-core-x.x.x.x.jar                #struts2核心包
* struts2-spring-plugin-2.5.10.1.jar      #struts2和spring整合包

#### [Hibernate官网](http://hibernate.org/orm/)
* lib\required下所有jar包
* lib\jpa下所有包
* lib\c3p0下的所有jar包

#### 数据库驱动包
* mysql-connector-java-x.x.x.jar

#### [Spring官网](http://projects.spring.io/spring-framework/)
#### 基本ioc包
* spring-beans-x.x.x.RELEASE.jar
* spring-context-x.x.x.RELEASE.jar
* spring-core-x.x.x.RELEASE.jar
* spring-expression-x.x.x.RELEASE.jar

#### 基本aop包
* spring-aop-x.x.x.RELEASE.jar
* spring-aspects-x.x.x.RELEASE.jar

#### 其他必须包
* spring-jdbc-x.x.x.RELEASE.jar
* spring-orm-x.x.x.RELEASE.jar
* spring-test-x.x.x.RELEASE.jar
* spring-tx-x.x.x.RELEASE.jar
* spring-web-x.x.x.RELEASE.jar

#### Struts2配置文件
#### web.xml
{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<web-app id="starter" version="2.4"
          xmlns="http://java.sun.com/xml/ns/j2ee"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">

<!-- 配置struts2的核心过滤器 -->
    <filter>
      <!-- 名字随意起 -->
        <filter-name>Struts2Filter</filter-name>
        <!-- struts2-core-x.x.x.jar下org.apache.struts2.dispatcher.filter包下
              StrutsPrepareAndExecuteFilter核心过滤器类文件的路径  	

          Ctrl+shift+T  即可查询到StrutsPrepareAndExecuteFilter完整路径 -->
        <filter-class>org.apache.struts2.dispatcher.filter.StrutsPrepareAndExecuteFilter</filter-class>
    </filter>

<!-- 配置拦截器的映射文件，让Strut2的核心Filter拦截所有请求 -->
    <filter-mapping>
      <!-- 名字要与上面配置的核心过滤器名字一致 -->
        <filter-name>Struts2Filter</filter-name>
        <!-- 默认过滤所有请求 -->
        <url-pattern>/*</url-pattern>
    </filter-mapping>

</web-app>
{% endhighlight xml %}

#### src/struts.xml
{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts PUBLIC
        "-//Apache Software Foundation//DTD Struts Configuration 2.5//EN"
        "http://struts.apache.org/dtds/struts-2.5.dtd">
<struts>

</struts>
{% endhighlight xml %}

#### Hibernate配置文件
#### hibernate.cfg.xml(在ssh整合中该配置文件可以省略)
#### 映射文件(后面配置)

#### spring配置文件
#### web.xml
{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<web-app id="starter" version="2.4"
          xmlns="http://java.sun.com/xml/ns/j2ee"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">

<!-- Spring的核心监听器 -->
<listener>
  <!--Ctrl+shift+T  即可查询到ContextLoaderListener完整路径 -->
  <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>

<context-param>
  <!-- 查询ContextLoaderListener父类ContextLoader源代码CONFIG_LOCATION_PARAM的值 -->
  <param-name>contextConfigLocation</param-name>
  <param-value>classpath:applicationContext.xml</param-value>
</context-param>

<!-- 配置struts2的核心过滤器 -->
    <filter>
      <!-- 名字随意起 -->
        <filter-name>Struts2Filter</filter-name>
        <!-- struts2-core-x.x.x.jar下org.apache.struts2.dispatcher.filter包下
              StrutsPrepareAndExecuteFilter核心过滤器类文件的路径  	

          Ctrl+shift+T  即可查询到StrutsPrepareAndExecuteFilter完整路径 -->
        <filter-class>org.apache.struts2.dispatcher.filter.StrutsPrepareAndExecuteFilter</filter-class>
    </filter>

<!-- 配置拦截器的映射文件，让Strut2的核心Filter拦截所有请求 -->
    <filter-mapping>
      <!-- 名字要与上面配置的核心过滤器名字一致 -->
        <filter-name>Struts2Filter</filter-name>
        <!-- 默认过滤所有请求 -->
        <url-pattern>/*</url-pattern>
    </filter-mapping>

</web-app>
{% endhighlight xml %}
#### applicationContext.xml
{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd">

</beans>
{% endhighlight xml %}

## 二、创建包结构
    cn.example.ssh
          ---action
          ---service
          ---dao
          ---domain

## 三、编写商品的实体domain/Product.java
{% highlight java %}
package cn.cheng.ssh.domain;
/**
  * 商品的实体
  * @author cheng
  *
  */
public class Product {

  private Integer pId;
  private String pName;
  private Double pPrice;
  public Integer getpId() {
    return pId;
  }
  public void setpId(Integer pId) {
    this.pId = pId;
  }
  public String getpName() {
    return pName;
  }
  public void setpName(String pName) {
    this.pName = pName;
  }
  public Double getpPrice() {
    return pPrice;
  }
  public void setpPrice(Double pPrice) {
    this.pPrice = pPrice;
  }

}
{% endhighlight java %}

## 四、Struts2整合spring
#### 创建页面（保存商品）addProduct.jsp
{% highlight jsp %}
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<%@ taglib prefix="s" uri="/struts-tags" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>保存商品的页面</title>
</head>
<body>
<h1>保存商品的页面</h1>
<s:form action="product_save" method="post" namespace="/" theme="simple">
  <table border="1" width="400">
    <tr>
      <td>商品名称</td>
      <td><s:textfield name="pName"/></td>
    </tr>
    <tr>
      <td>商品价格</td>
      <td><s:textfield name="pPrice"/></td>
    </tr>
    <tr>
      <td colspan="2"><input type="submit" value="添加"></td>
    </tr>
  </table>
</s:form>
</body>
</html>
{% endhighlight jsp %}

## 五、编写action、DAO、service
#### ProductDAO.java
{% highlight java %}
package cn.cheng.ssh.service;
import cn.cheng.ssh.dao.ProductDao;
/**
  * 商品管理的业务层类
  * @author cheng
  */
public class ProductService {

  //业务层注入DAO的类
  private ProductDao productDao;

  public ProductDao getProductDao() {
    return productDao;
  }

}
{% endhighlight java %}

#### ProductService.java
{% highlight java %}
package cn.cheng.ssh.service;
import cn.cheng.ssh.dao.ProductDao;
/**
  * 商品管理的业务层类
  * @author cheng
  */
public class ProductService {

  //业务层注入DAO的类
  private ProductDao productDao;

  public ProductDao getProductDao() {
    return productDao;
  }

}
{% endhighlight java %}

#### ProductAction.java
{% highlight java %}
package cn.cheng.ssh.action;
/**
  * 商品管理的Action类
  * @author cheng
  */
import com.opensymphony.xwork2.ActionSupport;
import com.opensymphony.xwork2.ModelDriven;
import cn.cheng.ssh.domain.Product;
import cn.cheng.ssh.service.ProductService;
public class ProductAction extends ActionSupport implements ModelDriven<Product>{

  //模型驱动使用的类
  private Product product=new Product();

  @Override
  public Product getModel() {
    return product;
  }

  //Struts和Spring整合过程中按名称自动注入的业务层类
  private ProductService productService;

  public ProductService getProductService() {
    return productService;
  }

}
{% endhighlight java %}

## 六、配置Action、Service和DAO类
#### struts2和Spring整合的两种方式：
#### ·Action的类由Struts2自身去创建
#### ·Action的类交给Spring框架创建(推荐)

#### 1.Action的类由Struts2自身去创建
{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts PUBLIC
        "-//Apache Software Foundation//DTD Struts Configuration 2.5//EN"
        "http://struts.apache.org/dtds/struts-2.5.dtd">

<struts>

  <package name="ssh" extends="struts-default" namespace="/">
    <action name="product_*" class="cn.cheng.ssh.action.ProductAction" method="{1}"></action>
  </package>

</struts>
{% endhighlight xml %}

#### Service和DAO配置
#### 直接在applicationContext.xml中进行配置
{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd">

  <!-- 配置业务层的类 -->
  <bean id="productService" class="cn.cheng.ssh.service.ProductService">
    <property name="productDao" ref="productDao"></property>
  </bean>

  <!-- 配置DAO类 -->
  <bean id="productDao" class="cn.cheng.ssh.dao.ProductDao">

  </bean>
</beans>
{% endhighlight xml %}

#### 编写DAO层商品的方法
{% highlight java %}
package cn.cheng.ssh.dao;
import cn.cheng.ssh.domain.Product;
/**
  * 商品管理的DAO类
  * @author cheng
  */
public class ProductDao {

  /**
    * DAO中保存商品的方法
    * @param product
    */
  public void save(Product product){
    System.out.println("DAO中的保存商品方法执行了...");
  }

}
{% endhighlight java %}

#### 编写业务层商品的方法
{% highlight java %}
package cn.cheng.ssh.service;
import cn.cheng.ssh.dao.ProductDao;
import cn.cheng.ssh.domain.Product;
/**
* 商品管理的业务层类
* @author cheng
*/
public class ProductService {

//业务层注入DAO的类
private ProductDao productDao;

public void setProductDao(ProductDao productDao) {
  this.productDao = productDao;
}

/**
* 业务层保存商品的方法
* @param product
*/
public void save(Product product){
  System.out.println("Service中的save方法执行了...");
  productDao.save(product);
}

}
{% endhighlight java %}
#### 编写action中的执行方法
{% highlight java %}
package cn.cheng.ssh.action;
/**
* 商品管理的Action类
* @author cheng
*/
import com.opensymphony.xwork2.ActionSupport;
import com.opensymphony.xwork2.ModelDriven;
import cn.cheng.ssh.domain.Product;
import cn.cheng.ssh.service.ProductService;

public class ProductAction extends ActionSupport implements ModelDriven<Product>{

//模型驱动使用的类
private Product product=new Product();

@Override
public Product getModel() {
  return product;
}

//Struts和Spring整合过程中按名称自动注入的业务层类
private ProductService productService;

public void setProductService(ProductService productService) {
  this.productService = productService;
}

/**
* 保存商品的执行方法：save
*/
public String save(){
  System.out.println("Action中的save方法执行了...");
  productService.save(product);
  return NONE;
}

}
{% endhighlight java %}

#### 运行项目，提交表单控制台输出以下语句，说明配置正确
    Action中的save方法执行了...
    Service中的save方法执行了...
    DAO中的保存商品方法执行了...


#### 2.Action类交给Spring管理
#### applicationContext.xml
{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd">

  <!-- 配置Action的类 -->
  <bean id="productAction" class="cn.cheng.ssh.action.ProductAction" scope="prototype">
    <!-- 手动注入Service -->
    <property name="productService" ref="productService"/>
  </bean>

  <!-- 配置业务层的类 -->
  <bean id="productService" class="cn.cheng.ssh.service.ProductService">
    <property name="productDao" ref="productDao"></property>
  </bean>

  <!-- 配置DAO类 -->
  <bean id="productDao" class="cn.cheng.ssh.dao.ProductDao">

  </bean>
</beans>
{% endhighlight xml %}

#### struts.xml
{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE struts PUBLIC
        "-//Apache Software Foundation//DTD Struts Configuration 2.5//EN"
        "http://struts.apache.org/dtds/struts-2.5.dtd">

<struts>

  <package name="ssh" extends="struts-default" namespace="/">
    <action name="product_*" class="productAction" method="{1}"></action>
  </package>

</struts>
{% endhighlight xml %}

#### 运行项目，提交表单控制台输出以下语句，说明配置正确
    Action中的save方法执行了...
    Service中的save方法执行了...
    DAO中的保存商品方法执行了...


## 七、Spring整合Hibernate
#### 创建数据库
{% highlight sql %}
CREATE DATABASE ssh;
{% endhighlight sql %}

#### 创建映射文件Product.hbm.xml
{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<!-- hibernate包hibernate下的hibernate-mapping-x.x.dtd -->
<!DOCTYPE hibernate-mapping PUBLIC
    "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
    "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">

<hibernate-mapping>
  <class name="cn.cheng.ssh.domain.Product" table="product">
    <id name="pId" column="pId">
      <generator class="native"/>
    </id>
    <property name="pName" column="pName" length="20"/>
    <property name="pPrice" column="pPrice"/>
  </class>
</hibernate-mapping>
{% endhighlight xml %}

#### 新建属性文件src/jdbc.properties
{% highlight properties %}
    jdbc.driverClass=com.mysql.jdbc.Driver
    jdbc.url=jdbc:mysql://localhost:3306/ssh
    jdbc.username=root
    jdbc.password=123
{% endhighlight properties %}
#### 在spring.xml引入jdbc.properties,配置连接池，配置Hibernate相关属性
{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:context="http://www.springframework.org/schema/context"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd">

<!-- 引入外部的属性文件 -->
<context:property-placeholder location="classpath:jdbc.properties" />
<!-- 配置c3p0连接池 -->
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
    <property name="driverClass" value="${jdbc.driverClass}"/>
    <property name="jdbcUrl"  value="${jdbc.url}"/>
    <property name="user" value="${jdbc.username}"/>
    <property name="password" value="${jdbc.password}"/>
</bean>

<!-- 配置Hibernate的相关属性 -->
<!-- spring-orm-x.x.jar下的org.springframework.orm.hibernate{x}.LocalSessionFactoryBean -->
<bean id="sessionFactory" class="org.springframework.orm.hibernate5.LocalSessionFactoryBean">
    <!-- 注入连接池 -->
    <property name="dataSource" ref="dataSource"/>
    <!-- 配置Hibernate的属性 -->
    <property name="hibernateProperties">
      <props>
        <!-- mySQL的方言   project/etc/hibernate.properties -->
        <prop key="hibernate.dialect"> org.hibernate.dialect.MySQLDialect</prop>
        <prop key="hibernate.show_sql">true</prop>
        <prop key="hibernate.format_sql">true</prop>
        <!-- 自动更新表结构 -->
        <prop key="hibernate.hbm2ddl.auto">update</prop>
      </props>
    </property>
    <!-- 加载Hibernate中的映射文件 -->
    <property name="mappingResources">
      <list>
        <value>cn/cheng/ssh/domain/Product.hbm.xml</value>
      </list>
    </property>
</bean>

<!-- 配置Action的类 -->
<bean id="productAction" class="cn.cheng.ssh.action.ProductAction" scope="prototype">
    <!-- 手动注入Service -->
    <property name="productService" ref="productService"/>
</bean>

<!-- 配置业务层的类 -->
<bean id="productService" class="cn.cheng.ssh.service.ProductService">
    <property name="productDao" ref="productDao"></property>
</bean>

<!-- 配置DAO类 -->
<bean id="productDao" class="cn.cheng.ssh.dao.ProductDao">

</bean>
</beans>
{% endhighlight xml %}

#### 运行项目，自动生成相应表结构

## 八、编写DAO中代码（使用Hibernate的模板）
#### 注入模板
{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd">

  <!-- 引入外部的属性文件 -->
  <context:property-placeholder location="classpath:jdbc.properties" />
  <!-- 配置c3p0连接池 -->
  <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
    <property name="driverClass" value="${jdbc.driverClass}"/>
    <property name="jdbcUrl"  value="${jdbc.url}"/>
    <property name="user" value="${jdbc.username}"/>
    <property name="password" value="${jdbc.password}"/>
  </bean>

  <!-- 配置Hibernate的相关属性 -->
  <!-- spring-orm-x.x.jar下的org.springframework.orm.hibernate{x}.LocalSessionFactoryBean -->
  <bean id="sessionFactory" class="org.springframework.orm.hibernate5.LocalSessionFactoryBean">
    <!-- 注入连接池 -->
    <property name="dataSource" ref="dataSource"/>
    <!-- 配置Hibernate的属性 -->
    <property name="hibernateProperties">
      <props>
        <!-- mySQL的方言   project/etc/hibernate.properties -->
        <prop key="hibernate.dialect"> org.hibernate.dialect.MySQLDialect</prop>
        <prop key="hibernate.show_sql">true</prop>
        <prop key="hibernate.format_sql">true</prop>
        <!-- 自动更新表结构 -->
        <prop key="hibernate.hbm2ddl.auto">update</prop>
      </props>
    </property>
    <!-- 加载Hibernate中的映射文件 -->
    <property name="mappingResources">
      <list>
        <value>cn/cheng/ssh/domain/Product.hbm.xml</value>
      </list>
    </property>
  </bean>

  <!-- 配置Action的类 -->
  <bean id="productAction" class="cn.cheng.ssh.action.ProductAction" scope="prototype">
    <!-- 手动注入Service -->
    <property name="productService" ref="productService"/>
  </bean>

  <!-- 配置业务层的类 -->
  <bean id="productService" class="cn.cheng.ssh.service.ProductService">
    <property name="productDao" ref="productDao"></property>
  </bean>

  <!-- 配置DAO类 -->
  <bean id="productDao" class="cn.cheng.ssh.dao.ProductDao">
    <!-- 注入Hibernate模板 -->
    <property name="sessionFactory" ref="sessionFactory"/>
  </bean>
</beans>
{% endhighlight xml %}

#### 编写DAO代码
{% highlight java %}
package cn.cheng.ssh.dao;
import org.springframework.orm.hibernate5.support.HibernateDaoSupport;
import cn.cheng.ssh.domain.Product;
/**
  * 商品管理的DAO类
  * @author cheng
  */
//注入Hibernate模板
public class ProductDao extends HibernateDaoSupport{

  /**
    * DAO中保存商品的方法
    * @param product
    */
  public void save(Product product){
    System.out.println("DAO中的保存商品方法执行了...");
    //调用Hibernate模板中的save方法
    this.getHibernateTemplate().save(product);
  }

}
{% endhighlight java %}

## 九、事务管理
#### 配置事务管理器，开启注解事务
{% highlight xml %}
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xmlns:context="http://www.springframework.org/schema/context"
xmlns:tx="http://www.springframework.org/schema/tx"
xsi:schemaLocation="http://www.springframework.org/schema/beans
  http://www.springframework.org/schema/beans/spring-beans.xsd
  http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.3.xsd
  http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.3.xsd">

<!-- 引入外部的属性文件 -->
<context:property-placeholder location="classpath:jdbc.properties" />
<!-- 配置c3p0连接池 -->
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
  <property name="driverClass" value="${jdbc.driverClass}"/>
  <property name="jdbcUrl"  value="${jdbc.url}"/>
  <property name="user" value="${jdbc.username}"/>
  <property name="password" value="${jdbc.password}"/>
</bean>

<!-- 配置Hibernate的相关属性 -->
<!-- spring-orm-x.x.jar下的org.springframework.orm.hibernate{x}.LocalSessionFactoryBean -->
<bean id="sessionFactory" class="org.springframework.orm.hibernate5.LocalSessionFactoryBean">
  <!-- 注入连接池 -->
  <property name="dataSource" ref="dataSource"/>
  <!-- 配置Hibernate的属性 -->
  <property name="hibernateProperties">
    <props>
      <!-- mySQL的方言   project/etc/hibernate.properties -->
      <prop key="hibernate.dialect"> org.hibernate.dialect.MySQLDialect</prop>
      <prop key="hibernate.show_sql">true</prop>
      <prop key="hibernate.format_sql">true</prop>
      <!-- 自动更新表结构 -->
      <prop key="hibernate.hbm2ddl.auto">update</prop>
    </props>
  </property>
  <!-- 加载Hibernate中的映射文件 -->
  <property name="mappingResources">
    <list>
      <value>cn/cheng/ssh/domain/Product.hbm.xml</value>
    </list>
  </property>
</bean>

<!-- 配置Action的类 -->
<bean id="productAction" class="cn.cheng.ssh.action.ProductAction" scope="prototype">
  <!-- 手动注入Service -->
  <property name="productService" ref="productService"/>
</bean>

<!-- 配置业务层的类 -->
<bean id="productService" class="cn.cheng.ssh.service.ProductService">
  <property name="productDao" ref="productDao"></property>
</bean>

<!-- 配置DAO类 -->
<bean id="productDao" class="cn.cheng.ssh.dao.ProductDao">
  <!-- 注入Hibernate模板 -->
  <property name="sessionFactory" ref="sessionFactory"/>
</bean>

<!-- 配置事务管理器 -->
<bean id="transactionManager" class="org.springframework.orm.hibernate5.HibernateTransactionManager">
  <property name="sessionFactory" ref="sessionFactory" />
</bean>

<!-- 开启注解事务 -->
<tx:annotation-driven transaction-manager="transactionManager"/>

</beans>
{% endhighlight xml %}

#### 添加事务注解
{% highlight java %}
package cn.cheng.ssh.service;
import org.springframework.transaction.annotation.Transactional;
import cn.cheng.ssh.dao.ProductDao;
import cn.cheng.ssh.domain.Product;
/**
  * 商品管理的业务层类
  * @author cheng
  */
@Transactional
public class ProductService {

  //业务层注入DAO的类
  private ProductDao productDao;

  public void setProductDao(ProductDao productDao) {
    this.productDao = productDao;
  }

  /**
    * 业务层保存商品的方法
    * @param product
    */
  public void save(Product product){
    System.out.println("Service中的save方法执行了...");
    productDao.save(product);
  }

}
{% endhighlight java %}

## 十、运行项目，提交表单，数据库内容被修改
#### [项目源代码](https://github.com/chengzequn/SSH)
