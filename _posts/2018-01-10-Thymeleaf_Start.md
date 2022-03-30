---
layout: post
title: "ThymeLeaf入手"
date:   2018-01-10 +0800
toc: true
category: spring
tags: ThymeLeaf
excerpt: 本篇文章主要让开发者快速入手ThymeLeaf,具体内容可以下载文末的文档查看学习，或者查找其他文档学习。
---
#### JSP页面无法再模板被解析之前通过浏览器直接显示，而thymeLeaf不仅可以让浏览器正确显示页面信息，而且可以在浏览器静态打开时显示一个默认的值。
#### 在项目中引入thymeleaf-spring5-3.0.11.RELEASE的jar包的依赖，同时会引入thymeleaf-3.0.11.RELEASE的jar包
{% highlight xml %}
<dependency>
        <groupId>org.thymeleaf</groupId>
        <artifactId>thymeleaf-spring5</artifactId>
        <version>3.0.11.RELEASE</version>
</dependency>
{% endhighlight xml %}

#### 增加头文件
{% highlight html %}
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
{% endhighlight html %}

#### 然后就可以在页面中使用thymeleaf，如下当静态加载页面时会默认显示“James Carrot”，而动态加载时会显示正确的值。
{% highlight html %}
<input type="text" name="userName" value="James Carrot" th:value="${user.name}" />
{% endhighlight html %}

#### 其他属性语法可以参考[thymeleaf_3.0.5_中文参考手册](https://raw.githubusercontent.com/chengzequn/LearningDocument/master/thymeleaf_3.0.5_%E4%B8%AD%E6%96%87%E5%8F%82%E8%80%83%E6%89%8B%E5%86%8C.pdf)下载
