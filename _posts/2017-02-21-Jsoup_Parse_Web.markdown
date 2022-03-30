---
layout: post
title: "Jsoup解析网页"
date:   2017-02-21 21:20:58 +0800
toc: true
category: Jsoup
tags: Jsoup 爬虫
excerpt: 在爬取解析网页方面，Jsoup是一个不错的Java包，所以学习Jsoup的用处很有必要。
---
#### 从一个URL解析HTML
{% highlight java %}
String url="http://www.baidu.com";
Document doc=Jsoup.connect(url).get();  //解析的结果是一个文档对象

Document doc=Jsoup.parse(html);   //创建一个干净的解析结果，无论HTML格式是否完整
{% endhighlight java %}
#### 设置参数链接
{% highlight java %}
Document doc=Jsoup.connect(url)
            .data("query","java")           //请求参数
            .userAgent("jsoup")             //设置User-Agent
            .cookie("auth","token")         //设置cookie
            .timeout(3000)                  //设置连接超时时间
            .post();                        //使用POST方式访问URL
{% endhighlight java %}

#### 获取网页标题
{% highlight java %}
String title=doc.title();         //获取网页标题
{% endhighlight java %}

#### 获取网页中的链接
{% highlight java %}
Elements link=doc.select("a[href]");        //获取带有href属性的a标签
Elements link=doc.select("a[name~=th]");    //获取a标签中的name属性带有“th”的链接
/*完整例子*/
String url="http://www.baidu.com";
Document doc=Jsoup.connect(url),get();      
Elements links=doc.select("a[href]");         //获取带有href属性的a标签
for(Elements link : links){                   //遍历每个链接
    String linkHref=link.attr("href");          //得到href属性的值，即URL地址
    String linkText=link.text();                //获取链接文本
    System.out.println(linkText+" "+linkHref);  //输出链接文本及地址
}
{% endhighlight java %}

#### 利用Dom获取数据
{% highlight java %}
Element content=doc.getElementById("sh");       //通过id获取对应的元素
Elements links=content.getElementsByTag("a");   //通过标签获取元素
for(Element link : links){              
    String linkHref=link.attr("href");
    String linkText=link.text();
}
{% endhighlight java %}
#### 获取class为Wrapper的Div区域的内容
{% highlight java %}
Element content=doc.select("div.Wrapper").first();
{% endhighlight java %}

#### 根据id获取内容
{% highlight java %}
Element content=doc.select("#set").first();
{% endhighlight java %}

#### 去掉注释
#### "<!-- --\>"是网页中的注释标签。注释是节点，用节点名#comment标识
{% highlight java %}
removeComments(doc);
System.out.print(doc.html)
public static void removeComments(Node node){
    for(int i=0;i<node.childNodes().size();i++){
        Node child=node.childNode(i);
        if(child.nodeName().equals("#comment")){
            child.remove();
        }else {
            removeComments(child);
        }
    }
}
{% endhighlight java %}