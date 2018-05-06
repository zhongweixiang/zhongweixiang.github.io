---
title:  
- 轻松理解HTTP基本知识
date: 2017-06-30 11:34:50
categories: 
- HTTP
tags:
- HTTP
toc: true # 是否启用内容索引
keywords: HTTP,HTTP协议,消息报头,普通报头,请求报头,响应报头,实体报头
description: HTTP是一个属于应用层的面向对象的协议，由于其简捷、快速的方式，适用于分布式超媒体信息系统。它于1990年提出，经过几年的使用与发展，得到不断地完善和扩展。目前在WWW中使用的是HTTP/1.0的第六版，HTTP/1.1的规范化工作正在进行之中，而且HTTP-NG(Next Generation of HTTP)的建议已经提出。
---
## 引言

HTTP是一个属于应用层的面向对象的协议，由于其简捷、快速的方式，适用于分布式超媒体信息系统。它于1990年提出，经过几年的使用与发展，得到不断地完善和扩展。目前在WWW中使用的是HTTP/1.0的第六版，HTTP/1.1的规范化工作正在进行之中，而且HTTP-NG(Next Generation of HTTP)的建议已经提出。

## HTTP协议的特点
1. 支持客户/服务器模式。
2. 简单快速：客户向服务器请求服务时，只需传送请求方法和路径。请求方法常用的有GET、HEAD、POST。每种方法规定了客户与服务器联系的类型不同。由于HTTP协议简单，使得HTTP服务器的程序规模小，因而通信速度很快。
3. 灵活：HTTP允许传输任意类型的数据对象。正在传输的类型由Content-Type加以标记。
4. 无连接：无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。
5. 无状态：HTTP协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。

## HTTP协议的应用场景

- Web service、WSDL、小偷程序、采集程序、爬虫程序、socket、防盗链

## http协议执行的粗糙流程

 1. chrome搜索自身的DNS缓存

 2. 搜索操作系统自身的DNS缓存（浏览器没有找到缓存或缓存已经失效，缓存时间大概只有一分钟）

 3. 读取本地host文件

 4. 浏览器发起一个DNS 的一个系统调用
 	> 宽带运营商服务器查询本地缓存 
	> 运营商服务器发起一个迭代DNS解析请求
   		1. 运营商服务器把结果返回操作系统内核同时缓存起来
   		2. 操作系统内核把结果返回浏览器
   		3. 最终浏览器那桐了xxx.abc.com对应的ip地址
 5. 浏览器获得域名对应的ip地址后，发起http“三次握手”	

 6. TCP/IP连接建立起来，浏览器就可以向服务器发送http请求了使用了比如说，用http的get方式请求一个根域里的一个域名，协议可以采用HTTP 1.1 的一个协议

 7. 服务器端接受到了这个请求，根据路径参数，经过后端的一些处理之后，把处理后的一个结果的数据返回给浏览器，如果是网站的页面就会把完整的HTML页面代码返回给浏览器。

 8. 浏览器拿到了网站的完整的HTML页面代码，在解析和渲染这个页面的时候，里面的JS,CSS,图片静态资源，他们同样也是一个HTTP请求，同需要经过上面的主要七个步骤。

 9. 浏览器根据拿到的资源对页面进行渲染，最终把一个完整的页面呈现给用户



## http协议的组成部分

HTTP协议主要可以拆分两大模块 “请求”与“响应”， 他们都具备 HTTP头 和 正文信息

详细的消息报头解释可跳步《[HTTP协议详解之消息报头篇](/http-msg)》

### 客户端 请求报文信息

- 报文首部(HTTP头)
> 请求行：包栝请求方法，URL和HTTP协议版本
> 请求首部字段、通用首部字段、实体首部字段、其他（包括请求的各种条件和属性【值键值对】)
- 空行（CR+LF）
- 报文主体（正文信息【即用户提交的表单数据】）

### 服务端 响应报文信息

- 报文首部(HTTP头)
> 状态行：包括响应结果的HTTP协议版本、状态码、状态描述
> 响应首部字段、通用首部字段、实体首部字段、其他（包括响应的各种条件和属性【值键值对】)
- 空行（CR+LF）
- 报文主体（正文信息【即服务端返回的数据】）

### telnet执行的代码案例

```html

POST /test.php HTTP/1.1 (CRLF)	<请求行>	        
host:localhost	(CRLF)    	<请求条件和属性>	        
Accept-Language:zh-cn (CRLF)
Accept-Encoding:gzip,deflate (CRLF)
If-Modified-Since:Thu,08 Mar 201507:17:51 GMT (CRLF)
If-None-Match:W/"80b1a4c018f3c41:8317" (CRLF)
Connection:Keep-Alive (CRLF)
(CRLF)
v:1.0  <请求报文主体>

HTTP/1.1 200 OK    <状态行>                         
Server: nginx      <响应条件和属性>                          
Date: Thu,08 Mar 201507:17:52 GMT
Connection: Keep-Alive                                 
Content-Length: 23330
Content-Type: text/html
Cache-control: private

http test <响应报文主体>

```

![telnetimg](/uploads/http-telnet.jpg)



## http请求方法
- 请求方法（所有方法全为大写）有多种，各个方法的解释如下：
- GET     请求获取Request-URI所标识的资源
- POST    在Request-URI所标识的资源后附加新的数据
- HEAD    请求获取由Request-URI所标识的资源的响应消息报头
- PUT     请求服务器存储一个资源，并用Request-URI作为其标识
- DELETE  请求服务器删除Request-URI所标识的资源
- TRACE   请求服务器回送收到的请求信息，主要用于测试或诊断
- CONNECT 保留将来使用
- OPTIONS 请求查询服务器的性能，或者查询与资源相关的选项和需求

## 响应状态码
- 状态代码有三位数字组成，第一个数字定义了响应的类别，且有五种可能取值：
- 1xx：指示信息--表示请求已接收，继续处理
- 2xx：成功--表示请求已被成功接收、理解、接受
- 3xx：重定向--要完成请求必须进行更进一步的操作
- 4xx：客户端错误--请求有语法错误或请求无法实现
- 5xx：服务器端错误--服务器未能实现合法的请求
---
- 常见状态代码、状态描述、说明：
- 200 OK      //客户端请求成功
- 400 Bad Request  //客户端请求有语法错误，不能被服务器所理解
- 401 Unauthorized //请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用 
- 403 Forbidden  //服务器收到请求，但是拒绝提供服务
- 404 Not Found  //请求资源不存在，eg：输入了错误的URL
- 500 Internal Server Error //服务器发生不可预期的错误
- 503 Server Unavailable  //服务器当前不能处理客户端的请求，一段时间后可能恢复正常