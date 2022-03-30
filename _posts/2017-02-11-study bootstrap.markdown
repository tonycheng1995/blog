---
layout: post
title:  "Bootstrap的学习"
date:   2017-02-11 22:56:58 +0800
toc: true
category: Bootstrap
tags: Bootstrap 前端
excerpt: 你是不是苦于写前端页面？总是感觉自己写的不是很好看，而bootstrap不仅能解决你的问题，它还是一个响应式框架，并且简单易学
---
#### 从[Bootstrap中文网站](http://www.bootcss.com/)下载bootsrap文件

## 三个个必须文件
#### ·bootstrap.min.css
#### ·jquery.min.js
#### ·bootstrap.min.js

#### 基本模板
{% highlight html %}
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
    <meta charset="utf-8">
    <!-- 设置IE的解析方式 -->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <!-- 视窗设置 -->
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>Bootstrap 101 Template</title>

    <!-- 1.加载Bootstrap层叠样式表 -->
    <link href="css/bootstrap.min.css" rel="stylesheet">

    <!-- HTML5 shim and Respond.js for IE8 support of HTML5 elements and media queries -->
    <!-- WARNING: Respond.js doesn't work if you view the page via file:// -->
    <!--[if lt IE 9]>
      <script src="https://cdn.bootcss.com/html5shiv/3.7.3/html5shiv.min.js"></script>
      <script src="https://cdn.bootcss.com/respond.js/1.4.2/respond.min.js"></script>
    <![endif]-->
  </head>
  <body>
    <h1>你好，世界！</h1>

    <!-- jQuery (2.Bootstrap必要的jQuery库，且必须在bootstrap.min.js之前加载) -->
    <script src="https://cdn.bootcss.com/jquery/1.12.4/jquery.min.js"></script>
    <!-- 3.加载bootstrap的核心js库-->
    <script src="js/bootstrap.min.js"></script>
  </body>
</html>
{% endhighlight html %}

#### Bootstrap需要为页面内容和栅格系统包裹一个`.container`容器。这两种容器不能相互嵌套。
#### `.container`类用于固定宽度并支持响应式布局的容器
{% highlight html %}
<div class="container">
  ...
</div>
{% endhighlight html %}

#### `.container-fluid`类用于100%宽度，占据全部视窗的容器
{% highlight html %}
<div class="container-fluid">
  ...
</div>
{% endhighlight html %}

## 字体图标使用
#### 把图标的属性粘贴到class属性的值里
{% highlight html %}
<span class="glyphicon glyphicon-euro"></span>
{% endhighlight html %}

## 按钮
#### 按钮样式有7种，分别为`btn-default`、`btn-primary`、`btn-success`、`btn-info`、`btn-warning`、`btn-danger`、`btn-link`
{% highlight html %}
<button type="button" class="btn btn-default">默认</button>
{% endhighlight html %}

## 尺寸
#### 使用`.btn-lg`、`默认`、`.btn-sm`或`.btn-xs`可以获得不同尺寸的按钮
{% highlight html %}
<button type="button" class="btn btn-default btn-lg">按钮</button>
{% endhighlight html %}

## 激活状态
#### 由于`：active`是伪状态，因此无需额外添加，但是在需要其表现出同样外观的时候可以添加`.active`类
{% highlight html %}
<button type="button" class="btn btn-default btn-lg active">按钮</button>
{% endhighlight html %}

## 禁用状态
#### 添加`disabled`属性，使按钮表现出禁用状态
{% highlight html %}
<button type="button" class="btn btn-default btn-lg disabled">按钮</button>
{% endhighlight html %}

## 按钮类
#### 以下四种都可以表现按钮样式
{% highlight html %}
<a class="btn btn-default" href="#" role="button">按钮</a>
<button class="btn btn-default" type="submit">按钮</button>
<input class="btn btn-default" type="button" value="按钮">
<input class="btn btn-default" type="submit" value="提交">
{% endhighlight html %}

