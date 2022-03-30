---
layout: post
title: "Android存取内部存储文件"
date:   2017-02-26 12:27:58 +0800
toc: true
category: Android
tags: Android 数据存取
excerpt: 编写一个应用，不可避免的我们可能会用到数据存取，这篇文章讲解在手机内部存储中存取数据。
---
#### 运行Android模拟器
#### 打开Explorer模块。Tools>Android>Android Devide Monitor>Window>Show View>Android>File>Explorer
#### 打开之后你可能看到什么也没有，那是因为没有获得所需要权限，怎么办，换一个低版本的模拟器
#### Explorer>data>data,在此文件夹下可以看到我们的项目的包
![]({{site.url}}/img/File_Explorer.png)


#### activuty_test.xml
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

    <EditText
        android:id="@+id/editText"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="请输入内容"/>

    <Button
        android:id="@+id/writeBtn"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="保存数据"/>

    <Button
        android:id="@+id/readBtn"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="读取数据"/>

    <TextView
        android:id="@+id/show"
        android:layout_width="match_parent"
        android:layout_height="wrap_content" />

</LinearLayout>
{% endhighlight xml %}

#### TestActivity.java
{% highlight java %}
package com.example.cheng.myapplication;

import android.app.Activity;
import android.content.Context;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.io.UnsupportedEncodingException;

public class TestActivity extends Activity {
    private String fileName="test";
    private EditText editText;
    private TextView show;
    private Button writeButton;
    private Button readButton;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_test);
        editText=(EditText)findViewById(R.id.editText);
        show=(TextView)findViewById(R.id.show);
        readButton=(Button)findViewById(R.id.readBtn);
        writeButton=(Button)findViewById(R.id.writeBtn);

        writeButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                try {
                    FileOutputStream fos=openFileOutput(fileName, Context.MODE_PRIVATE);     //将文件数据固定的输入到应用程序的内部存储
                    OutputStreamWriter osw=new OutputStreamWriter(fos,"UTF-8");
                    osw.write(editText.getText().toString());
                    osw.flush();
                    fos.flush();
                    osw.close();
                    fos.close();
                    Toast.makeText(getApplicationContext(),"写入完成",Toast.LENGTH_SHORT).show();
                } catch (FileNotFoundException e) {
                    e.printStackTrace();
                } catch (UnsupportedEncodingException e) {
                    e.printStackTrace();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        });

        readButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                try {
                    FileInputStream fis=openFileInput(fileName);
                    InputStreamReader isr=new InputStreamReader(fis,"UTF-8");
                    char input[]=new char[fis.available()];     //构建字符数组
                    isr.read(input);
                    isr.close();
                    fis.close();
                    String str=new String(input);
                    show.setText(str);
                } catch (FileNotFoundException e) {
                    e.printStackTrace();
                } catch (UnsupportedEncodingException e) {
                    e.printStackTrace();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        });
    }
}
{% endhighlight java %}
#### 运行程序，输入文本，点击保存数据，打开File Explorer>date>date>项目包>file下看见创建的test文件
#### 运行程序，点击读取数据，可以看到上次保存的文本显示到了界面上
