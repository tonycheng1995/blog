---
layout: post
title: "Struts2国际化"
date:   2017-04-26 +0800
toc: true
category: Struts2
tags: SSH Struts2
excerpt: 本篇文章主要讲解Struts2的国际化
---
#### Struts2中的国际化就是i18n(Internationalization)
#### 实现步骤
#### 1.在struts.xml中加入
#### \<constant name="struts.custom.i18n.resources" value="message" />
#### 2.在与struts.xml文件同目录下创建以下两个文件“message”与value值一致
#### message_en_US.properties(配置英文信息)
#### message_zh_CN.properties(配置中文信息)
#### 3.在jsp中使用struts的标签完成界面所有内容

#### struts.xml
{% highlight xml %}
<struts>
    <constant name="struts.custom.i18n.resources" value="message"></constant>

    <package name="test" extends="struts-default">
        <action name="changeLanguage" class="com.cheng.action.ChangeLanguageAction">
            <result name="success">/index.jsp</result>
        </action>
    </package>
</struts>
{% endhighlight xml %}

#### message_zh_CN.properties
{% highlight properties %}
login.username=\u7528\u6237\u540D
login.password=\u5BC6\u7801
login.title=\u767B\u5F55\u754C\u9762
login.submit=\u63D0\u4EA4
{% endhighlight properties %}

#### message_en_US.properties
{% highlight properties %}
login.username=username
login.password=password
login.title=loginInterface
login.submit=submit
{% endhighlight properties %}

#### ChangeLanguageAction.java
{% highlight java %}
package com.cheng.action;
import com.opensymphony.xwork2.ActionSupport;
public class ChangeLanguageAction extends ActionSupport {

    @Override
    public String execute() throws Exception {

        return SUCCESS;
    }
}
{% endhighlight java %}