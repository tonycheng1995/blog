---
layout: post
title: "Android读取Assets文件中的数据"
date:   2017-02-26 12:17:58 +0800
toc: true
category: Android
tags: Android 数据存取
excerpt: 编写一个应用，不可避免的我们可能会用到数据存取，这篇文章讲解在assets中存取数据。
---
#### 在项目目录/src/java/添加assets目录
#### 在(项目名).iml文件中添加设置
    <option name="ASSEST_FOLDER_RELATIVE_PATH" value="/src/main/assets"/>


#### 在assets文件夹下建立一个文本文件info.txt,并写入一些文字

#### 建立布局文件activity_test.xml和相应的Java文件TestActivity.java
#### activity_test.xml
{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_test"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="com.example.cheng.myapplication.TestActivity">

    <Button
        android:id="@+id/readTxtBtn"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="按钮"/>

</LinearLayout>
{% endhighlight xml %}

#### TestActivity.java
{% highlight java %}
package com.example.cheng.myapplication;

import android.app.Activity;
import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;

public class TestActivity extends Activity {
    private Button myButton;
    public static final String TAG="ReadAssets";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_test);

        myButton=(Button)findViewById(R.id.readTxtBtn);

        myButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                try {
                    InputStream is=getResources().getAssets().open("info.txt");     //读取文件资源
                    InputStreamReader isr=new InputStreamReader(is,"UTF-8");        //字节流。编码要与文本文件一致，否则可能导致乱码
                    BufferedReader br=new BufferedReader(isr);                //缓冲流
                    String str="";
                    while ((str=br.readLine())!=null){
                        Log.i(TAG,str);
                    }
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        });
    }
}
{% endhighlight java %}