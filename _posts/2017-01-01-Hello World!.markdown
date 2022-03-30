---
layout: post
title:  "Hello World！"
date:   2017-01-01 16:43:58 +0800
toc: true
category: 博客
tags: 博客 Jekyll
excerpt: 对于程序员来说，每一门语言的开始都要学会打印一句“Hello World！”，而这里我将要讲解一下如何构建一个像我这样的博客
feature: https://timgsa.baidu.com/timg?image&quality=80&size=b10000_10000&sec=1491455860&di=607cd3c5ba32258f376ec12054168f4b&src=http://dl.bizhi.sogou.com/images/2014/01/21/497316.jpg
---
#### 对于程序员来说，每一门语言的开始都要学会打印一句“Hello World！”，而这里我将要讲解一下如何构建一个像我这样的博客。

#### 运行环境：windows10

#### [Jekyll中文文档](http://jekyll.com.cn/),[Jekyll英文文档](https://jekyllrb.com/),[Jekyll Themes](http://jekyllthemes.org/)

## 1.安装Ruby和DevKit

#### 1.1从[RubyInstaller for Windows](https://rubyinstaller.org/)下载Ruby2.3.3(x64)和DevKit-mingw64-64-4.7.2-20130224-1432-sfx.exe，安装的时候注意将ruby的可执行文件加入到环境变量PATH中，如下图
![添加环境变量]({{site.url}}/img/2017-01-21-01.png)

#### 1.2在命令行进入解压后的DevKit文件夹，并执行如下命令：
{% highlight bash %}
cd F:\DevKid
ruby dk.rb init
ruby dk.rb install
ruby -v #查看ruby版本，以确认安装成功
gem -v  #查看gem版本，以确认安装成功
{% endhighlight bash %}
![命令]({{site.url}}/img/2017-01-21-02.png)

## 2.安装Jekyll

#### 在命令行执行如下命令：
{% highlight bash %}
gem install jekyll
jekyll -v  #查看jekyll版本，以确认安装成功
{% endhighlight bash %}

## 3.下载Jekyll模板

#### 你可以从[Jekyll Themes](http://jekyllthemes.org/)或者其他地方下载你喜欢的模板到本地。

#### 进入模板目录，执行如下命令：
{% highlight bash %}
cd E:\Moon
jekyll server
{% endhighlight bash %}
![运行]({{site.url}}/img/2017-01-21-03.png)

#### 在浏览器输入http://localhost:4000, 即可看到效果
![效果]({{site.url}}/img/2017-01-21-04.png)


## 问题：

#### 1.执行jekyll server命令后可能会出现如下错误
{% highlight bash %}
D:/Ruby23-x64/lib/ruby/2.3.0/rubygems/core_ext/kernel_require.rb:55:in `require': cannot load such file -- bundler (LoadError)
    from D:/Ruby23-x64/lib/ruby/2.3.0/rubygems/core_ext/kernel_require.rb:55:in `require'
    from D:/Ruby23-x64/lib/ruby/gems/2.3.0/gems/jekyll-3.3.1/lib/jekyll/plugin_manager.rb:34:in `require_from_bundler'
    from D:/Ruby23-x64/lib/ruby/gems/2.3.0/gems/jekyll-3.3.1/exe/jekyll:9:in `<top (required)>'
    from D:/Ruby23-x64/bin/jekyll:22:in `load'
    from D:/Ruby23-x64/bin/jekyll:22:in `<main>'
{% endhighlight bash %}

#### 解决：执行以下命令
{% highlight bash %}
gem install bundler
{% endhighlight bash %}

#### 2.执行jekyll server命令后可能会出现如下错误
{% highlight bash %}
D:/Ruby23-x64/lib/ruby/gems/2.3.0/gems/bundler-1.14.3/lib/bundler/spec_set.rb:87:in `block in materialize: Could not find i18n-0.7.0 in any of the sources (Bundler::GemNotFound)
...
{% endhighlight bash %}

#### 解决：执行以下命令
{% highlight bash %}
bundle install
{% endhighlight bash %}

#### 3.执行jekyll server命令后可能会出现如下错误
{% highlight bash %}
D:/Ruby23-x64/lib/ruby/gems/2.3.0/gems/bundler-1.14.0/lib/bundler/resolver.rb:376:in `block in verify_gemfile_dependencies_are_found!': Could not find gem 'jekyll (~> 3.2.1) x64-mingw32' in any of the gem sources listed in your Gemfile. (Bundler::GemNotFound)
    ......
{% endhighlight bash %}

#### 解决：查看Gemfile文件里的jekyll版本是否与安装的版本一致，若不一致，修改为此时安装的版本

