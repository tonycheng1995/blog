---
layout: post
title:  "存取Cookie"
date:   2018-04-17  +0800
toc: true
category: js
tags: js
excerpt: cookie是网站为了辨别用户身份、进行 session 跟踪而储存在用户本地终端上的数据。
---
#### cookie是网站为了辨别用户身份、进行 session 跟踪而储存在用户本地终端上的数据。因此单单的写一个html页面访问是不会存储cookie的，需要有服务器。
#### 代码样例
{% highlight html %}
<html>
<head >
<meta charset="utf-8" />
</head>
<body>
账号：<input id="name" type="text"/>
密码：<input id="password" type="password"/>
<input type="checkbox" id="remember" value="Car" />记住密码
<input id="submit" value="登录"   type="submit"/>
</body>
<script>
window.onload=function(){
	var nameBox=document.getElementById("name");
	var passwordBox=document.getElementById("password");
	var rememberBox=document.getElementById("remember");
	var buttonBox=document.getElementById("submit");

	//cookie是否保存账号
	if(getCookie("name")&&getCookie("password")){
		nameBox.value=getCookie("name");
		passwordBox.value=getCookie("password");
		rememberBox.checked=true;
	}

	//如果未勾选记住密码，清除cookie数据
	rememberBox.onchange=function(){
		if(!this.checked){
			delCookie("name");
			delCookie("password");
		}
	};

	//提交数据，如果勾选记住密码，保存cookie
	buttonBox.onclick=function(){
		if(rememberBox.checked){
			setCookie("name",nameBox.value,7);
			setCookie("password",passwordBox.value,7);
		}
	};

	//将数据保存至cookie
	function setCookie(name,value,day){
		var date = new Date();
		date.setTime(date.getTime()+(day*24*60*60*1000));
		var expires = "expires="+date.toGMTString();
		document.cookie = name+"="+value+"; "+expires;
	};

	//获取cookie数据
	function getCookie(cname){
		var name = cname + "=";
		var ca = document.cookie.split(';');
		for(var i=0; i<ca.length; i++) {
			var c = ca[i].trim();
			if (c.indexOf(name)==0) { return c.substring(name.length,c.length); }
		}
		return "";
	}

	//清除cookie
	function delCookie(name){
		setCookie(name,null,-1);
	};

};
</script>
</html>
{% endhighlight html %}