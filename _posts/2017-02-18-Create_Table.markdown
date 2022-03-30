---
layout: post
title: "Android自动生成表格"
date: 2017-02-18 20:32:34 +0800
category: Android
toc: true
tags: Android
excerpt: 本篇文件讲解如何生成一张表格
---
#### 在开发中我们经常会根据需求需要自动生成一张表格，那么下面就说一下如何做。

#### 布局文件activity_main.xml
{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_main_margin"
    android:paddingRight="@dimen/activity_main_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="com.example.cheng.myapplication.MainActivity">

    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="XX大学学生成绩单"
            android:textSize="30sp"
            android:textColor="@color/colorPrimaryDark"/>

    </LinearLayout>

    <TableLayout
        android:id="@+id/id_tableLayout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="#000000">

        <TableRow
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_margin="0.5dip"
            android:background="#000000">

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_margin="1dip"
                android:background="#ffffff"
                android:layout_weight="1"
                android:gravity="center"
                android:text="课程"
                android:textSize="20dp"
                android:textStyle="bold"/>

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_margin="1dip"
                android:background="#ffffff"
                android:layout_weight="1"
                android:gravity="center"
                android:text="修习类别"
                android:textSize="20dp"
                android:textStyle="bold"/>

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_margin="1dip"
                android:background="#ffffff"
                android:layout_weight="1"
                android:gravity="center"
                android:text="学分"
                android:textSize="20dp"
                android:textStyle="bold"/>

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_margin="1dip"
                android:background="#ffffff"
                android:layout_weight="1"
                android:gravity="center"
                android:text="成绩"
                android:textSize="20dp"
                android:textStyle="bold"/>

            <TextView
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_margin="1dip"
                android:background="#ffffff"
                android:layout_weight="1"
                android:gravity="center"
                android:text="绩点"
                android:textSize="20dp"
                android:textStyle="bold"/>

        </TableRow>

    </TableLayout>

</LinearLayout>
{% endhighlight xml %}

#### 资源文件/drawable/shape.xml
{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android">
    <gradient
        android:startColor="#ffffff"
        android:endColor="#ffffff"
        android:angle="0"/>
    <stroke
        android:width="0.5dp"
        android:color="#dedcd2"/>
    <corners
        android:radius="2dp"/>
    <padding
        android:left="10dp"
        android:top="10dp"
        android:right="10dp"
        android:bottom="10dp"/>
</shape>
{% endhighlight xml %}

#### Java文件MainActivity.Java
{% highlight java %}
package com.example.cheng.myapplication;

import android.app.Activity;
import android.graphics.Color;
import android.os.Bundle;
import android.view.ViewGroup;
import android.widget.LinearLayout;
import android.widget.TableLayout;
import android.widget.TableRow;
import android.widget.TextView;

public class MainActivity extends Activity {

    private final int WC= ViewGroup.LayoutParams.WRAP_CONTENT;
    private final int FP= ViewGroup.LayoutParams.WRAP_CONTENT;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        createTable();
    }

    public void createTable(){
        TableLayout tableLayout=(TableLayout)findViewById(R.id.id_tableLayout);
        tableLayout.setStretchAllColumns(true);     //设置指定列号的列属性为Stretchable

        for(int row=0;row<3;row++){     //产生的行数
            TableRow tableRow=new TableRow(MainActivity.this);
            tableRow.setBackgroundColor(Color.rgb(222, 220, 210));      //设置表格背景
            for (int col = 0; col < 5; col++) {     //产生的列数
                TextView tv=new TextView(MainActivity.this);
                tv.setBackgroundResource(R.drawable.shape);     //设置背景
                tv.setText("测试");
                tableRow.addView(tv);
            }
            tableLayout.addView(tableRow,new LinearLayout.LayoutParams(FP, WC));
        }
    }

}
{% endhighlight java %}

#### 效果如图
<img src="{{base.url}}/img/table.jpg" >
