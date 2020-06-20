---
title: fiddler抓包工具以及跨域
categories:
  - 前端
tags:
  - 工具
---

<!--more-->

常用的抓包工具有很多，小到常用 web 调试工具 fireBug， 大到抓包工具 wireShark。fireBug 虽然可抓包，但对模拟 http 请求功能、分析 http 请求详情支持不够且若刷新页面，所有修改不会被保存。wireShark 比较庞大，对只需抓取 http 请求应用来说大材小用。而 fiddler 是客户端和服务器端的 http 代理，能够记录客户端和服务器端间所有 HTTP 请求，针对特定 HTTP 请求，分析请求数据、设置断点、调试 web 应用、修改请求数据，修改服务器响应数据。功能强大，是 web 调试利器。

代理：客户端请求经过 fiddler，然后转发到服务器。反之，响应数据也会先经过 fiddler 在发送到客户端。基于此模式，fiddler 支持所有可设置 http 代理为 127.0.0.1::8888 的浏览器和应用程序

## 扩展工具的用法 MiniFiddler

[学习资料](https://www.cnblogs.com/yyhh/p/5140852.html)

插件安装好后，控制台 tab 会出现一个 MiniFiddler 栏

### 记录客户端和服务器端间所有 HTTP 请求

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200619103605922.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)
重点注意请求方式，资源类型，地址，状态码，点击查看详情，则如下图出现该请求的详细信息。

### 针对特定请求，分析请求数据

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200619103317321.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)
单击请求地址栏可以切换编码方式（分别为 urlencoding 和 urldecoding）

### 功能区：过滤器+阻塞规则

过滤器：根据 css，图片，js 资源去过滤请求
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200619104024837.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)
阻塞规则
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200619105450165.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xqbGhuaWNr,size_16,color_FFFFFF,t_70)
URL Blocking 可以让我们添加需要阻塞的 URL 请求规则，关闭控制面板阻塞规则依旧生效，多个页面的阻塞规则相互不影响

### 设置断点

1.打断点后，fiddler 可修改 http 请求头信息（UA，Cookie，Referer），通过伪造相应信息达到调式、模拟用户真实请求的目标 2.构造请求数据，突破表单验证以及限制，避免 js 和表单限制影响相关调试 3.拦截响应数据，修改响应体

js 通过模拟真实服务器端响应，不必等待接口，降低联调难度。

两种断点方式
1.fiddler 菜单-> rules->automatic Breakpoints->断点方式（断点对之后所有的 http 请求有效） 2.命令行输入 bpater xx、bpv（针对特定类型的请求）

两个断点位置
1.before response：发请求后，fiddler 中转请求前，此时可修改请求数据
2.after response： 响应后，fiddler 中转响应前，此时可以修改响应数据

## Fiddler 命令

### select

选择所有相依类型（content-type）为置顶类型的 HTTP 请求

### allbut

选择响应类型不是指定类型的 HTTP 请求

### ?text

选择 URL 匹配问号后字符的全部 HTTP 请求

### >size <size

选择响应大小大于、小于某个大小（b）的 HTTP 请求

### =status

### @host

选择 host 含指定 host 的 HTTP 请求

## 跨域

JSON 跨域只能实现 get 请求，因为 srcipt 请求 src 所指向的 js 脚本是 get 方式。

### JSONP 跨域

原理：ajax 因同源策越限制，不可跨域请求，而 script 标签中的 src 可以跨域请求 js 脚本。利用此特性，服务器返回可执行的某个函数的 js 代码，在 src 中调用，就实现了跨域。

代码实现

```
<script type="text/javascript">
	$(document).ready(function(){
        function jsonhandle(data){
            alert("age:" + data.age + "name:" + data.name);
        }
		var url = "http://www.practice-zhao.com/student.php?id=1&callback=jsonhandle";
		var obj = $('<script><\/script>');
		obj.attr("src",url);
		$("body").append(obj);
	});
</script>
```

jq 提供便于使用 jsonp 的方式

```
<script type="text/javascript">
	$(document).ready(function(){
		$.ajax({
			type : "get",
			async: false,
			url : "http://www.practice-zhao.com/student.php?id=1",
			dataType: "jsonp",
			jsonpCallback: "jsonhandle",//要执行的回调函数
			success : function(data) {
				alert("age:" + data.age + "name:" + data.name);
			}

		});
	});
</script>
```

### CORS 跨域

需要浏览器和服务器支持，浏览器一旦发现 ajax 请求跨域，就会自动添加一些附加头信息，关键是服务器要实现 CORS 接口。
