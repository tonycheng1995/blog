---
layout: post
title: "Android横竖屏切换"
date: 2017-03-01 20:35:00 +0800
toc: true
category: Android
tags: Android
excerpt: 在看视频的时候，为了更佳的观看效果，我们经常会将小屏幕放大横屏观看，那么横竖屏之间的切换是怎么完成的呢？我们来学习一下。
---
#### 新建一个项目，编辑布局文件
{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_sreen"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="com.example.cheng.myapplication.ScreenActivity">

    <Switch
        android:id="@+id/id_switch"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:thumb="@drawable/q"
        android:track="@drawable/p"
        android:minWidth="50dp"
        android:showText="true"
        android:textOn="竖屏"
        android:textOff="横屏" />

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="现在是竖屏"/>

</LinearLayout>
{% endhighlight xml %}

#### 复制一份布局文件，我们将文件的路径修改为layout-land，为了区分，我们对布局稍微做一下修改
<img src="{{base.url}}/img/screen.png"/>
{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_sreen"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="com.example.cheng.myapplication.ScreenActivity">

    <Switch
        android:id="@+id/id_switch"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:thumb="@drawable/q"
        android:track="@drawable/p"
        android:minWidth="50dp"
        android:showText="true"
        android:textOn="竖屏"
        android:textOff="横屏" />

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="现在是横屏"/>

</LinearLayout>
{% endhighlight xml %}

#### 当我们翻转屏幕的时候，我们可以看到界面的布局和文字发生了改变
<img src="{{base.url}}/img/screen01.gif"/>
#### 接下来我们对ScreeActivity.java类文件做下修改，使在切换按钮的情况下，布局发生改变
{% highlight java %}
package com.example.cheng.myapplication;

import android.app.Activity;
import android.os.Bundle;
import android.widget.CompoundButton;
import android.widget.LinearLayout;
import android.widget.Switch;
import android.widget.TextView;

public class ScreenActivity extends Activity {

    Switch mSwitch;
    TextView textView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_sreen);

        mSwitch=(Switch)findViewById(R.id.id_switch);
        textView=(TextView)findViewById(R.id.textView);
        final LinearLayout layout=(LinearLayout)findViewById(R.id.activity_sreen);

        mSwitch.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
            @Override
            public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
                if(isChecked){
                    layout.setOrientation(LinearLayout.VERTICAL);
                }else{
                    layout.setOrientation(LinearLayout.HORIZONTAL);
                }
            }
        });
    }
}
{% endhighlight xml %}

#### 切换按钮，我们可以看到界面的布局发生了变化
<img src="{{base.url}}/img/screen02.gif"/>