## 下拉菜单
#### 下拉菜单必须有一个外部的容器,并且class的值为`.dropdown;`或者为声明了`position:relative;`的元素。button元素的`id`值与ul的`aria-labelledby`的值一致，
{% highlight html %}
<div class="dropdown">
<button class="btn btn-default dropdown-toggle" type="button" id="dropdownMenu1" data-toggle="dropdown" aria-haspopup="true" aria-expanded="true">
菜单
<span class="caret"></span>
</button>
<ul class="dropdown-menu" aria-labelledby="dropdownMenu1">
  <li class="dropdown-header">选项标题</li>
  <li><a href="#">选项1</a></li>
  <li><a href="#">选项2</a></li>
  <!-产生分割线--->
  <li role="separator" class="divider"></li>
  <li><a href="#">选项3</a></li>
  <!--.disabled类用来达到禁用效果-->
  <li class="disabled"><a href="#">禁用选项</a></li>
</ul>
</div>
{% endhighlight html %}

## 按钮组
#### 将一系列button放到class值为`.btn-group`的div容器中,只需给`.btn-group`加上`btn-group-*`,就省去了为组中的每个按钮都赋予尺寸
{% highlight html %}
<div class="btn-group">
<button type="button" class="btn btn-default">向左</button>
<button type="button" class="btn btn-default">中间</button>
<button type="button" class="btn btn-default">向右</button>
</div>
{% endhighlight html %}

## 按钮工具栏
#### 把多个按钮组归到一排。把多个`<div class="btn-group">`组合放到一个`<div class="btn-toolbar">`中
{% highlight html %}
<div class="btn-toolbar" >
<div class="btn-group" role="group" aria-label="...">...</div>
<div class="btn-group" role="group" aria-label="...">...</div>
<div class="btn-group" role="group" aria-label="...">...</div>
</div>
{% endhighlight html %}

## 嵌套
#### 把下拉菜单混合到一系列按钮中，只需把`.btn-group`放入到另外一个`.btn-group`中
{% highlight html %}
<div class="btn-group" role="group" aria-label="...">
  <button type="button" class="btn btn-default">1</button>
  <button type="button" class="btn btn-default">2</button>

  <div class="btn-group" role="group">
    <button type="button" class="btn btn-default dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
      下拉菜单
      <span class="caret"></span>
    </button>
      <ul class="dropdown-menu">
        <li><a href="#">选项1</a></li>
        <li><a href="#">选项2</a></li>
      </ul>
  </div>
</div>
{% endhighlight html %}

## 垂直排列
#### 让一组按钮垂直显示，只需把外部容器的class的值设为`btn-group-vertical`
{% highlight html %}
<div class="btn-group-vertical" role="group" aria-label="...">
  ...
</div>
{% endhighlight html %}

## 按钮组两端对齐占满容器
#### 1.对于`<a>`按钮，只需将元素放到`.btn-group.btn-group-justified`中
{% highlight html %}
<div class="btn-group btn-group-justified">
  <a class="btn btn-default" href="#" role="button">1</a>
  <a class="btn btn-default" href="#" role="button">2</a>
  <a class="btn btn-default" href="#" role="button">3</a>
</div>
{% endhighlight html %}

#### 2.对于button按钮，必须将每个按钮放到按钮组中
{% highlight html %}
<div class="btn-group btn-group-justified">
  <div class="btn-group" role="group">
    <button type="button" class="btn btn-default">1</button>
  </div>
  <div class="btn-group" role="group">
    <button type="button" class="btn btn-default">2</button>
  </div>
  <div class="btn-group" role="group">
    <button type="button" class="btn btn-default">3</button>
  </div>
</div>
{% endhighlight html %}

## 分列式下拉菜单
{% highlight html %}
<div class="btn-group">
  <button type="button" class="btn btn-danger">菜单</button>
  <button type="button" class="btn btn-danger dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
    <span class="caret"></span>
  </button>
  <ul class="dropdown-menu">
    <li><a href="#">选项1</a></li>
    <li><a href="#">选项2</a></li>
  </ul>
</div>
{% endhighlight html %}

