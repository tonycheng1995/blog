---
layout: post
title: "Spring Boot起步"
date: 2017-02-28 20:14:34 +0800
toc: true
category: SpringBoot
tags: SpringBoot
excerpt: Spring Boot的设计目的是用来简化新Spring应用的初始搭建以及开发过程。因为SpringBoot内嵌Tomcat，所以SpringBoot可以以jar包形式执行`java -jar xx.jar`运行，SpringBoot使用starter起步依赖来简化包的加载。
---
#### 开发工具：IDEA
#### Spring Boot的设计目的是用来简化新Spring应用的初始搭建以及开发过程。因为SpringBoot内嵌Tomcat，所以SpringBoot可以以jar包形式执行`java -jar xx.jar`运行，SpringBoot使用starter起步依赖来简化包的加载。
#### 新建项目Spring Initializr
<img src="{{base.url}}/img/springboot01.png" >
<img src="{{base.url}}/img/springboot02.png">
#### 选择项目所需要的技术，这里我们只需勾中Web下的Web
<img src="{{base.url}}/img/springboot03.png" >
#### 设置项目名和项目存储位置
<img src="{{base.url}}/img/springboot04.png" >
#### 生成的项目的根包目录下会有一个入口文件{artifactId}Application命名规则的入口文件。运行项目
<img src="{{base.url}}/img/springboot05.png" >
#### 在浏览器输入http://localhost:8080 ,但是我却看到了一个404页面，不代表我们没有成功运行，而是我们还没写控制器
<img src="{{base.url}}/img/springboot06.png" >
#### 那么下面我们来写一个类，让它展示一些东西
#### 新建一个HelloController.java
{% highlight java %}
package com.chengzequn;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @RequestMapping(value = "/hello",method = RequestMethod.GET)
    public String say() {
        return "Hello Spring Boot!";
    }
}
{% endhighlight java %}

#### 重新运行项目，在浏览器输入http://localhost:8080 ,我们可以看到页面输出了“Hello Spring Boot！”
