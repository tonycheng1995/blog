---
layout: post
title: "数据库横表纵表转换"
date:   2018-11-20 +0800
toc: true
category: database
tags: 数据库
excerpt: 本篇文章主要讲解数据库中横表与纵表之间的转换。
---
#### 横表就是普通的建表方式,表结构一般为：主键、字段1、字段2、字段3...。如果变为纵表，表结构则为：主键、字段代码、字段值。字段代码为字段1、字段2、字段3...。
#### 横表
#### ·优点：一行表示了一个实体记录，清晰可见，一目了然
#### ·缺点：如果要给表加一个新字段，那么必须重建表结构
#### 纵表
#### ·优点：如果给表添加一个新字段，只需添加一些记录
#### ·缺点：数据描述不是很清晰，会造成数据库数据很多。如果需要分组统计，要先group by,比较繁琐。
#### 因此，建议把不需要改动表结构的设计为横表，需要经常改动不确定的表结构设计为纵表。

#### 横表
![]({{site.url}}/img/HorizontalTable.png)
#### 纵表
![]({{site.url}}/img/VerticalTable.png)

## 横表转纵表
#### (适用于mySQL)
{% highlight sql %}
select t.id,t.name,'MEDICAL_FEE' as FEE_NAME,t.medical_fee as FEE from A_TEST t
union all
select t.id,t.name,'RENT' as FEE_NAME,t.rent as FEE from A_TEST t
union all
select t.id,t.name,'ELECTRICITY_FEE' as FEE_NAME,t.electricity_fee as FEE from A_TEST t
union all
select t.id,t.name,'PHONE_FEE' as FEE_NAME,t.phone_fee as FEE from A_TEST t
union all
select t.id,t.name,'WTER_FEE' as FEE_NAME,t.water_fee as FEE from A_TEST t
{% endhighlight sql %}

![]({{site.url}}/img/HorizontalToVertical.png)
## 纵表转横表
#### (适用于mySQL,注:因为字段中有汉字，所以Orcal查询中要用到to_char())
{% highlight sql %}
select t.name,
sum(case to_char(t.fee_name) when 'MEDICAL_FEE' then t.fee end) as MEDICAL_FEE,
sum(case to_char(t.fee_name) when 'RENT' then t.fee end) as RENT,
sum(case to_char(t.fee_name) when 'ELECTRICITY_FEE' then t.fee end) as ELECTRICITY_FEE,
sum(case to_char(t.fee_name) when 'PHONE_FEE' then t.fee end) as PHONE_FEE,
sum(case to_char(t.fee_name) when 'WTER_FEE' then t.fee end) as WTER_FEE
from B_TEST t
group by t.name
{% endhighlight sql %}
![]({{site.url}}/img/VerticalToHorizontal.png)
