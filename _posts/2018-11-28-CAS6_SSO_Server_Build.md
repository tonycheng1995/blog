---
layout: post
title:  "CAS6.0单点服务器搭建"
date:   2018-11-28 +0800
toc: true
category: CAS
tags: cas 单点登录 SSO
excerpt: 前面已经讲过cas4.0的搭建，但是自cas5.0开始有了些许变化，所以又重新写了一遍以最新cas6.0为例搭建的步骤。
---
## 1.中间件下载
### 1.1网盘下载以下压缩包
#### JDK: https://pan.baidu.com/s/1mBzEoQCyb_2jD2VO0r-aTQ 提取码：18io
#### Tomcat: https://pan.baidu.com/s/1liYXfuiGhKZPKbDGIZdO8g 提取码：f4ih
#### Maven: https://pan.baidu.com/s/1pYB40lIJCxfKLB_X9l-TaA 提取码：0fs6
#### Cas: https://pan.baidu.com/s/1OiyexVbzbAcl5ujCl6k9UQ 提取码：zav0

### 1.2官网下载
#### JDK: https://www.oracle.com/technetwork/java/javase/downloads/jdk11-downloads-5066655.html
#### Tomcat: https://tomcat.apache.org/download-90.cgi
#### Maven: http://maven.apache.org/download.cgi
#### Cas: https://github.com/apereo/cas-overlay-template/tree/6.0

## 2.解压压缩包
#### 将压缩包放置到/usr/local/文件件下
#### Tomcat: tar -zxvf apache-tomcat-9.0.13.tar.gz
#### Maven: tar -zxvf apache-maven-3.6.0-bin.tar.gz
#### Cas: unzip cas-overlay-template-6.0.zip
#### 重命名文件夹：
{% highlight bash %}
[root@VM_152_123_centos local]# mv jdk-11.0.1 jdk
[root@VM_152_123_centos local]# mv apache-tomcat-9.0.13 tomcat
[root@VM_152_123_centos local]# mv apache-maven-3.6.0 maven
[root@VM_152_123_centos local]# mv cas-overlay-template-6.0 cas
{% endhighlight bash %}
## 3.配置环境变量
#### 编辑环境变量vim /etc/profile,加入以下语句
{% highlight bash %}
#java environment
export JAVA_HOME=/usr/local/jdk
export JRE_HOME=\${JAVA_HOME}/
export CLASSPATH=.:${JAVA_HOME}/lib/dt.jar:${JAVA_HOME}/lib/tools.jar
#tomcat environment
export JRE_HOME=/usr/local/jdk
export JAVA_HOME=/usr/local/jdk
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib:${JAVA_HOME}/lib/tools.jar
export CATALINA_HOME=/usr/local/tomcat
export CATALINA_BASE=/usr/local/tomcat
export PATH=${PATH}:${JAVA_HOME}/bin
#maven environment
export MAVEN_HOME=/usr/local/maven
export PATH=${MAVEN_HOME}/bin:${PATH}
{% endhighlight bash %}

#### 执行命令
{% highlight bash %}
[root@VM_152_123_centos ~]# source /etc/profile
{% endhighlight bash %}
## 4.验证安装
### 4.1验证jdk
{% highlight bash %}
[root@VM_152_123_centos ~]# java -version
java version "10.0.1" 2018-04-17
Java(TM) SE Runtime Environment 18.3 (build 10.0.1+10)
Java HotSpot(TM) 64-Bit Server VM 18.3 (build 10.0.1+10, mixed mode)
{% endhighlight bash %}

### 4.2验证maven
{% highlight bash %}
[root@VM_152_123_centos ~]# mvn -v
Apache Maven 3.6.0 (97c98ec64a1fdfee7767ce5ffb20918da4f719f3; 2018-10-25T02:41:47+08:00)
Maven home: /usr/local/maven
Java version: 10.0.1, vendor: Oracle Corporation, runtime: /usr/local/java
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "3.10.0-862.3.2.el7.x86_64", arch: "amd64", family: "unix"
{% endhighlight bash %}
### 4.3验证tomcat
{% highlight bash %}
[root@VM_152_123_centos ~]# cd /usr/local/tomcat/bin/
[root@VM_152_123_centos bin]# ./startup.sh
Using CATALINA_BASE:   /usr/local/tomcat
Using CATALINA_HOME:   /usr/local/tomcat
Using CATALINA_TMPDIR: /usr/local/tomcat/temp
Using JRE_HOME:        /usr/local/java/jre
Using CLASSPATH:       /usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar
Tomcat started.
[root@VM_152_123_centos bin]# ./shutdown.sh
{% endhighlight bash %}