## 输入框组
#### 通过在文本框`<input>`前面、后面和两边加上文字或按钮，可以实现对表单控件的扩展。为`.input-group`赋予`.input-group-addon`或`.input-group-btn`类，可以给`.form-control`的前面或后面添加额外的元素
#### 为`.input-group`添加相应的尺寸类，其内部包含的元素将自动调整自身的尺寸。
{% highlight html %}
<label for="basic-url">您的常用邮箱</label>
<div class="input-group input-group-lg">
  <span class="input-group-addon" id="basic-addon1">邮箱：</span>
  <input type="text" class="form-control" placeholder="chengzequn" >
  <span class="input-group-addon" id="basic-addon2">@</span>
  <input type="text" class="form-control" placeholder="163" >
  <span class="input-group-addon" id="basic-addon2">.com</span>
</div>
{% endhighlight html %}

## 表单
#### 单独的表单控件会自动赋予一些全局样式，设置`.form-control`类的`<input>`、`<textarea>`和`<select>`元素默认宽度属性为`width:100%`.
#### 改变form的class属性值`默认`、`.form-inline`或`form-horizontal`可以改变其样式
#### 输入框包括大部分的表单控件，支持`text`、`password`、`datetimelocal`、`date`、`month`、`time`、`week`、`number`、`email`、`url`、`search`、`tel`和`color`
#### 为输入框加入属性`disabled`将输入框状态设置为禁用状态,添加属性`readonly`设置为只读状态
#### 为表单组里的div添加`form-group-lg`或`form-group-sm`可以相应改变表单组的大小
#### 为输入框添加`.input-lg`、`默认`或`input-sm`可相应设置输入框的大小
{% highlight html %}
<form>
  <div class="form-group">
    <label for="exampleInputEmail1">账户：</label>
    <input type="email" class="form-control" id="exampleInputEmail1" placeholder="Email">
  </div>
  <div class="form-group">
    <label for="exampleInputPassword1">密码：</label>
    <input type="password" class="form-control" id="exampleInputPassword1" placeholder="Password">
  </div>
  <div class="form-group">
    <label for="exampleInputFile">文件上传：</label>
    <input type="file" id="exampleInputFile">
    <p class="help-block">帮助信息</p>
  </div>
  <div class="checkbox">
    <label>
      <input type="checkbox"> 我同意该协议
    </label>
  </div>
  <button type="submit" class="btn btn-default">提交</button>
  <button type="reset" class="btn btn-default">重置</button>
</form>
{% endhighlight html %}

### 水平排列表单
{% highlight html %}
<form class="form-horizontal">
  <div class="form-group">
    <label for="inputEmail3" class="col-sm-2 control-label">Email</label>
    <div class="col-sm-8">
      <input type="email" class="form-control" id="inputEmail3" placeholder="Email" disabled>
    </div>
  </div>
</form>
{% endhighlight html %}

## 校验状态
#### 添加`.has-warning`、`.has-error`、`.has-success`类到控件的父元素，任何包含在此元素之内的`.control-label`、`.form-control`或`.help-block`元素都将接受这些校验状态的样式
{% highlight html %}
<div class="form-group has-success">
  <label class="control-label" for="inputSuccess1">输入正确</label>
  <input type="text" class="form-control" id="inputSuccess1" aria-describedby="helpBlock2">
  <span id="helpBlock2" class="help-block">帮助提示信息</span>
</div>
<div class="form-group has-warning">
  <label class="control-label" for="inputWarning1">输入警告</label>
  <input type="text" class="form-control" id="inputWarning1">
</div>
<div class="form-group has-error">
  <label class="control-label" for="inputError1">输入正确</label>
  <input type="text" class="form-control" id="inputError1">
</div>
{% endhighlight html %}

## 添加额外图标
#### 针对校验状态为输入框添加额外图标。只需设置相应的`.has-feedback`类并添加正确的图标即可，图标只能使用在文本输入框`<input class="form-control">`元素上
{% highlight html %}
<div class="form-group has-success has-feedback">
  <input type="text" class="form-control" id="inputSuccess2" aria-describedby="inputSuccess2Status">
  <span class="glyphicon glyphicon-ok form-control-feedback" aria-hidden="true"></span>
  <span id="inputSuccess2Status" class="sr-only">(success)</span>
