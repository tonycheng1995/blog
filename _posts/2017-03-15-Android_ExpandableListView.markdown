---
layout: post
title: "Android之折叠列表ExpandableListView"
date: 2017-03-15 23:49:00 +0800
toc: true
category: Android
tags: Android
excerpt: 腾讯QQ我们都不陌生，但是QQ列表的好友分组是怎么弄的呢？本篇文章就来说一说折叠列表的事儿。
---

#### android:groupIndicator用来设置组列表项的图标，默认一个三角线，这里我们设置为'@null',然后自定义显示图标。
#### activity_main.xml
{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context="com.example.cheng.myapplication.MainActivity">

    <ExpandableListView
        android:id="@+id/elv"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:groupIndicator="@null" />

</LinearLayout>
{% endhighlight xml %}
#### MainActivity.java
{% highlight java %}
package com.example.cheng.myapplication;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.Gravity;
import android.view.View;
import android.view.ViewGroup;
import android.widget.AbsListView;
import android.widget.BaseExpandableListAdapter;
import android.widget.ExpandableListAdapter;
import android.widget.ExpandableListView;
import android.widget.ImageView;
import android.widget.LinearLayout;
import android.widget.TextView;
import android.widget.Toast;

public class MainActivity extends AppCompatActivity {

    private ExpandableListView elv;
    private ImageView imageView;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        //创建BaseExpandableListAdapter对象
        ExpandableListAdapter ela=new BaseExpandableListAdapter() {
            private String[] groupTypes=new String[]{"歌手","演员","作家"};
            private String[][]  groups=new String[][]{
                    {"陈奕迅","周杰伦","李玟"},
                    {"刘诗诗","唐嫣","张国立","沙溢"},
                    {"大冰","席慕蓉","张嘉佳"}
            };

            @Override
            public int getGroupCount() {//返回包含的组列表项的数量
                return groupTypes.length;
            }

            @Override
            public int getChildrenCount(int groupPosition) {//返回特定组所包含的子列表项的数量
                return groups[groupPosition].length;
            }

            @Override
            public Object getGroup(int groupPosition) {  //获取指定组位置处的组数据
                return groupTypes[groupPosition];
            }

            @Override
            public Object getChild(int groupPosition, int childPosition) {
                return groups[groupPosition][childPosition];
            }

            @Override
            public long getGroupId(int groupPosition) {
                return groupPosition;
            }

            @Override
            public long getChildId(int groupPosition, int childPosition) {
                return childPosition;
            }

            @Override
            public boolean hasStableIds() {
                return true;
            }

            private TextView getTextView(){
                AbsListView.LayoutParams lp=new AbsListView.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT,64);
                TextView textView=new TextView(MainActivity.this);
                textView.setLayoutParams(lp);
                textView.setGravity(Gravity.CENTER_VERTICAL|Gravity.LEFT);
                textView.setPadding(36,0,0,0);
                textView.setTextSize(20);
                return textView;
            }

            //设置每个组选项的外观
            @Override
            public View getGroupView(int groupPosition, boolean isExpanded, View convertView, ViewGroup parent) {//返回的View对象将作为组列表项
                LinearLayout ll=new LinearLayout(MainActivity.this);
                ll.setOrientation(LinearLayout.HORIZONTAL);
                imageView=new ImageView(MainActivity.this);
                if(isExpanded){//判断选项组状态是否是打开
                    imageView.setImageResource(R.drawable.triangle1);
                }else{
                    imageView.setImageResource(R.drawable.triangle0);
                }
                ll.addView(imageView);
                TextView textView=getTextView();
                textView.setText(getGroup(groupPosition).toString());
                ll.addView(textView);
                return ll;
            }
            //设置每个子选项的外观
            @Override
            public View getChildView(int groupPosition, int childPosition, boolean isLastChild, View convertView, ViewGroup parent) {//返回的View对象将作为特定组、特定位置的子列表项
                TextView textView=getTextView();
                textView.setText(getChild(groupPosition, childPosition).toString());
                return textView;
            }

            @Override
            public boolean isChildSelectable(int groupPosition, int childPosition) {
                return true;
            }
        };

        elv=(ExpandableListView)findViewById(R.id.elv);
        elv.setAdapter(ela);

        //为子列表设置点击监听
        elv.setOnChildClickListener(new ExpandableListView.OnChildClickListener() {
            @Override
            public boolean onChildClick(ExpandableListView parent, View v, int groupPosition, int childPosition, long id) {
                String str=groupPosition+"-"+childPosition+"被点击";
                Toast.makeText(MainActivity.this,str,Toast.LENGTH_SHORT).show();
                return false;
            }
        });
    }
}
{% endhighlight java %}

#### 效果如图
![]({{site.url}}/img/elv.gif)

#### 代码分享
#### 链接：[百度云盘](http://pan.baidu.com/s/1bph95d9) 密码：k5f7