## 5.生成HTTPS证书：
### 5.1生成server.keystore
{% highlight bash %}
[root@VM_152_123_centos bin]# cd /usr/local/tomcat/
[root@VM_152_123_centos tomcat]# keytool -genkey -alias tomcat -keyalg RSA -keypass tomcat -storepass tomcat -keystore server.keystore -validity 3600
What is your first and last name?
  [Unknown]:  sxbgjzlw.xyz    #网站域名
What is the name of your organizational unit?
  [Unknown]:  cheng
What is the name of your organization?
  [Unknown]:  cheng
What is the name of your City or Locality?
  [Unknown]:  sh
What is the name of your State or Province?
  [Unknown]:  sh
What is the two-letter country code for this unit?
  [Unknown]:  cn
Is CN=localhost, OU=cheng, O=cheng, L=sh, ST=sh, C=cn correct?
  [no]:  y
{% endhighlight bash %}

![]({{site.url}}/img/cas07.png)

#### 删除的操作
{% highlight bash %}
keytool -delete -alias tomcat -keystore server.keystore -storepass tomcat
{% endhighlight bash %}

### 5.2.生成证书server.cer
{% highlight bash %}
[root@VM_152_123_centos tomcat]# keytool -export -trustcacerts -alias tomcat -file server.cer -keystore server.keystore -storepass tomcat
Certificate stored in file <server.cer>
{% endhighlight bash %}
#### 导入证书,证书默认密码changeit,新密码至少6位
{% highlight bash %}
keytool -import -trustcacerts -alias tomcat -keystore "/usr/local/java/jdk/lib/security/cacerts" -file "/usr/local/tomcat/server.cer"
{% endhighlight bash %}

![]({{site.url}}/img/cas08.png)     

#### 若想删除此证书
{% highlight bash %}
keytool -delete -alias tomcat -keystore "/usr/local/jdk/lib/security/cacerts" -storepass changeit
{% endhighlight bash %}

## 6.配置证书
#### tomcat配置https,编辑server.xml
{% highlight bash %}
[root@VM_152_123_centos security]# vim /usr/local/tomcat/conf/server.xml
{% endhighlight bash %}
#### 将8443端口配置去掉注释，并改为以下内容：
{% highlight xml %}
<Connector port="8443" protocol="org.apache.coyote.http11.Http11Protocol"
     maxThreads="150" SSLEnabled="true" scheme="https" secure="true"
     clientAuth="false" sslProtocol="TLS" keystoreFile="server.keystore" keystorePass="tomcat"  />
{% endhighlight xml %}

#### ·port:https的端口默认8443
#### ·clientAuth:设置是否双向验证，默认为false,设置为true代表双向验证keystoreFile
#### ·keystoreFile:keystore证书的路径
#### ·keystorePass:生成keystore时的口令

## 7.验证
#### 测试连接是否成功。在浏览器输入http://sxbgjzlw.xyz:8443 ,出现tomcat页面

![]({{site.url}}/img/cas09.png)

## 8.编译cas包
{% highlight bash %}
[root@VM_152_123_centos cas]# cd /usr/local/cas/
[root@VM_152_123_centos cas]# [root@VM_152_123_centos cas]# ./build.sh
{% endhighlight bash %}

![]({{site.url}}/img/cas10.png)

## 9.部署
#### 编译成功后，/usr/local/cas/maven-overlay/文件夹下多出一个target文件夹，将/target/cas.war文件放到tomcat/webapps下
{% highlight bash %}
[root@VM_152_123_centos target]# cp cas.war /usr/local/tomcat/webapps/
{% endhighlight bash %}

#### 访问https://sxbgjzlw.xyz:8843/cas/login获取到如下页面
![]({{site.url}}/img/cas11.png)

#### 输入默认用户名和密码casuser/Mellon,成功登录页面如图所示
![]({{site.url}}/img/cas12.png)