</div>
{% endhighlight html %}

## 响应式图片
#### 为图片添加`.img-responsive`类可以让图片支持响应式布局，实质是为图片设置了`max-width:100%`、`hight:auto`和`display:block`
#### 要使使用`.img-responsive`类的图片居中，添加`.center-block`类
{% highlight html %}
<img src="..." class="img-responsive" alt="Responsive image">
{% endhighlight html %}

#### 图片形状
{% highlight html %}
<img src="..." alt="..." class="img-rounded">
<img src="..." alt="..." class="img-circle">
<img src="..." alt="..." class="img-thumbnail">
{% endhighlight html %}

## 关闭按钮
{% highlight html %}
<button type="button" class="close" aria-label="Close"><span aria-hidden="true">&times;</span></button>
{% endhighlight html %}

## 快速浮动
{% highlight html %}
<div class="pull-left">...</div>
<div class="pull-right">...</div>
{% endhighlight html %}

## 清除浮动
#### 为父元素添加`.clearfix`类可以很容易地清除浮动（float）
{% highlight html %}
<div class="clearfix">...</div>
{% endhighlight html %}

## 显示或隐藏
#### `.show`和`.hidden`类可以强制任意元素显示或隐藏(对于屏幕阅读器也能起效)
{% highlight html %}
<div class="show">...</div>
<div class="hidden">...</div>
{% endhighlight html %}

## 栅格系统
#### “行（row）”必须包含在`.container`（固定宽度）或`.container-fluid`（100% 宽度）中，以便为其赋予合适的排列（aligment）和内补（padding）。
#### 如果一“行（row）”中包含了的“列（column）”大于 12，多余的“列（column）”所在的元素将被作为一个整体另起一行排列
#### 如果不希望在小屏幕上所有列堆叠在一起，我们可以添加针对不同屏幕的类
{% highlight html %}
<div class="row">
  <div class="col-xs-12 col-md-8">.col-xs-12 .col-md-8</div>
  <div class="col-xs-6 col-md-4">.col-xs-6 .col-md-4</div>
</div>
{% endhighlight html %}

#### 列偏移。使用`.col-md-offset-*`类可以将列向右侧偏移
{% highlight html %}
<div class="row">
  <div class="col-md-4">.col-md-4</div>
  <div class="col-md-4 col-md-offset-4">.col-md-4 .col-md-offset-4</div>
</div>
{% endhighlight html %}

#### 嵌套列。为了使用内置的栅格系统将内容再次嵌套，可以通过添加一个新的`.row`元素和一系列`.col-sm-*`元素到已经存在的`.col-sm-*`元素内
{% highlight html %}
<div class="row">
  <div class="col-sm-9">
    Level 1: .col-sm-9
    <div class="row">
      <div class="col-xs-8 col-sm-6">
        Level 2: .col-xs-8 .col-sm-6
      </div>
      <div class="col-xs-4 col-sm-6">
        Level 2: .col-xs-4 .col-sm-6
      </div>
    </div>
  </div>
</div>
{% endhighlight html %}

#### 列排序。通过使用`.col-md-push-*`和`.col-md-pull-*`类就可以很容易的改变列（column）的顺序,相当于推出去和拉回来多少列
{% highlight html %}
<div class="row">
  <div class="col-md-9 col-md-push-3">.col-md-9 .col-md-push-3</div>
  <div class="col-md-3 col-md-pull-9">.col-md-3 .col-md-pull-9</div>
</div>
{% endhighlight html %}

## 导航
#### 标签页。`类依赖`类
#### 胶囊式标签页。只需将类改为`.nav-pills`，如果想垂直排列只需加入`.nav-stacked`
{% highlight html %}
<ul class="nav nav-tabs">
  <li role="presentation" class="active"><a href="#">主页</a></li>
  <li role="presentation"><a href="#">介绍</a></li>
  <li role="presentation"><a href="#">联系</a></li>
</ul>
{% endhighlight html %}

