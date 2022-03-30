---
layout: post
title: "多线程断点续传"
date:   2018-10-11 +0800
toc: true
category: thread
tags: 多线程 断点续传
excerpt: 本篇文章主要讲解如何编写多线程断点续传的功能实现。
---
#### 多线程多线续传只需要在多线程的基础上增加一个记录线程的下载进度。
#### 只需要在多线程下载的基础上修改DowmTest.java文件和DownloadRunnable.java
#### DownloadRunnable.java
{% highlight java %}
public class DownloadRunnable implements Runnable{
	private int total=0;//标识下载进度
	private int begin;
	private int end;
	private File file;
	private URL url;
	private int id;
	private boolean downloading;
	private List<HashMap<String, Integer>> threadList;
	public DownloadRunnable(int begin, int end, File file, URL url, int id,List<HashMap<String, Integer>> threadList,boolean downloading) {
		this.begin = begin;
		this.end = end;
		this.file = file;
		this.url = url;
		this.id = id;
		this.threadList=threadList;
		this.downloading=downloading;
	}
	@Override
	public void run() {
		if(begin>end) {
			return;
		}
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
				HashMap<String, Integer> map=threadList.get(id);
				while((len=is.read(buf))!=-1&&downloading==true){
				randomFile.write(buf,0,len);
				updateProgress(len);
				map.put("finished", map.get("finished")+len);
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

#### DownTest.java
{% highlight java %}
public class DownTest {
	private boolean downloading=false;//标识是否在下载
	private URL url;
	private File file;
	private List<HashMap<String, Integer>> threadList;
	public void down() {
		if(downloading) {//正在下载时,变为暂停
			downloading=false;
			return;
		}
		downloading=true;//变为下载
		if(threadList.size()==0) {//开始线程
			new Thread(new Runnable() {
				@Override
				public void run() {
					try {
						url=new URL("https://files.cnblogs.com/files/阿里巴巴Java开发手册终极版v1.3.0.pdf");
						HttpURLConnection conn=(HttpURLConnection) url.openConnection();
						conn.setRequestMethod("GET");
						conn.setConnectTimeout(5000);
						conn.setRequestProperty("User-Agent", "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:61.0) Gecko/20100101 Firefox/61.0");
						int length=conn.getContentLength();
						if(length<0) {//文件不存在
							return;
						}
						file=new File("d:\\", getFileName("https://files.cnblogs.com/files/阿里巴巴Java开发手册终极版v1.3.0.pdf"));
						RandomAccessFile randomFile=new RandomAccessFile(file, "rw");
						randomFile.setLength(length);
						int blockSize=length/3;
						for (int i = 0; i < 3; i++) {
							int begin=i*blockSize;
							int end=(i+1)*blockSize;
							if(i==2) {
								end=length;
							}
							HashMap<String, Integer> map=new HashMap<String,Integer>();
							map.put("begin", begin);
							map.put("end", end);
							map.put("finished", 0);
							threadList.add(map);
							//创建新的线程,下载文件
							Thread t=new Thread(new DownloadRunnable(begin, end, file, url, i,threadList,downloading));
							t.start();
						}
					} catch (MalformedURLException e) {
						e.printStackTrace();
					} catch (IOException e) {
						e.printStackTrace();
					}
				}
			}).start();
		}else {//恢复线程
				for(int i=0;i<threadList.size();i++){
					HashMap<String,Integer> map=threadList.get(i);
					int begin=map.get("begin");
					int end=map.get("end");
					int finished=map.get("finished");
					Thread t=new Thread(new DownloadRunnable(begin+finished,end,file,url,i,threadList,downloading));
					t.start();
					}
		}
	}
	//获取文件名
	public String getFileName(String url) {
		int index=url.lastIndexOf("/")+1;
		return url.substring(index);
	}
}
{% endhighlight java %}