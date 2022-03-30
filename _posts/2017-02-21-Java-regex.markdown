---
layout: post
title: "Java正则表达式"
date:   2017-02-21 21:17:58 +0800
toc: true
category: Java
tags: Java 爬虫
excerpt: 数据分析获取的时候我们难免要用到正则表达式，这里简单的介绍一下。
---
字符|描述
:--:|:--:
\ |将下一个字符标记为特殊字符、原义字符、向后引用或八进制转义符。
^ |匹配输入字符串的开始位置。
$ |匹配输入字符串的结束位置。
* |匹配前面的子表达式零次或多次。
+ |匹配前面的子表达式一次或多次。
? |匹配前面的子表达式零次或一次。当改字符紧跟在任何一个其它限制符(* ,+,?,{n},{n,},{n,m})后面时，匹配模式是非贪婪的。
{n} |n是一个非负整数，匹配确定的n次。
{n,}|n是一个非负整数，至少匹配n次。
{n,m}|n和m均为非负整数，最少匹配n次且最多匹配m次。
. |匹配除{\n}之外的任何单个字符。
x&#124;y|匹配x或y
[xyz]|字符集合，匹配所包含的任意字符
[^xyz]|负值字符集合，匹配未包含的任意字符
[a-z]|字符范围，匹配指定范围内的任意字符
[^a-z]|负值字符范围，匹配不在指定范围内的任意字符
\b|匹配一个单词边界，也就是指单词和空格的位置。
\B|匹配非单词边界
\cx|匹配由x指明的控制字符。
\d|匹配一个数字字符，等价于[0-9]
\D|匹配一个非数字字符，等价[^0-9]
\f|匹配一个换页符，等价于\x0c和\cL
\n|匹配一个换行符，等价于\x0a和\cJ
\r|匹配一个回车符，等价于\x0d和\cM
\s|匹配任何空白字符，包括空格、制表符、换页符等，等价于[\f\n\r\t\v]
\S|匹配任何非空白字符，等价于[^\f\n\r\t\v]
\t|匹配一个制表符，等价于\x09和\cI
\v|匹配一个垂直制表符，等价于\x0b和\cK
\w|匹配包括下划线的任何单词字符，等价于[A-Za-z0-9_]
\W|匹配任何非单词字符，等价于[^A-za-z0-9_]


#### java.util.regex包主要包括以下三个类：
#### Pattern类：pattern对象是一个正则表达式的编译表示
#### Matcher类：Matcher对象是对输入字符串进行解释和匹配操作的引擎
#### PatternSyntaxException：PatternSyntaxException是一个非强制异常类，表示一个正则表达式中的语法错误
#### 捕获组：把多个字符当一个单独单元进行处理,正则表达式中以()标记的子表达式所匹配的内容就是一个分组(group),通过调用matcher对象的groupCount方法可以查看表达式有多少分组。group(0)表示整个表达式，不包括在groupCount的返回值中。

#### Matcher类方法

序号|方法及说明
:--:|:---
1|public int start()<br>返回以前匹配的初始索引
2|public int start(int group)<br>返回在以前的匹配操作期间，由给定组所捕获的子序列的初始索引
3|public int end()<br>返回最后匹配字符之后的偏移量
4|public int end(int group)<br>返回在以前的匹配操作期间，由给定组所捕获子序列的最后字符之后的偏移量

#### 研究方法

|序号|方法及说明|
|:--:|:---|
|1|public boolean lookingAt()<br>尝试将从区域开头开始的输入序列与该模式匹配|
|2|public boolean find()<br>尝试查找与该模式匹配的输入序列的下一个子序列|
|3|public boolean find(int start)<br>重置此匹配器，然而尝试查找匹配该模式、从指定索引开始的输入序列的下一个子序列|
|4|public bolean matches()<br>尝试将整个区域与模式匹配|

#### 替换方法

|序号|方法及说明|
|:--:|:--|
|1|public Matcher appendReplacement(Stringbuffer sb,String replacement)<br>实现非终端添加和替换步骤，将给定替换字符串添加到字符串缓冲区|
|2|public StringBuffer appendTail(StringBuffer sb)<br>实现终端添加和替换步骤|
|3|public String replaceAll(String replacement)<br>替换模式与给定替换字符串匹配的输入序列的每个子序列|
|4|public String replaceFirst(String replacement)<br>替换模式与给定替换字符串匹配的输入序列的第一个序列|
|5|public static String quoteReplacement(Stirng s)<br>返回指定字符串的字面替换字符串。|

