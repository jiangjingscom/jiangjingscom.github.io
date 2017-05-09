---
title: 一次http请求
date: 2017-04-17 18:13:27
categories: Font-end
---
好久没有写博客了，记录几天前的一个bug，顺便回顾一些之前学的HTTP知识。
客户之前要求浏览器支持360和chrome，对版本没有什么要求。想到360极速模式使用的webkit内核，平常代码运行在又chrome上，我感觉可以任性了！但是最近客户改需求需要运行在IE10及以上，小伙伴们开始在IE10上测试。其中一处bug：某次HTTP 请求在chrome上返回200，但在IE10上返回401。
在chrome中运行，HTTP 请求详细信息截图如下：

<img src="../../../../assets/img/4-17-1.png"   align=center />

<!--more-->

在IE10下HTTP请求响应码返回401，表示身份信息有问题，请求失败：

<img src="../../../../assets/img/4-17-2.png"   align=center />

查看request header：

<img src="../../../../assets/img/4-17-3.png"   align=center />

发现设置在headers中的的authorization字段不见了，在chrome上存在authorization 字段,那么IE10上应该存在，为什么找不到？看到桌面绿色的fiddler，想到它可以抓到很多HTTP 信息，可能会得到其他有用的信息。
运行fiddler ，找到目标session：

<img src="../../../../assets/img/4-17-4.png"   align=center />

得到IE10下详细的返回信息：

<img src="../../../../assets/img/4-17-5.png"   align=center />

所以，authorization还是有的，只是IE10没有显示出来，而且变成了Authorization，首字母大写；应该是后端验证出现问题，将bug报给后端被解决。

#### HTTP基础知识([HTTP basics](https://www.ntu.edu.sg/home/ehchua/programming/webprogramming/HTTP_Basics.html))
一次完整的请求过程：
1.域名解析
2.建立TCP连接，三次握手
3.Web浏览器向Web服务端发送HTTP请求报文
4.浏览器解析HTML代码，并请求HTML代码中的资源
6.浏览器对页面进行渲染呈献给用户
7.断开TCP连接

#### HTTP的请求方法([HTTP/1.1: Method Definitions](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html))
一些get请求方式：1. 直接输入某个地址 2. 点击链接 3. 表单默认提交方式

#### 关于HTTP 响应码([HTTP Status Code](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) )
1xx：#临时响应# 代表请求已经被接受，但需要继续处理。
　　100 Continue — 服务器仅接收到部分请求，但是一旦服务器并没有拒绝该请求，客户端应该继续发送其余的请求。
　　101 Switching Protocols — 服务器转换协议：服务器将遵从客户的请求转换到另外一种协议。
　　102 Processing — 由WebDAV(RFC 2518)扩展的状态码，代表处理将被继续执行。
2xx：#成功# 代表请求已经被服务器接收、理解、并接受
　　200 OK：请求成功(其后是对GET和POST请求的应答文档。)
　　201 Created — 请求被创建完成，同时新的资源被创建。
　　202 Accepted — 供处理的请求已被接受，但是处理未完成。
　　203 Non-authoritative Information — 文档已经正常地返回，但一些应答头可能不正确，因为使用的是文档的拷贝。
　　204 No Content — 没有新文档。浏览器应该继续显示原来的文档。如果用户定期地刷新页面，而Servlet可以确定用户文档足够新，这个状态代码是很有用的。
　　205 Reset Content — 没有新文档。但浏览器应该重置它所显示的内容。用来强制浏览器清除表单输入内容。
　　206 Partial Content — 客户发送了一个带有Range头的GET请求，服务器完成了它。
　　207 Multi-Status — 由WebDAV(RFC 2518)扩展的状态码，代表之后的消息体将是一个XML消息，并且可能依照之前子请求数量的不同，包含一系列独立的响应代码。
3xx: #重定向# 代表客户端需要采取进一步的操作才能完成请求
　　300 Multiple Choices — 多重选择。链接列表。用户可以选择某链接到达目的地。最多允许五个地址。
　　301 Moved Permanently — 所请求的页面已经转移至新的url。
　　302 Found — 所请求的页面已经临时转移至新的url。
　　303 See Other — 所请求的页面可在别的url下被找到。
　　304 Not Modified — 未按预期修改文档。客户端有缓冲的文档并发出了一个条件性的请求(一般是提供If-Modified-Since头表示客户只想比指定日期更新的文档)。服务器告诉客户，原来缓冲的文档还可以继续使用。
　　305 Use Proxy — 客户请求的文档应该通过Location头所指明的代理服务器提取。
　　306 Unused — 此代码被用于前一版本。目前已不再使用，但是代码依然被保留。
　　307 Temporary Redirect — 被请求的页面已经临时移至新的url。
4xx：#客户端错误# 代表客户端可能发生了错误，阻碍了服务器的处理，
　　400 Bad Request — 服务器未能理解请求或是请求参数有误。
　　401 Unauthorized — 被请求的页面需要用户名和密码。
　　402 Payment Required — 此代码尚无法使用(为了将来可能的需求而预留的。)
　　403 Forbidden — 对被请求页面的访问被禁止。
　　404 Not Found — 服务器无法找到被请求的页面。
　　405 Method Not Allowed — 请求中指定的方法不被允许。
　　406 Not Acceptable — 服务器生成的响应无法被客户端所接受。
　　407 Proxy Authentication Required — 用户必须首先使用代理服务器进行验证，这样请求才会被处理。
　　408 Request Timeout — 请求超出了服务器的等待时间。
　　409 Conflict — 由于冲突，请求无法被完成。
　　410 Gone — 被请求的页面不可用。
　　411 Length Required"Content-Length — " 未被定义。如果无此内容，服务器不会接受请求。
　　412 Precondition Failed — 请求中的前提条件被服务器评估为失败。
　　413 Request Entity Too Large — 由于所请求的实体的太大，服务器不会接受请求。
　　414 Request-url Too Long — 由于url太长，服务器不会接受请求。当post请求被转换为带有很长的查询信息的get请求时，就会发生这种情况。
　　415 Unsupported Media Type — 由于媒介类型不被支持，服务器不会接受请求。
　　416 — 服务器不能满足客户在请求中指定的Range头。
　　417 Expectation Failed
5xx： #服务器错误# 代表服务器在处理请求的过程中有错误或者异常状态发生，也有可能是服务器意识到以当前的软硬件资源无法完成对请求的处理。
　　500 Internal Server Error — 请求未完成。服务器遇到不可预知的情况。
　　501 Not Implemented — 请求未完成。服务器不支持所请求的功能。
　　502 Bad Gateway — 请求未完成。服务器从上游服务器收到一个无效的响应。
　　503 Service Unavailable — 请求未完成。服务器临时过载或当机。
　　504 Gateway Timeout — 网关超时。
　　505 HTTP Version Not Supported — 服务器不支持请求中指明的HTTP协议版本。

#### HTTP1.1与HTTP1.1的区别([Key Differences between HTTP/1.0 and HTTP/1.1](http://www8.org/w8-papers/5c-protocols/key/key.html))
最大的区别：HTTP1.0规定浏览器与服务器只保持短暂的链接，每次请求需要建立TCP连接，完成后立即断开；HTTP1.1支持持久连接，一个连接可用于多次响应请求交换，允许客户端不用等待上一次结果返回，就可以发送下一个请求。

总结：博客要坚持写，小问题也可以写写。