#### 4.执行jekyll server命令后可能会出现如下类似错误
{% highlight bash %}
D:/Ruby23-x64/lib/ruby/gems/2.3.0/gems/bundler-1.14.0/lib/bundler/resolver.rb:376:in `block in verify_gemfile_dependencies_are_found!': Could not find gem 'jekyll-sitemap x64-mingw32' in any of the gem sources listed in your Gemfile. (Bundler::GemNotFound)
{% endhighlight bash %}

#### 解决：执行以下命令
{% highlight bash %}
gem install jekyll-sitemap
{% endhighlight bash %}

## 多说评论

#### 1.注册多说

#### 2.复制以下代码到post.html的<div class="content"></div>下
{% highlight html %}
    <!-- 多说评论框 start -->
    <div class="ds-thread" data-thread-key="请将此处替换成文章在你的站点中的ID" data-title="请替换成文章的标题" data-url="请替换成文章的网址"></div>
    <!-- 多说评论框 end -->
    <!-- 多说公共JS代码 start (一个网页只需插入一次) -->
    <script type="text/javascript">
    var duoshuoQuery = {short_name:"多说名称"};
    (function() {
      var ds = document.createElement('script');
      ds.type = 'text/javascript';ds.async = true;
      ds.src = (document.location.protocol == 'https:' ? 'https:' : 'http:') + '//static.duoshuo.com/embed.js';
      ds.charset = 'UTF-8';
      (document.getElementsByTagName('head')[0]
      || document.getElementsByTagName('body')[0]).appendChild(ds);
      })();
      </script>
      <!-- 多说公共JS代码 end -->
{% endhighlight html %}

#### 这段代码有四处要改：

#### ·data-thread-key填上\{\{page.id\}\}

#### ·data-title填上\{\{page.title\}\}

#### ·data-url填上\{\{page.url\}\}

#### ·short_name填上注册多说账号时的二级短域名

## 友言评论

#### 近日，多说突然发出公告，不再提供服务。好吧，那么我们可以用友言。个人觉得友言的设置操作要比多说简单，所以在此就不再写怎么用了。当然，我也觉得友言的功能在某些方面不如多说。

## 网易云跟帖
#### 不知道为什么，有言评论莫名奇妙加载不出来了，在忍受了一段没有评论模块的状态之后，我又去搜寻了一些其他的评论插件，当然网易云跟帖入了我的眼。本来不打算把跟换网易云跟帖这事写进来的，因为这些操作跟上面的一样，但是昨天突然收到一个小伙伴的邮件向我请教这件事，所以特地在这里写一下详细操作，避免其他小伙伴也出现这样的问题。

#### 1.注册，填写站点信息。(这是你使用所有的评论插件都需要做的)

#### 2.复制相应代码，当然这里要用web代码!!!,粘贴到post.html中去。(我在这里详细说一下，post.html里面的所有代码就是生成你文章界面的代码，所有要给文章加评论等修改文章界面格式的代码就要修改这里的代码)


## 分享功能

#### 修改meta.html页面

#### 可以根据你的需求将<i>标签里的class替换为自己需要的代码,具体代码可以看[FontAwesome图标](http://www.yeahzan.com/fa/facss.html),并将href的值改为相应代码

## 部署到github

#### 1.申请并建立仓库
![]({{site.url}}/img/2017-01-21-05.png)
#### 仓库的名字必须以io为后缀

#### 2.进入到项目目录，在命令行运行如下命令：
{% highlight bash %}
git init  #对已有项目进行git初始化
{% endhighlight bash %}

#### 运行命令后会自动创建一个master分支

#### 3.发布
{% highlight bash %}
git add .
git commit -a -m "v0.0.1 first blog" #提交所有的修改到本地仓库
{% endhighlight bash %}

#### 3.上传到github
{% highlight bash %}
git remote add origin https://github.com/(github用户名)/(jekyll项目名称).git
git push origin master
{% endhighlight bash %}

#### 4.修改后上传
{% highlight bash %}
git add .                     #增加所有新增的文件到项目中
git commit -a -m "提交注释"    #提交所有修改
git push origin master        #将修改提交到远程github服务器
{% endhighlight bash %}

## 绑定自己的域名

#### 1.在项目根目录创建一个CNAME文件，写上自己购买的域名

#### 2.在DNS中增加一条记录，如下图
![]({{site.url}}/img/2017-01-21-06.png)

#### 注意

##### 文件的名字格式应为xxxx-xx-xx-名字.markdown,名字不能是中文，否则将找不到文件！

## 将github同步到coding

#### 大家可能都知道github把百度的爬虫给屏蔽了，所以很难被百度收录，那么我们可以将博客放一份到coding上。二来为了防止万一仓库某一天不能用了，我们也有个备份，有备无患。好了废话不多说了，开始着手

#### 1.在coding上注册并创建新项目
![]({{site.url}}/img/coding01.png)

#### 2.填写项目名、项目描述、项目属性、导入仓库的地址、创建项目
![]({{site.url}}/img/coding02.png)

#### 3.过几分钟项目就被复制到coding上了
![]({{site.url}}/img/coding03.png)

#### 4.设置Pages服务

#### 部署来源填写和github的一致
![]({{site.url}}/img/coding04.png)

#### 5.更改git下的config文件
{% highlight bash %}
[remote "origin"]
    url = https://github.com/chengzequn/chengzequn.github.io.git
    url = https://git.coding.net/chengzequn/chengzequn.git
    fetch = +refs/heads/*:refs/remotes/origin/*
{% endhighlight bash %}

#### 6.更改项目下的_config.xml文件
{% highlight bash %}
deploy:
    type: git
    repo:
    github: https://github.com/chengzequn/chengzequn.github.io.git
    coding: https://git.coding.net/chengzequn/chengzequn.git
    bransh: master
{% endhighlight bash %}

#### 7.下次更改后push就可以同时上传同步到github和coding上
