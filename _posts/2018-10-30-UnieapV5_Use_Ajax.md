---
layout: post
title: "unieapV5使用ajax"
date:   2018-10-30 +0800
toc: true
category: unieap
tags: unieap ajax
excerpt: 本篇文章主要讲解在ACAP V5中使用自己写的ajax方法。
---
#### 尽管ACAP中为我们封装了前台请求后台的方法，但是有时候因为需求仍需要我们自己写ajax方法。但是其中一些请求头数据是必有的，方法demo如下。
## GET请求
{% highlight js %}
var headers = {ajaxRequest:true};
headers['replayId'] = uuid.v4().replace(/\-/g, "").toUpperCase();
headers['Content-Type']="application/json";
  $.ajax({
        url : "/framework/ws/optionItem/listOptionItem?id="+approvalItemId,
        type : 'GET',
        timeout: 120 * 1000,
    async: true,
    headers: headers,     //必须有
      xhrFields: {
          withCredentials: true
      },
        success : function(data) {
          //方法
        }
    });
{% endhighlight js %}

## POST请求
{% highlight js %}
var formData = new FormData();
formData.append("myfile", fileObject);
$.ajax({
    url : "/framework/ws/annexaa/uploadAnnex?replayId="+replayId+"&fileUpload=true",
    type : 'POST',
    data:formData,
    timeout: 120 * 1000,
    async: true,
    processData:false,
    contentType:false,
  xhrFields: {
        withCredentials: true
    },
    success : function(data) {
      //方法
    }
});
{% endhighlight js %}