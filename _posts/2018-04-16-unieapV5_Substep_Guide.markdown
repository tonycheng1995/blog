---
layout: post
title:  "unieapV5分步向导"
date:   2018-04-16  +0800
toc: true
category: unieap
tags: JS
excerpt: 对于unieapV5的分步向导存在按钮无点击事件和不可点击的问题，那我们就需要根据自己的需求修改代码。
---
#### 对于unieapV5的分步向导存在按钮无点击事件和不可点击的问题，那我们就需要根据自己的需求修改代码。
#### 分步向导容器
#### 1.添加容器控件Wizard,可在属性中设置按钮的位置和向导图标的形状。
#### 2.添加多个WizardPane,可在属性中设置向导页的初始激活状态和标题。
#### 3.在向导页中添加需要的内容

#### 附加：
#### 分步向导容器对应的“上一步”，“下一步”按钮没有相应的事件，“完成”按钮不可点击，我们可以自己为按钮添加事件并使它可以点击
#### 编辑page_load()事件，(本例以三步向导为例)
{% highlight html %}
var step=1;
//下一步事件
$(".btn-primary").mousedown(function(){
switch(step) {
	case 1:
		step+=1;
		alert("第一步完成！");
		break;
	case 2:
		step+=1;
		alert("第二步完成！");
		break;
	case 3:
		// step+=1;
		alert("第三步完成！");
		break;
	default:
		//default_statement;
}
});
//将完成按钮置位可点击
$(".btn-primary").click(function(){
this.removeAttribute("disabled");
});
//上一步事件
$(".btn-default").mousedown(function(){
switch(step) {
	case 2:
		step-=1;
		alert("第二步取消！");
		break;
	case 3:
		step-=1;
		alert("第三步取消！");
		break;
	default:
		//default_statement;
}
});
{% endhighlight html %}

#### 至此，已基本完成，可以在相应位置添加需要执行的事件。
