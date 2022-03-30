---
layout: post
title: "Android之下拉菜单Spinner"
date: 2017-03-14 19:11:00 +0800
toc: true
category: Android
tags: Android
excerpt: Spinner组件的作用是作为一个下拉菜单，提供选项供用户选择。
---
#### 首先看一下效果吧
![]({{site.url}}/img/spinner01.gif)

#### 1.固定选项
#### 如果我们已经确定了让用户选择的选项值，那么可以采取这种方式。
#### 编写布局文件main.xml
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
        android:text="你的职业："
        android:textSize="21sp"/>
    <Spinner
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:entries="@array/profession" />

</LinearLayout>
{% endhighlight xml %}
#### 编写数据资源arrays.xml
{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string-array name="profession" >
        <item>教师</item>
        <item>学生</item>
        <item>司机</item>
        <item>厨师</item>
        <item>工人</item>
    </string-array>
</resources>
{% endhighlight xml %}

#### 2.动态选项
#### 在实际环境下，也许我们会根据用户的操作，来提供不同的选项值来供用户选择，那么就应该选择这种方式。
#### 编写布局文件main.xml
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
        android:text="你的职业："
        android:textSize="21sp"/>
    <Spinner
        android:id="@+id/spinner"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />

</LinearLayout>
{% endhighlight xml %}

#### 编写Activity.java
{% highlight java %}
package com.example.cheng.myapplication;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.widget.ArrayAdapter;
import android.widget.Spinner;

public class MainActivity extends AppCompatActivity {

    Spinner spinner;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        spinner=(Spinner)findViewById(R.id.spinner);
        String profession[]=new String[]{"歌手","演员","律师","公务员","设计师"};
        //创建ArrayAdapter对象
        ArrayAdapter<String> aa=new ArrayAdapter<String>(this,android.R.layout.simple_spinner_dropdown_item,profession);
        //为Spinner设置Adapter
        spinner.setAdapter(aa);
    }
}
{% endhighlight java %}

#### 附：
#### 可能在一些老的教材或视频上会讲'prompt'设置选择框的提示信息，但是在新版本的默认状态下是不起作用的，你需要将style设置为‘Widge.Spinner’
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
        android:text="你的职业："
        android:textSize="21sp"/>
    <Spinner
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:entries="@array/profession"
        style="@android:style/Widget.Spinner"
        android:prompt="@string/tip"
        />

</LinearLayout>
{% endhighlight xml %}

#### 界面如图
![]({{site.url}}/img/spinner02.png)
#### 获取选项被选中的值：spinner.getSelectedItem().toString();
