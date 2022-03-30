---
layout: post
title: "Android存取SD卡中的数据"
date:   2017-02-26 12:30:58 +0800
toc: true
category: Android
tags: Android 数据存取
excerpt: 编写一个应用，不可避免的我们可能会用到数据存取，这篇文章讲解在外部SD卡中存取数据。
---
#### sdcard的文件路径，File Explorer>storage>sdcard
#### activity.xml
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
import android.os.Bundle;
import android.os.Environment;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;
import java.io.File;
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
    File sdcard= Environment.getExternalStorageDirectory();

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

                File myFile=new File(sdcard,"This is my File.txt");
                if(!sdcard.exists()){    //判断是否存在sd卡
                    Toast.makeText(getApplicationContext(),"当前系统不具备SD卡目录",Toast.LENGTH_SHORT).show();
                    return;
                }
                try {
                    myFile.createNewFile();
                    Toast.makeText(getApplicationContext(),"文件已经创建完成！",Toast.LENGTH_SHORT).show();
                    FileOutputStream fos=new FileOutputStream(myFile);
                    OutputStreamWriter osw=new OutputStreamWriter(fos);
                    osw.write(editText.getText().toString());
                    osw.flush();
                    osw.close();
                    fos.close();
                    Toast.makeText(getApplicationContext(),"文件已经写入完成！",Toast.LENGTH_SHORT).show();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        });

        readButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {

                File myFile=new File(sdcard,"This is my File.txt");
                if(sdcard.exists()) {    //判断是否存在sd卡
                    try {
                        FileInputStream fis=new FileInputStream(myFile);
                        InputStreamReader isr=new InputStreamReader(fis,"UTF-8");
                        char[] input=new char[fis.available()];
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
            }
        });
    }
}
{% endhighlight java %}

#### 在AndroidManifest.xml中添加权限
{% highlight xml %}
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS"/>
{% endhighlight xml %}

#### 运行程序，输入内容，点击保存数据，可以在File Explorer>storage>sdcard下创建了一个文件
![]({{site.url}}/img/File_Explorer02.png)
#### 运行程序，输入内容，点击读取数据，可以看到SD卡中文本的信息被读取到了界面中
