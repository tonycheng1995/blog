---
layout: post
title: "HttpClient登录绕过证书验证"
date:   2017-05-28 +0800
toc: true
category: Java
tags: Java
excerpt: 本篇文章主要讲解HttpClient登录绕过证书验证
---
#### 如图，是个使用了证书验证的网站，当我们点击登录时登录会弹出证书验证
![](/img/Pass_Cer_Vali01.png)
#### 当我们使用java代码模拟登录时，会报如下错误
![](/img/Pass_Cer_Vali02.png)
#### 对于需要证书验证的登录，1.加载证书、2.绕过证书
#### 那么下面这段代码就是用来绕过证书验证的
{% highlight java %}
/**
	* 绕过证书验证
	*/
public static String send(String url, List<NameValuePair> params, String encoding)
		throws KeyManagementException, NoSuchAlgorithmException, ClientProtocolException, IOException {
	String body = "";
	// 采用绕过验证的方式处理https请求
	SSLContext sslcontext = createIgnoreVerifySSL();

	// 设置协议http和https对应的处理socket链接工厂的对象
	Registry<ConnectionSocketFactory> socketFactoryRegistry = RegistryBuilder.<ConnectionSocketFactory>create()
			.register("http", PlainConnectionSocketFactory.INSTANCE)
			.register("https", new SSLConnectionSocketFactory(sslcontext)).build();
	PoolingHttpClientConnectionManager connManager = new PoolingHttpClientConnectionManager(socketFactoryRegistry);
	HttpClients.custom().setConnectionManager(connManager);

	// 创建自定义的httpclient对象
	CloseableHttpClient client = HttpClients.custom().setConnectionManager(connManager).build();

	// 创建post方式请求对象
	HttpPost httpPost = new HttpPost(url);

	// 装填参数

	// 设置参数到请求对象中
	httpPost.setEntity(new UrlEncodedFormEntity(params, encoding));

	System.out.println("请求地址：" + url);
	System.out.println("请求参数：" + params.toString());

	// 设置header信息
	// 指定报文头【Content-type】、【User-Agent】
	httpPost.setHeader("Content-type", "application/x-www-form-urlencoded");
	httpPost.setHeader("User-Agent", "Mozilla/4.0 (compatible; MSIE 5.0; Windows NT; DigExt)");

	// 执行请求操作，并拿到结果（同步阻塞）
	CloseableHttpResponse response = client.execute(httpPost);
	// 获取结果实体
	HttpEntity entity = response.getEntity();
	if (entity != null) {
		// 按指定编码转换结果实体为String类型
		body = EntityUtils.toString(entity, encoding);
	}
	EntityUtils.consume(entity);
	// 释放链接
	response.close();
	return body;
}

public static SSLContext createIgnoreVerifySSL() throws NoSuchAlgorithmException, KeyManagementException {
	SSLContext sc = SSLContext.getInstance("SSLv3");

	// 实现一个X509TrustManager接口，用于绕过验证，不用修改里面的方法
	X509TrustManager trustManager = new X509TrustManager() {
		@Override
		public void checkClientTrusted(java.security.cert.X509Certificate[] paramArrayOfX509Certificate,
				String paramString) throws CertificateException {
		}

		@Override
		public void checkServerTrusted(java.security.cert.X509Certificate[] paramArrayOfX509Certificate,
				String paramString) throws CertificateException {
		}

		@Override
		public java.security.cert.X509Certificate[] getAcceptedIssuers() {
			return null;
		}
	};

	sc.init(null, new TrustManager[] { trustManager }, null);
	return sc;
}
{% endhighlight java %}