## 导航条
#### 图标logo。由于`.navbar-brand`已经被设置了内补（padding）和高度（height），你需要根据自己的情况添加一些 CSS 代码从而覆盖默认设置。
{% highlight html %}
<nav class="navbar navbar-default">
  <div class="container-fluid">
    <div class="navbar-header">
      <a class="navbar-brand" href="#">
        <img alt="Brand" src="...">
      </a>
    </div>
  </div>
</nav>
{% endhighlight html %}

#### 表单。将表单放置于`.navbar-form`之内可以呈现很好的垂直对齐，并在较窄的视口（viewport）中呈现折叠状态。 使用对齐选项可以规定其在导航条上出现的位置。
{% highlight html %}
<form class="navbar-form navbar-left" role="search">
  <div class="form-group">
    <input type="text" class="form-control" placeholder="Search">
  </div>
  <button type="submit" class="btn btn-default">Submit</button>
</form>
{% endhighlight html %}

#### 按钮。对于不包含在`<form>`中的`<button>`元素，加上`.navbar-btn`后，可以让它在导航条里垂直居中
{% highlight html %}
<button type="button" class="btn btn-default navbar-btn">Sign in</button>
{% endhighlight html %}

#### 文本。把文本包裹在`.navbar-text`中时，为了有正确的行距和颜色，通常使用`<p>`标签
{% highlight html %}
<p class="navbar-text">欢迎访问</p>
{% endhighlight html %}

#### 非导航链接。或许你希望在标准的导航组件之外添加标准链接，那么，使用`.navbar-link`类可以让链接有正确的默认颜色和反色设置。
{% highlight html %}
<p class="navbar-text navbar-right">请您点击<a href="#" class="navbar-link">链接</a></p>
{% endhighlight html %}

#### 固定在顶部。添加`.navbar-fixed-top`类可以让导航条固定在顶部，还可包含一个`.container`或`.container-fluid`容器，从而让导航条居中，并在两侧添加内补（padding）
{% highlight html %}
<nav class="navbar navbar-default navbar-fixed-top">
  <div class="container">
    ...
  </div>
</nav>
{% endhighlight html %}

#### 反色导航条。通过添加`.navbar-inverse`类可以改变导航条的外观
{% highlight html %}
<nav class="navbar navbar-inverse">
  ...
</nav>
{% endhighlight html %}

#### 路径导航。
{% highlight html %}
<ol class="breadcrumb">
  <li><a href="#">一级</a></li>
  <li><a href="#">二级</a></li>
  <li class="active">三级</li>
</ol>
{% endhighlight html %}

## 分页
#### 我们可以给不能点击的链接添加`.disabed`类、给当前页添加`.active`类
#### 添加`.pagination-lg`或`.pagination-sm`可以改变分页的尺寸
{% highlight html %}
<nav aria-label="...">
  <ul class="pagination">
    <li class="disabled"><a href="#" aria-label="Previous"><span aria-hidden="true">&laquo;</span></a></li>
    <li class="active"><a href="#">1 <span class="sr-only">(current)</span></a></li>
    <li><a href="#">2</a></li>
        <li><a href="#">3</a></li>
        <li><a href="#">4</a></li>
        <li><a href="#">5</a></li>
        <li>
          <a href="#" aria-label="Next">
            <span aria-hidden="true">&raquo;</span>
          </a>
        </li>
  </ul>
</nav>
{% endhighlight html %}

## 翻页
{% highlight html %}
<nav aria-label="...">
  <ul class="pager">
    <li><a href="#">上一页</a></li>
    <li><a href="#">下一页</a></li>
  </ul>
</nav>
{% endhighlight html %}

## 标签
#### `<h3>`这是一个`<span class="label label-default">`标签`</span></h3>`

## 徽章
#### 给链接、导航等元素嵌套`<span class="badge">`元素，可以很醒目的展示新的或未读的信息条目
{% highlight html %}
<a href="#">信息<span class="badge">42</span></a>
<button class="btn btn-primary" type="button">
  信息<span class="badge">4</span>
</button>
{% endhighlight html %}

