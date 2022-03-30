---
layout: post
title: "Android之文本提示AutoCompleteTextView"
date: 2017-03-15 09:35:00 +0800
toc: true
category: Android
tags: Android
excerpt: 搜索引擎大家都是用过的，当我们在搜索框输入内容时，它就会弹出一个下拉菜单提供选项供我们选择，我们只需要点击内容就能填到文本框内，当然了它提供的选项是与输入文本相关的内容，而我们这里做的是文本提示。
---

属性|方法|说明
:--|:--|:--
android:completionHint|setCompletionHint()|设置下拉菜单中的提示标题
android:completionThreshold|setThreshold()|设置至少输入几个字符才会提示
android:dropDownAnchor|setDropDownHeight()|设置下拉菜单的高度
android:popupBackground|setDropDownBackgroundResource()|设置背景

#### 布局文件main.xml
{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal"
    tools:context="com.example.cheng.myapplication.MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="请写出你喜欢的诗人："
        android:textSize="21sp"/>
    <AutoCompleteTextView
        android:id="@+id/auto"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:completionHint="--请选择--"
        android:completionThreshold="1"/>

</LinearLayout>
{% endhighlight xml %}

#### MainActivity.java
{% highlight xml %}
package com.example.cheng.myapplication;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.widget.ArrayAdapter;
import android.widget.AutoCompleteTextView;

public class MainActivity extends AppCompatActivity {

    AutoCompleteTextView actv;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        actv=(AutoCompleteTextView)findViewById(R.id.auto);
        String person[]=new String[]{"李白","苏辙","杜甫","苏轼","李清照","李贺","李商隐","苏洵"};
        //创建ArrayAdapter对
        ArrayAdapter<String> aa=new ArrayAdapter<String>(this,android.R.layout.simple_spinner_dropdown_item,person);
        //为Spinner设置Adapter
        actv.setAdapter(aa);
    }
}
{% endhighlight xml %}

#### 效果
![]({{site.url}}/img/actv.gif)

#### AutoCompleteTextView只能完成一次提示，而如果我们想为用户提供多次填写的提示时，就可以使用AutoCompleteTextView的子类：MultiAutoCompleteTextView，MultiAutoCompleteTextView允许输入多个提示项，它会自动添加分隔符来分割各项。
#### main.xml
{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal"
    tools:context="com.example.cheng.myapplication.MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="请写出你喜欢的诗人："
        android:textSize="21sp"/>

    <MultiAutoCompleteTextView
        android:id="@+id/multi"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:completionThreshold="1"/>

</LinearLayout>
{% endhighlight xml %}
#### MainActivity.java
{% highlight xml %}
package com.example.cheng.myapplication;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.widget.ArrayAdapter;
import android.widget.MultiAutoCompleteTextView;

public class MainActivity extends AppCompatActivity {

    MultiAutoCompleteTextView mactv;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mactv=(MultiAutoCompleteTextView) findViewById(R.id.multi);
        String person[]=new String[]{"李白","苏辙","杜甫","苏轼","李清照","李贺","李商隐","苏洵"};
        //创建ArrayAdapter对
        ArrayAdapter<String> aa=new ArrayAdapter<String>(this,android.R.layout.simple_spinner_dropdown_item,person);
        //为Spinner设置Adapter
        mactv.setAdapter(aa);
        //为MultiAutoCompleteTextView设置分隔符
        mactv.setTokenizer(new MultiAutoCompleteTextView.CommaTokenizer());
    }
}
{% endhighlight xml %}

#### 效果
![]({{site.url}}/img/mactv.gif)
