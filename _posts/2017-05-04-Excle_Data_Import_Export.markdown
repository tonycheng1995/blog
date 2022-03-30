---
layout: post
title: "Excel数据导入导出"
date:   2017-05-04 +0800
toc: true
category: Excel
tags: Java
excerpt: 本篇文章主要讲解如何将数据从Excel导入导出
---
#### 1.下载jxl.jar
#### 2.编写代码
#### Book.Java
{% highlight java %}
package test;
public class Book {
	private int id;
	private String name;
	private String type;

	public Book() {
		super();
	}

	public Book(int id, String name, String type) {
		this.id = id;
		this.name = name;
		this.type = type;
	}

	public int getId() {
		return id;
	}
	public void setId(int id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getType() {
		return type;
	}
	public void setType(String type) {
		this.type = type;
	}

}
{% endhighlight java %}
#### 将Object对象数据写入xls
#### ExcelBook.java
{% highlight java %}
package test;
import java.io.File;
import java.util.ArrayList;
import jxl.Workbook;
import jxl.write.Label;
import jxl.write.WritableSheet;
import jxl.write.WritableWorkbook;
public class ExcelBook {

	public static void main(String[] args) {
		ExcelBook eb=new ExcelBook();
		ArrayList<Book> books=new ArrayList<Book>();
		Book b1=new Book(1,"Java程序设计","学习");
		Book b2=new Book(2,"天局","小说");
		Book b3=new Book(3,"未来简史","社科");
		Book b4=new Book(4,"苏菲的世界","哲学");
		books.add(b1);
		books.add(b2);
		books.add(b3);
		books.add(b4);
		eb.excelOut(books);
	}

	public void excelOut(ArrayList<Book> books){
		WritableWorkbook book=null;//Excel对象
		try {
			//创建excel对象
			book=Workbook.createWorkbook(new File("e:/book.xls"));
			//通过excel对象创建一个选项卡对象
			WritableSheet sheet=book.createSheet("sheet1", 0);
			//创建一个单元格对象(列，行，值)
			//Label la=new Label(0, 2, "text");
			for(int i=0;i<books.size();i++){
				Book b=books.get(i);
				//创建一个单元格对象(列，行，值)
				Label l1=new Label(0, i, String.valueOf(b.getId()));
				Label l2=new Label(1, i,b.getName());
				Label l3=new Label(2, i, b.getType());
				//将创建好的单元格对象放入选项卡中
				sheet.addCell(l1);
				sheet.addCell(l2);
				sheet.addCell(l3);
			}

			//将创建好的单元格对象放入选项卡中
			//sheet.addCell(la);
			//写入目标路径
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

}
{% endhighlight java %}

#### 3.运行程序可以在E盘下生成了一个book.xls，打开文件可以看到填入了相应数据
![]({{site.url}}/img/excel01.png)
#### 从xls获取数据到Object对象
#### ExcelBook.java
{% highlight java %}
package test;
import java.io.File;
import java.util.ArrayList;
import jxl.Cell;
import jxl.Sheet;
import jxl.Workbook;
import jxl.write.Label;
import jxl.write.WritableSheet;
import jxl.write.WritableWorkbook;
public class ExcelBook {

	public static void main(String[] args) {
		ExcelBook eb=new ExcelBook();
		ArrayList<Book> array=eb.excelIn();
		for(Book b:array){
			System.out.println(b.getId()+b.getName()+b.getType());
		}
	}

	private ArrayList<Book> excelIn(){
		ArrayList<Book> books=new ArrayList<Book>();
		Workbook book=null;
		try {
			book=Workbook.getWorkbook(new File("e:/book.xls"));
			Sheet sheet=book.getSheet(0);
			for(int i=0;i<sheet.getRows();i++){
				Book b=new Book();
				Cell cell=sheet.getCell(0, i);
				b.setId(Integer.valueOf(cell.getContents()));
				b.setName(sheet.getCell(1,i).getContents());
				b.setType(sheet.getCell(2,i).getContents());
				books.add(b);
			}

		} catch (Exception e) {
			e.printStackTrace();
		}finally {
			book.close();
		}
		return books;
	}

}
{% endhighlight java %}

#### 运行程序，在控制台输出从Xls中获取的数据
#### 附：[jxl.jar包](http://pan.baidu.com/s/1eR18Baq) 密码：831k