#### PatternSyntaxException类的方法

|序号|方法及说明|
|:--:|:--|
|1|public String getDescription()<br>获取错误描述|
|2|public int getindex()<br>获取错误索引|
|3|public String getPattern()<br>获取错误的正则表达式模式|
|4|public String getMessage()<br>返回多行字符串，包含语法错误及其索引的描述、错误的正则表达式模式和模式中错误索引的可视化指示|


#### 1.查找以Java开头，以任意字符结尾的字符串
{% highlight java %}
Pattern pattern=Pattern.compile("^Java.*");
Matcher matcher=pattern.matcher("Java是一门编程语言");
boolean b=matcher.matches();
{% endhighlight java %}
#### 2.以多个条件分割字符串
{% highlight java %}
Pattern pattern=Pattern.compile("[,|]+");
String[] strs=pattern.split("Hello World,Welcom to China")
{% endhighlight java %}
#### 3.文字替换(首次)
{% highlight java %}
Pattern pattern=Pattern.compile("JAVA");    //要被替换的字符
Matcher matcher=pattern.matcher("Hello JAVA,Hello JAVA");
String str=matcher.replaceFirst(Java);      //要替换为的字符
{% endhighlight java %}
#### 4.文字替换(全部)
{% highlight java %}
Pattern pattern=Pattern.compile("JAVA");    //要被替换的字符
Matcher matcher=pattern.matcher("Hello JAVA,Hello JAVA");
String str=matcher.replaceAll(Java);      //要替换为的字符
{% endhighlight java %}
#### 5.文字替换(置换字符)，效果与4相同
{% highlight java %}
Pattern pattern=Pattern.compile("JAVA");
Matcher matcher=pattern.matcher("Hello JAVA,JAVA");
StringBuffer sb=new StringBuffer();
while (matcher.find()){     //查找下一个子序列
    matcher.appendReplacement(sb,"java");   //将要替换为的字符添加到缓冲区
}
matcher.appendTail(sb);
System.out.println(sb.toString());
{% endhighlight java %}
#### 6.验证是否为邮箱地址
{% highlight java %}
Pattern pattern=
Pattern.compile("[]\\w.\\-]+@([\\w\\-]+\\.)+[\\w\\-]+",Pattern.CASE_INSENSITIVE);
Matcher matcher=pattern.matcher(str);
{% endhighlight java %}
#### 7.去除html标记
{% highlight java %}
//在dotall模式中,表达式.可以匹配任何字符，包括行结束符。默认情况下，此表达式不匹配行结束符
Pattern pattern=Pattern.compile("<.+?>",Pattern.DOTALL);
Matcher matcher=pattern.matcher("<a href=\"http://www.baidu.com\">百度</a>");
String str=matcher.replaceAll("");  // str的值为“百度”
{% endhighlight java %}
#### 8.查找html中对应条件字符串
{% highlight java %}
Pattern pattern=Pattern.compile("href=\"(.+?)\"");
Matcher matcher=pattern.matcher("<a href=\"http://www.baidu.com\">百度</a>");
if(matcher.find()){
    System.out.println(matcher.group(1));   //输出所有的链接
}
{% endhighlight java %}
#### 9.截取http:// 地址，效果和8相同
{% highlight java %}
Pattern pattern=Pattern.compile("(http://|https://){1}[\\w\\.\\-/:]+");
Matcher matcher=pattern.matcher("sagdi<http://www.baidu.com/index.html>sdfsdf");
StringBuffer sb=new StringBuffer();
while(matcher.find()){
    sb.append(matcher.group());
    sb.append("\r\n");
    System.out.println(sb.toString());   //输出所有的链接
}
{% endhighlight java %}
#### 10.替换{}的文字
{% highlight java %}
String str="人的年龄应该设置为{23}至{544}";
String[][] object={new String[]{"\\{23\\}","0"},new String[]{"\\{544\\}","120"}};
System.out.println(replace(str,object));
public static String replace(final String sourceString,Object[] object){
            String temp=sourceString;
            for(int i=0;i<object.length;i++){
                String[] result=(String[])object[i];
                Pattern pattern=Pattern.compile(result[0]);
                Matcher matcher=pattern.matcher(temp);
                temp=matcher.replaceAll(result[1]);
            }
            return temp;
}
{% endhighlight java %}