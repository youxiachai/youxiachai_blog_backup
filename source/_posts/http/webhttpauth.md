title: 基于HTTP 协议认证介绍与实现
date: 2013-06-15
tags: http
---

##导言##
一直对http 的头认证有兴趣,就是路由器的那种弹出对话框输入账号密码怎么实现一直不明白,最近,翻了一下http 协议,发现这是一个RFC 2617的实现,所以写篇文章介绍一下吧.
<!--more-->

###Http基本认证###
这是一个用于web浏览器或其他客户端在请求时提供用户名和密码的登录认证,要实现这个认证很简单:

我们先来看下协议里面怎么定义这个认证的.
1. 编码: 将用户名 追加一个 冒号(':')接上密码,把得出的结果字符串在用Base64算法编码.

2. 请求头: Authorization: 认证类型 编码字符串

来看一下客户端如何发起请求
例如,有一个用户名为:tom, 密码为:123456 怎么认证呢?

步骤如下
1. 编码 
> Base64('tom:123456') == dG9tOjEyMzQ1Ng==;

2. 把编码结果放到请求头当中
> Authorization: Basic dG9tOjEyMzQ1Ng==

请求样例
客户端
``` shell
GET / HTTP/1.1
Host: localhost
Authorization: Basic dG9tOjEyMzQ1Ng
```

服务端应答
``` shell
HTTP/1.1 200 OK
Date: Thu, 13 Jun 2013 20:25:37 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 53
```

如果没有认证信息
``` shell
HTTP/1.1 401 Authorization Required
Date: Thu, 13 Jun 2013 20:25:37 GMT
WWW-Authenticate: Basic realm="Users"
```
验证失败的时候,响应头加上WWW-Authenticate: Basic realm="请求域".

这种http 基本实现,几乎目前所有浏览器都支持.不过,大家可以发现,直接把用户名和密码只是进行一次base64 编码实际上是很不安全的,因为对base64进行反编码十分容易,所以这种验证虽然简便,但是很少会在公开访问的互联网使用,一般多用在小的私有系统,例如,你们家里头的路由器,多用这种认证方式.


###Http 摘要认证###
这个认证可以看做是基本认证的增强版本,使用随机数+密码进行md5,防止通过直接的分析密码MD5防止破解.
摘要访问认证最初由 RFC 2069 (HTTP的一个扩展：摘要访问认证)中被定义
加密步骤:

1. ![](/images/webhttp/ha1.png)

2. ![](/images/webhttp/ha2.png)

3. ![](/images/webhttp/res.png)

后来发现,就算这样还是不安全(md5 可以用彩虹表进行攻击),所以在RFC 2617入了一系列安全增强的选项；“保护质量”(qop)、随机数计数器由客户端增加、以及客户生成的随机数。这些增强为了防止如选择明文攻击的密码分析。

>![](/images/webhttp/d1.png)

1. 如果 qop 值为“auth”或未指定，那么 HA2 为
> ![](/images/webhttp/d2.png)

2.  如果 qop 值为“auth-int”，那么 HA2 为
> ![](/images/webhttp/d3.png)
 
3.  如果 qop 值为“auth”或“auth-int”，那么如下计算 response：
> ![](/images/webhttp/d4.png)

4. 如果 qop 未指定，那么如下计算 response：
> ![](/images/webhttp/d5.png)



好了,知道加密步骤,下面我们用文字来描述一下;

最后,我们的response 由三步计算所得.
1. 对用户名、认证域(realm)以及密码的合并值计算 MD5 哈希值，结果称为 HA1。
> HA1 = MD5( "tom:Hi!:123456" )
       = d8ae91c6c50fabdac442ef8d6a68ae8c

2. 对HTTP方法以及URI的摘要的合并值计算 MD5 哈希值，例如，"GET" 和 "/index.html"，结果称为 HA2。
> HA2 = MD5( "GET:/" ) = 71998c64aea37ae77020c49c00f73fa8

3. 最后生成的响应码
> Response = MD5("d8ae91c6c50fabdac442ef8d6a68ae8c:L4qfzASytyQJAC2B1Lvy2llPpj9R8Jd3:00000001:c2dc5b32ad69187a<br />:auth:71998c64aea37ae77020c49c00f73fa8") = 2f22e6d56dabb168702b8bb2d4e72453;

RFC2617 的安全增强的主要方式:

发起请求的时候,服务器会生成一个密码随机数(nonce)(而这个随机数只有每次"401"相应后才会更新),为了防止攻击者可以简单的使用同样的认证信息发起老的请求,于是,在后续的请求中就有一个随机数计数器(cnonce),而且每次请求必须必前一次使用的打.这样,服务器每次生成新的随机数都会记录下来,计数器增加.在RESPONSE 码中我们可以看出计数器的值会导致不同的值,这样就可以拒绝掉任何错误的请求.



请求样例(服务端 qop 设置为"auth")

客户端 无认证
``` shell
GET / HTTP/1.1
Host: localhost
```

服务器响应(qop 为 'auth')

``` shell
HTTP/1.1 401 Authorization Required
Date: Thu, 13 Jun 2013 20:25:37 GMT
WWW-Authenticate: Digest realm="Hi!", nonce="HSfb5dy15hKejXAbZ2VXjVbgNC8sC1Gq", qop="auth"
```

客户端请求(用户名: "tom", 密码 "123456")

``` shell
GET / HTTP/1.1
Host: localhost
Authorization: Digest username="tom",
                     realm="Hi!",
                     nonce="L4qfzASytyQJAC2B1Lvy2llPpj9R8Jd3",
                     uri="/",
                     qop=auth,
                     nc=00000001,
                     cnonce="c2dc5b32ad69187a",                     response="2f22e6d56dabb168702b8bb2d4e72453"
```

服务端应答

``` shell
HTTP/1.1 200 OK
Date: Thu, 13 Jun 2013 20:25:37 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 53
```

注意qop 设置的时候慎用:auth-int,因为一些常用浏览器和服务端并没有实现这个协议.


