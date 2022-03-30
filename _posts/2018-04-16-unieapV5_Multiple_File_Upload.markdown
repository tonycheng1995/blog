---
layout: post
title:  "unieapV5多文件上传"
date:   2018-04-16  +0800
toc: true
category: jekyll update
tags: JS
excerpt: 对于unieapV5的文件上传默认只是单文件上传，如果想要多文件上传，那么就要自己修改
---
#### 对于unieapV5的文件上传默认只是单文件上传，如果想要多文件上传，那么就要自己修改
#### FileInput控件未提供multiple属性的设置，在源码中添加属性也无效，所以需要使用js为控件添加此属性
#### page_load()方法
{% highlight js %}
//为控件添加mutile属性
$("#"+unieap.getRealId("fileInput1")).attr("multiple","multiple");
//获取多文件上传时每个文件的值
$("#"+unieap.getRealId("fileInput1")).change(function () {
	var length=this.files.length;
	for(var i=0;i<length;i++ ){
		alert(this.files[i].name);
	}
});
{% endhighlight js %}

#### FileInput需要放在form中，并且enctype="multipart/form-data"。(这些不用我多说什么)
#### 提交按钮调用后端接口。(这些也不用我多说)
{% highlight js %}
var dc=new unieap.ds.DataCenter();
dc.addHeaderAttribute("formId", "form1");
view.processor.updateFile(dc);
{% endhighlight js %}

#### 后端BPO中的代码(这个也不用我多说什么)
{% highlight java %}
@Override
@POST
@Consumes(MediaType.MULTIPART_FORM_DATA)
@Path("/uploadFile")
public void updateFile(@Context HttpServletRequest request) {
	boolean isMultipart = ServletFileUpload.isMultipartContent(request);
	if(isMultipart){
		FileItemFactory factory=new DiskFileItemFactory();
		ServletFileUpload upload=new ServletFileUpload(factory);
		upload.setHeaderEncoding("UTF-8");
		List<FileItem> list=null;
		try {
			list=upload.parseRequest(request);
			for(FileItem file:list){
				if(file.isFormField()){
			//普通表单值
			System.out.println(file.getString());
			//TODO
				}else{
					String fileName=file.getName();
					System.out.println(fileName);
			InputStream input = file.getInputStream();
			//TODO进行一些文件上传操作
				}
			}
		} catch (FileUploadException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}
{% endhighlight java %}