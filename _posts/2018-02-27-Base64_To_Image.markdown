---
layout: post
title: "Base64与图片之间的转换"
date:   2018-02-27 +0800
toc: true
category: java
tags: Base64 Java
excerpt: 本篇文章主要讲解Base64与图片之间的转换。
---
#### 要使用base64需要配置sun.misc.BASE64Decoder包，Eclipse自动配置的JDK是不存在jar包的，需要自己配置引进
#### 右击工程>properties>Java Build Path>将配置的JRE System Library移除，并Add Library..添加JRE System Library

#### Base64与图片之间的转换
{% highlight java %}
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import sun.misc.BASE64Decoder;
import sun.misc.BASE64Encoder;
public class Base64ToImage {
	public void base64ToImage(String base64Str,String imgFilePath) {
		BASE64Decoder decoder=new BASE64Decoder();
		try {
			byte[] b=decoder.decodeBuffer(base64Str);
			for(int i=0;i<b.length;i++) {
				if(b[i]<0) {
					//调整异常数据
					b[i]+=256;
				}
			}
			OutputStream outputStream=new FileOutputStream(imgFilePath);
			outputStream.write(b);
			outputStream.flush();
			outputStream.close();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}

	}

	public void ImageToBase64(String imgFilePath) {
		InputStream inputStream=null;
		byte[] data=null;
		try {
			inputStream = new FileInputStream(imgFilePath);
			data=new byte[inputStream.available()];
			inputStream.read(data);
			inputStream.close();
		} catch (FileNotFoundException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		BASE64Encoder encoder=new BASE64Encoder();
		String base64Str=encoder.encode(data);
		System.out.println(base64Str);
	}
}
{% endhighlight java %}
		
		
#### 测试类
{% highlight java %}
public class Test {
	public static void main(String[] args) throws IOException {
		QueryPetsOnSale queryPetsOnSale=new QueryPetsOnSale();
		try {
				String base64Str="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";
				Base64ToImage base64ToImage=new Base64ToImage();
				base64ToImage.base64ToImage(base64Str,"E:\\new.img");
				base64ToImage.ImageToBase64("E:\\new.img");
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}
{% endhighlight java %}