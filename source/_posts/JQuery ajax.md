---
title: JQuery Ajax
layout: page
tags: [JavaScript,ajax]
date: 2015-08-16 14:03:07
---

### XHR用法
使用XHR对象时，要调用的第一个方法是open(),它接受3个参数，要发送的请求类型，请求的url，表示是否异步发送请求的布尔值。

例如：xhr.open("get","example.php",false);
注意：调用oepn()方法并不会真正发送请求，而只是启动一个请求以备发送。要发送特定请求，

例如：xhr.open("get","example.txt",false);
      xhr.send(null);

### XHR取得响应

响应数据会自动填充XHR对象的属性，属性简介：
responseText:作为响应主体被返回的文本。 responseXML：获得XML形式的返回数据。
status:响应的HTTP状态 statusText:HTTP状态的说明
检测XHR对象的readyState属性，该属性表示请求/响应的过程的当前活动阶段，取值范围为0~4,4表示完成。
例如：

```bash
var xhr=creatXHR();
xhr.onreadystatechange=function(){//readyState值改变即触发该事件，该事件必须在调用open()方法之前调用。
	if(xhr.readyState==4){
	if((xhr.status>=200&&xhr.status<300)||xhr.status==304){
	alert(xhr.responseText);
	}else{
	alert("error"+xhr.status);
  }
}
};
xhr.open("get","example.txt",true);
xhr.send(null);
```

<!-- more -->

### HTTP请求

一个完整的HTTP请求通常包括以下7个步骤：
1：建立TCP连接
2：Web浏览器向Web服务器发送请求命令
3：Web浏览器发送请求头信息
4：Web服务器应答
5：Web服务器发送应答头信息
6：Web服务器向浏览器发送数据
7：Web服务器关闭TCP连接

一个HTTP请求一般由四部分组成：
1：HTTP请求的方法或动作，比如post,get等。
2：请求url
3：请求头
4：请求体，也就是正文。

一个HTTP响应一般由3部分组成：
1：一个数字和文字组成的状态码
2：响应头
3：响应体

**注意：使用setRequestHeader()方法可以设置自定义的请求头部信息，接受两个参数，头部字段的名称和头部字段的值，但是必须在调用open()方法且调用send()方法之前调用它。**

```bash
var xhr=creatXHR();
xhr.onreadystatechange=function(){//readyState值改变即触发该事件，该事件必须在调用open()方法之前调用。
	if(xhr.readyState==4){
	if((xhr.status>=200&&xhr.status<300)||xhr.status==304){
	alert(xhr.responseText);
	}else{
	alert("error"+xhr.status);
  }
}
};
xhr.open("get","example.txt",false);
xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded");
xhr.send(null);
```

### jQuery.ajax([settings])

常用参数：

* type：类型，post,get等
* url:发送的地址,必选
* data:是一个对象，连同发送到数据库的数据
* dataType:预期服务器返回的数据类型，如果不指定，Jquery将根据HTTP包MIEI信息来智能判断。
* success:请求成功返回的回调函数
* error:请求失败试调用此函数
* contentType：内容编码类型默认为”application/x-www-form-urlencoded”
* async:默认设置为true，所有请求均为异步请求。如果需要发送同步请求，请将此选项设置为false。注意，同步请求将锁住浏览器，用户其他操作必须等待请求完成才可以执行。

jquery与原生js对比例子：
JQuery:

```bash
$.ajax({
  type: 'POST',
  url: '/my/url',
  data: data
});
```

IE8+

```bash
var request = new XMLHttpRequest();
request.open('POST', '/my/url', true);
request.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded; charset=UTF-8');
request.send(data);
```
JQuery:

```bash
$.ajax({
  type: 'GET',
  url: '/my/url',
  success: function(resp) {

  },
  error: function() {

  }
});
```

IE9+

```bash
var request = new XMLHttpRequest();
request.open('GET', '/my/url', true);
request.onload = function() {
  if (request.status >= 200 && request.status < 400) {
    // Success!
    var resp = request.responseText;
  } else {
    // We reached our target server, but it returned an error

  }
};

request.onerror = function() {
  // There was a connection error of some sort
};

request.send();
```