---
layout: post
title:  "JS打印HTML页面"
date:   2018-03-27  +0800
toc: true
category: js
tags: js
excerpt: 调用js打印html页面。
---
#### 简单的一行代码调用
{% highlight html %}
        <script>
            function printHtml(){
            window.print();
            }
        </script>
        <button onclick="printHtml();">打印</button>
{% endhighlight html %}

#### 打印指定区域
{% highlight html %}
<script>
    function appointprint(){
    var newWindow=window.open("打印窗口","_blank");
    var docStr=document.getElementById('appoint').innerHTML;
    newWindow.document.write(docStr);
    newWindow.print();
    newWindow.close();
    }
</script>
<button onclick="appointprint()">打印指定区域</button>
{% endhighlight html %}

#### 但是对于某些特殊情况，如想要打印弹出页的内容，而这时的弹出页和原页面是一体的，这时候如果使用上面的打印方法就会出现打印乱码的情况，这时可以给要打印的内容加个div,然后调用下面的打印方法。
{% highlight js %}
function printdiv(id) {
    var headhtml = "<html><head><title></title></head><body>";
    var foothtml = "</body>";
    // 获取div中的html内容
    var newhtml = document.all.item(id).innerHTML;

    // 获取原来的窗口界面body的html内容，并保存起来
    var oldhtml = document.body.innerHTML;

    // 给窗口界面重新赋值，赋自己拼接起来的html内容
    document.body.innerHTML = headhtml + newhtml + foothtml;
    // 调用window.print方法打印新窗口
    window.print();

    // 将原来窗口body的html值回填展示
    document.body.innerHTML = oldhtml;
    return false;
}
{% endhighlight js %}