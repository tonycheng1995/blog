---
layout: post
title: "多线程下载"
date:   2018-10-10 +0800
toc: true
category: thread
tags: 多线程
excerpt: 本篇文章主要讲解Java中使用多线程进行文件的下载
---

#### 下载线程DownloadRunnable.java
{% highlight java %}
import java.io.File;
import java.io.IOException;
import java.io.InputStream;
import java.io.RandomAccessFile;
import java.net.HttpURLConnection;
import java.net.URL;
public class DownloadRunnable implements Runnable{
	private int total=0;//标识下载进度
	private int begin;//开始位置
	private int end;//结束位置
	private File file;
	private URL url;
	private int id;
	public DownloadRunnable(int begin, int end, File file, URL url, int id) {
		this.begin = begin;
		this.end = end;
		this.file = file;
		this.url = url;
		this.id = id;
	}
	@Override
	public void run() {
		try {
			HttpURLConnection conn = (HttpURLConnection) url.openConnection();
			conn.setRequestMethod("GET");
			conn.setRequestProperty("User-Agent","Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:61.0) Gecko/20100101 Firefox/61.0");
			conn.setRequestProperty("Range","bytes="+begin+"-"+end);
			InputStream is=conn.getInputStream();
				byte[] buf=new byte[1024*1024];
				RandomAccessFile randomFile=new RandomAccessFile(file,"rw");
				randomFile.seek(begin);
				int len=0;
				while((len=is.read(buf))!=-1){
				randomFile.write(buf,0,len);
				updateProgress(len);
				System.out.println("下载进度"+total);
				}
				is.close();
				randomFile.close();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
	synchronized private void updateProgress(int add) {
		total+=add;
	}
}
{% endhighlight java %}

#### 下载文件类DownTest.java
{% highlight java %}
import java.io.File;
import java.io.IOException;
import java.io.RandomAccessFile;
import java.net.HttpURLConnection;
import java.net.MalformedURLException;
import java.net.URL;
public class DownTest {
	public void down() {
		new Thread(new Runnable() {
			@Override
			public void run() {
				try {
					URL url=new URL("https://files.cnblogs.com/files/han-1034683568/%E9%98%BF%E9%87%8C%E5%B7%B4%E5%B7%B4Java%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C%E7%BB%88%E6%9E%81%E7%89%88v1.3.0.pdf");
					HttpURLConnection conn=(HttpURLConnection) url.openConnection();
					conn.setRequestMethod("GET");
					conn.setConnectTimeout(5000);
					conn.setRequestProperty("User-Agent", "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:61.0) Gecko/20100101 Firefox/61.0");
					int length=conn.getContentLength();
					if(length<0) {//文件不存在
						return;
					}
					File file=new File("d:\\", getFileName("https://files.cnblogs.com/files/han-1034683568/阿里巴巴Java开发手册终极版v1.3.0.pdf"));
					RandomAccessFile randomFile=new RandomAccessFile(file, "rw");
					randomFile.setLength(length);
					int blockSize=length/3;
					for (int i = 0; i < 3; i++) {
						int begin=i*blockSize;
						int end=(i+1)*blockSize;
						if(i==2) {
							end=length;
						}
						//创建新的线程,下载文件
						Thread t=new Thread(new DownloadRunnable(begin, end, file, url, i));
						t.start();
					}
				} catch (MalformedURLException e) {
					e.printStackTrace();
				} catch (IOException e) {
					e.printStackTrace();
				}
			}
		}).start();
	}
	//获取文件名
	public String getFileName(String url) {
		int index=url.lastIndexOf("/")+1;
		return url.substring(index);
	}
}
{% endhighlight java %}

#### 主方法类Main.java
{% highlight java %}
public class Main {
	public static void main(String[] args) {
		DownTest downTest=new DownTest();
		downTest.down();
	}
}
{% endhighlight java %}