## 巨幕
#### 延伸至整个浏览器视口来展示网站上的关键内容
{% highlight html %}
<div class="jumbotron">
  <h1>Hello, world!</h1>
  <p>...</p>
  <p><a class="btn btn-primary btn-lg" href="#" role="button">Learn more</a></p>
</div>
{% endhighlight html %}

## 页头
#### 页头组件能够为`h1`标签增加适当的空间，并且与页面的其他部分形成一定的分隔。它支持`h1`标签内内嵌`small`元素的默认效果，还支持大部分其他组件
{% highlight html %}
<div class="page-header">
  <h1>页头<small>文本</small></h1>
</div>
{% endhighlight html %}

## 缩略图
{% highlight html %}
<div class="row">
  <div class="col-xs-6 col-md-3">
    <a href="#" class="thumbnail">
      <img src="..." alt="...">
    </a>
  </div>
</div>
{% endhighlight html %}

#### 自定义内容
{% highlight html %}
<div class="row">
  <div class="col-xs-6 col-md-3">
    <a href="#" class="thumbnail">
      <img src="..." alt="...">
    </a>
    <div class="caption">
        <h3>标题</h3>
        <p>内容</p>
        <p><a href="#" class="btn btn-primary" role="button">Button</a> <a href="#" class="btn btn-default" role="button">Button</a></p>
      </div>
  </div>
</div>
{% endhighlight html %}

## 警告框
#### 将任意文本和一个可选的关闭按钮组合在一起就能组成一个警告框，`.alert`类是必须要设置的，另外我们还提供了有特殊意义的4个类（例如`.alert-success`），代表不同的警告信息
#### 如果想要可关闭的警告框，可以添加`.alert-dismissible`
#### 用`.alert-link`工具类，可以为链接设置与当前警告框相符的颜色
{% highlight html %}
<div class="alert alert-success" role="alert">
  <a href="#" class="alert-link">...</a>
</div>
<!--可关闭的警告框-->
<div class="alert alert-warning alert-dismissible" role="alert">
  <button type="button" class="close" data-dismiss="alert" aria-label="Close"><span aria-hidden="true">&times;</span></button>
  <strong>警告</strong> 可关闭的警告框
</div>
{% endhighlight html %}

## 进度条
#### 添加`progress-bar-success`、`progress-bar-info`、`proress-bar-waring`等可以改变进度条颜色
#### 添加`progress-bar-striped`类可以通过渐变为进度条创建条纹
{% highlight html %}
<div class="progress">
  <div class="progress-bar" role="progressbar" aria-valuenow="60" aria-valuemin="0" aria-valuemax="100" style="width: 60%;">
    60%
  </div>
</div>
{% endhighlight html %}

#### 在展示很低的百分比时，如果需要让文本提示能够清晰可见，可以为进度条设置`min-width`
{% highlight html %}
<div class="progress">
  <div class="progress-bar" role="progressbar" aria-valuenow="0" aria-valuemin="0" aria-valuemax="100" style="min-width: 2em;">
    0%
  </div>
</div>
{% endhighlight html %}

#### 动画效果。为`.progress-bar-striped`添加`.active`类，使其呈现出右向左运动的动画效果
{% highlight html %}
<div class="progress">
  <div class="progress-bar progress-bar-striped active" role="progressbar" aria-valuenow="45" aria-valuemin="0" aria-valuemax="100" style="width: 45%">
    <span class="sr-only">45% Complete</span>
  </div>
</div>
{% endhighlight html %}

#### 堆叠效果。把多个进度条放入同一个`.progress`中，使它们呈现堆叠的效果
{% highlight html %}
<div class="progress">
  <div class="progress-bar progress-bar-success" style="width: 35%">
  </div>
  <div class="progress-bar progress-bar-warning progress-bar-striped" style="width: 20%">
  </div>
  <div class="progress-bar progress-bar-danger" style="width: 10%">
  </div>
</div>
{% endhighlight html %}

## Well嵌入效果
{% highlight html %}
<div class="well">...</div>
{% endhighlight html %}

