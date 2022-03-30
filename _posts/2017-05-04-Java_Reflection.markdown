---
layout: post
title: "Java反射机制"
date:   2017-05-04 +0800
toc: true
category: Java
tags: Java 反射
excerpt: 本篇文章主要讲解Java的反射机制
---
#### Reflection也就是反射是Java被视为动态(或准动态)语言的一个性质
#### 反射机制指的是程序在运行时能够获取任何类的内部所有信息，并对其操作(包括私有属性)

## class对象概述
#### class其实就是类的类型
#### 字符串类型就是String，整形类型就是Integer，String和Integer类型就是Class

| 方法名 | 作用 |
|:--------|:-------|
|getName()|获得类中完整名称|
|----
|getDeclaredFields()|获得类中所有属性|
|----
|getDeclareMethods()|获得类中所有方法|
|----
|getConstructors()|获得类构造方法|
|----
|newInstance()|实例化类对象|
|----
|=====
{: rules="groups"}


#### 获得类对象的三种方法
{% highlight java %}
Class c1=null;
//1.通过Class的静态forName()方法，参数为完成类路径
c1=Class.forName("com.cheng.chapter01.Person");
//2.通过累的实例化对象
Person p=new Person();
Object o=p;
//3.
c1=Person.class();
{% endhighlight java %}

## Field
#### java.lang.reflect.Field类，是用于表示类中、接口中属性对象的类
#### 可以操作类中私有，以及共有等全部属性和属性的信息

| 方法名 | 作用 |
|:--------|:-------|
|getName()|获得属性名称|
|----
|getType()|获得属性类型|
|----
|get(Object obj)|获得obj对象这个属性值|
|----
|set(Object obj,Object value)|向obj对象中这个属性赋值value|
|----
|setAccessible(true)|启用/禁用访问控制权|
|----
|=====
{: rules="groups"}

#### 获取类中属性
{% highlight java %}
Person pp=new Person();
//1.getDeclaredFields()可以将私有属性获取到
Field[] ff=pp.getClass().getDeclaredFields();
//2.getFields()只能获得公有属性
//Field[] ff=pp.getClass().getFields();
for(Field fi :ff){
	System.out.println(fi.getName());
	System.out.println(fi.getType());
	fi.setAccessible(true);//get()无法获取私有类，需要启用访问权限
	System.out.println(fi.get(p));
}
{% endhighlight java %}


## Method
#### java.lang.reflect.Method类用于表示类中、接口中方法对象的类
#### 可以操作类中私有、以及公共等全部方法

| 方法名 | 作用 |
|:--------|:-------|
|getName()|获得方法名称|
|----
|getReturnType()|获得方法返回值类型|
|----
|invoke(Object obj,Object... args)|利用obj对象调用该方法|
|----
|getParamterTypes()|获得方法所有参数类型，按照顺序返回Class数组getDeclaredAn|
|----
|getDeclaredAnnotations()|获取方法的全部注解|
|----
|=====
{: rules="groups"}

#### 获取类中方法
{% highlight java %}
Product p=new Product();
	Class c=p.getClass();
	Method[] m=c.getDeclaredMethods();
	for(Method i:m){
	System.out.println("方法名"+i.getName());
	System.out.println("方法修饰符"+Modifier.toString(i.getModifiers()));
	System.out.println("返回值类型"+i.getReturnType());
	Class[] preType=i.getParameterTypes();
	System.out.println("方法参数列表");
	for(Class cl:preType){
		System.out.println(cl.getName());
	}
}
{% endhighlight java %}
## 利用反射机制实现Excel导入导出
#### 如果有一个图书管理系统的项目，要求实现图书信息导入导出、用户信息导入导出、借阅信息导入导出、维护信息导入导出、管理员信息导入导出等等，一个导入导出只需要两个方法，如果这么多都要写的话就显得有点浪费精力与时间了，那么我们可以利用反射来编写一个万能的导入导出方法
#### ExcleUtil.java
{% highlight java %}
package test.util;
import java.io.File;
import java.lang.reflect.Field;
import java.util.ArrayList;
import jxl.Sheet;
import jxl.Workbook;
import jxl.write.Label;
import jxl.write.WritableSheet;
import jxl.write.WritableWorkbook;

import test.Book;

public class ExcleUtil {

	public static void excleOut(ArrayList arr,String str){
		WritableWorkbook book=null;
		try {
			book=Workbook.createWorkbook(new File(str));
			WritableSheet sheet=book.createSheet("sheet", 0);
			for(int i=0;i<arr.size();i++){
				Object obj=arr.get(i);
				Class cla=obj.getClass();
				Field[] field=cla.getDeclaredFields();
				for(int j=0;j<field.length;j++){
					field[j].setAccessible(true);
					Label label=new Label(j, i, String.valueOf(field[j].get(obj)));
					sheet.addCell(label);
				}
			}
			book.write();
		} catch (Exception e) {
			e.printStackTrace();
		}finally {
			try {
				book.close();
			} catch (Exception e) {
				e.printStackTrace();
			}
		}

	}

	public static ArrayList excleIn(Class cla,String str){
		ArrayList arr=new ArrayList();
		Workbook book=null;
		try {
			book=Workbook.getWorkbook(new File(str));
			Sheet sheet=book.getSheet(0);
			Field[] fields=cla.getDeclaredFields();
			for(int i=0;i<sheet.getRows();i++){
				Object obj=cla.newInstance();//创建实例化对象
				for(int j=0;j<fields.length;j++){
					fields[j].setAccessible(true);
					String string=sheet.getCell(j,i).getContents();
					if(fields[j].getType().toString().equals("class java.lang.String")){
						fields[j].set(obj, string);
					}else if(fields[j].getType().toString().equals("int")){
						fields[j].setInt(obj, Integer.valueOf(string));
					}
				}
				arr.add(obj);
			}
		} catch (Exception e) {
			e.printStackTrace();
		}finally {
			book.close();
		}
		return arr;

	}

	public static void main(String[] args) {
//		ArrayList<Book> books=new ArrayList<Book>();
//		Book b1=new Book(1,"Java程序设计","学习");
//		Book b2=new Book(2,"天局","小说");
//		Book b3=new Book(3,"未来简史","社科");
//		Book b4=new Book(4,"苏菲的世界","哲学");
//		books.add(b1);
//		books.add(b2);
//		books.add(b3);
//		books.add(b4);
//		ExcleUtil.excleOut(books, "e:/books.xls");


		ArrayList<Book> array=ExcleUtil.excleIn(Book.class,"e:/books.xls" );
		for(Book b:array){
			System.out.println(b.getId()+b.getName()+b.getType());
		}
	}
}
{% endhighlight java %}