## 媒体对象
#### 这是一个抽象的样式，用以构建不同类型的组件，这些组件都具有在文本内容的左或右侧对齐的图片（就像博客评论或 Twitter 消息等）
{% highlight html %}
<div class="media">
  <div class="media-left">
    <a href="#">
      <img class="media-object" src="..." alt="...">
    </a>
  </div>
  <div class="media-body">
    <h4 class="media-heading">Media heading</h4>
    ...
  </div>
</div>
{% endhighlight html %}

## 列表组
#### 带有多个列表条目的无序列表
{% highlight html %}
<ul class="list-group">
  <li class="list-group-item">123</li>
  <li class="list-group-item">456</li>
  <li class="list-group-item">789</li>
</ul>
{% endhighlight html %}

## 面板
#### 默认的`.panel`组件所做的只是设置基本的边框（border）和内补（padding）来包含内容
{% highlight html %}
<div class="panel panel-default">
  <div class="panel-body">
    Basic panel example
  </div>
</div>
{% endhighlight html %}
#### 带标题的面板。通过`.panel-heading`可以很简单地为面板加入一个标题容器。你也可以通过添加设置了`.panel-title`类的`<h1>-<h6>`标签，添加一个预定义样式的标题。不过,`<h1>-<h6>`标签的字体大小将被`.panel-heading`的样式所覆盖。
#### 为了给链接设置合适的颜色，务必将链接放到带有`.panel-title`类的标题标签内
{% highlight html %}
<div class="panel panel-default">
  <div class="panel-heading">标题</div>
  <div class="panel-body">
    内容
  </div>
</div>

<div class="panel panel-default">
  <div class="panel-heading">
    <h3 class="panel-title">标题</h3>
  </div>
  <div class="panel-body">
    内容
  </div>
</div>
{% endhighlight html %}

#### 带脚注的面板。把按钮或次要的文本放入`.panel-footer`容器内。注意面版的脚注不会从情境效果中继承颜色，因为他们并不是主要内容
{% highlight html %}
<div class="panel panel-primary">
  <div class="panel-body">
    内容
  </div>
  <div class="panel-footer">脚注</div>
</div>
{% endhighlight html %}

#### 带表格的面板。为面板中不需要边框的表格添加`.table`类，是整个面板看上去更像是一个整体设计。如果是带有`.panel-body`的面板，我们为表格的上方添加一个边框，看上去有分隔效果。
{% highlight html %}
<div class="panel panel-default">
  <!-- Default panel contents -->
  <div class="panel-heading">标题</div>
  <div class="panel-body">
    <p>...</p>
  </div>
  <!-- Table -->
  <table class="table">
    ...
  </table>
</div>
{% endhighlight html %}

#### 带列表组面板。
{% highlight html %}
<div class="panel panel-default">
  <!-- Default panel contents -->
  <div class="panel-heading">标题</div>
  <div class="panel-body">
    <p>...</p>
  </div>
  <!-- List group -->
  <ul class="list-group">
    <li class="list-group-item">123</li>
    <li class="list-group-item">456</li>
  </ul>
</div>
{% endhighlight html %}

## 具有响应式特性的嵌入内容
#### 根据被嵌入内容的外部容器的宽度，自动创建一个固定的比例，从而让浏览器自动确定视频或`slideshow`的尺寸，能够在各种设备上缩放。
#### 这些规则被直接应用在`<iframe>`、`<embed>`、`<video>`和`<object>`元素上。如果你希望让最终样式与其他属性相匹配，还可以明确地使用一个派生出来的`.embed-responsive-item`类。
{% highlight html %}
<!-- 16:9 aspect ratio -->
<div class="embed-responsive embed-responsive-16by9">
  <iframe class="embed-responsive-item" src="..."></iframe>
</div>

<!-- 4:3 aspect ratio -->
<div class="embed-responsive embed-responsive-4by3">
  <iframe class="embed-responsive-item" src="..."></iframe>
</div>
{% endhighlight html %}

#### 看了上面的内容你可能会觉得不仅没有比官网的介绍简单，也许反而更无聊。sorry,浪费你的时间了，但是这篇只算是我学习时的点滴吧。不喜勿喷。
