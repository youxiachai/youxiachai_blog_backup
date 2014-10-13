title: Pomelo 一周之旅--星期三
date: 2013-06-05
tags: [node, pomelo]
---

##前言##
今天，我们谈谈Pomelo的会话机制
<!--more-->

##会话机制##
从pomelo 的api 文档我们可以发现，有三个大类与session有关

* LocalSessionService
* SessionService
* LocalSession

###Session 介绍###
从pomelo 的api文档中我们可以总结出：

SessionService 是只存在于前端服务器（frontend），session 以每个客户端请求自增1的形式生成 ,用于管理连接 pomelo的客户端，如果在前端服务器不进行相关控制对于每个请求都会产生一个Session，就是说客户端都会在前端的服务器（frontend）里的sessionService产生一个会话，值得注意的是自pomelo0.4.x支持了支持同一账号多处登录，所以seesionService 里面的session 对应的是一个session数组，如果，对session不做任何处理的话，没刷新一次页面，都会对这个session 数组自增 1.从暴露的api，我们可以看出，这个SessionService 可以用于对连接在前端服务器的客户端，踢下线，或者利用session id 直接在前端服务器发消息给客户端。

LocalSessionService 由于SessionService只存在于前端服务器（frontend），如果想在后端服务器（Backend）操作SessionService的话，就需要一个代理类（因为这是两个进程），从源码中可以看到，这个就是从前端服务器复制出来用于backend进行操作的SessionService，主要用于获取踢客户端下线，或者获取相关客户端Session Id。

LocalSession 是用于我们自定义的id 与全局 sessionService进行管理的类。主要用于服务端对客户端之间会话的管理。

从api 文档暴露的接口我们可以得知主要作用：

* 让我们自定义的id 可以绑定到客户端与服务端之间的会话，用于管理客户端的状态。例如，利用绑定的id实现控制对客户端进行踢掉，监听session的关闭事件。

* localSession 还提供了一个K/V 的数据存取操作 需要用push或者pushAll 对sessionSerive进行更新。但是，根据官方的回复，不建议把session当做内存库。



###Session FAQ##
1. Session 是否适合当初内存数据库使用？
> session里存的是只读的用户状态数据， 不会同步到数据库。 内存数据的同步是另一个模块实现的。
不要把session当成内存数据库使用， session会在各服务器节点间传递， 因此session的内容越少、越轻量越好。<br/>问题及答案来自于：[谁能帮我介绍一下 pomelo的session 用法？](https://github.com/NetEase/pomelo/issues/84)

2. localSessionService和sessionService的区别？
> 如果看完本章节还是不明白的话？<br/>可以参见[有没有文档具体解释localSessionService和sessionService？](https://github.com/NetEase/pomelo/issues/199)

3. 如何在connector以外的服务器中获取全局session？
> [如何在connector以外的服务器中获取全局session](https://github.com/NetEase/pomelo-cn/issues/31)

4. uid 不能为object
> 问题来源于 ： [fail to send message by uid for session not exist](https://github.com/NetEase/pomelo-cn/issues/90)



##扩展阅读##
ES 5的 bind()方法。

在pomelo的一些demo 里面可能会不理解这么一句话
``` js
session.on('closed', onUserLeave.bind(null, this.app));

var onUserLeave = function(app, session) {
	if(!session || !session.uid) {
		return;
	}
	app.rpc.chat.chatRemote.kick(session, session.uid, app.get('serverId'), session.get('rid'), null);
};
```

你可能会有这样的疑问

1. 为什么onUserLeave 会有两个参数? 

2. 函数的bind() 第一个参数null 什么什么意思，结合第一个问题，这里就传了一个参数？那么那个session 还是怎么传过来的？


这个函数 bind() 方法是ES 5 新增的一个特性。用于将函数绑定到某个对象上。

那么这里bind()的方法的作用是什么呢？

要明白这里的bind()的用法，首先我们需要了解一种函数式编程技术---柯里化（currying）

> [Wiki是这么定义的: ](http://zh.wikipedia.org/wiki/%E6%9F%AF%E9%87%8C%E5%8C%96)在计算机科学中，柯里化（Currying），是把接受多个参数的函数变换成接受一个单一参数（最初函数的第一个参数）的函数，并且返回接受余下的参数而且返回结果的新函数的技术。

为了帮助理解这里写一个小例子(来自于Javascript 权威指南第六版 p191)
``` js
var sum = function(x, y){
    return x+y;
}
//创建一个类似于sum的新函数，但this的值绑定到null
//并且第一个参数绑定到1， 这个新函数的期望只传入一个实参
var succ = sum.bind(null, 1);

console.log(succ(2)); //输出 3； x 绑定到1，并传入2作为实参

//另外一个做累计计算的函数
var sum2 = function(y ,z){
    return this.x + y + z;
}

//绑定this 和 y
var bindSum2 = sum2.bind({x : 1}, 2);
console.log(bindSum2(3)); //输出 6； this.x 绑定到1，y绑定到2， z 绑定到3.
```

上面这个例子就是柯里化的应用了，现在我们回到pomelo看下，chatpomelo例子里面怎么使用这个柯里化技术。

现在，应该能解决开头的第一个问题了，那么第二个问题，我们需要阅读一下pomelo的源码

阅读源码[sessionService.js 464-478](https://github.com/NetEase/pomelo/blob/master/lib/common/service/sessionService.js)

从这十几行代码和柯里化的知识，我们就能够明白为什么onUserLeave为什么会有两个参数了。
``` js
var onUserLeave = function(app,  session){
   
}
```

最后的参数是通过柯里化的技术把单参数 session 传到我们多参数函数里面。


这里有一篇讲柯里化技术挺有趣的文章：
[http://www.zhangxinxu.com/wordpress/2013/02/js-currying/](http://www.zhangxinxu.com/wordpress/2013/02/js-currying/)


补充内容：
Session

Session可以看成一个简单的key/value对象，主要作用是维护当前玩家状态信息，比如：当前玩家的id，所连的frontend服务器id等。Session对象由客户端所连接的frontend服务器维护。在分发请求给backend服务器时，frontend服务器会克隆session，连同请求一起发送给backend服务器。所以，在backend服务器上，session应该是只读的，或者起码只是本地读写的一个对象。任何直接在session上的修改，只对本服务器进程生效，并不会影响到该玩家的全局状态信息。如需修改全局session里的状态信息，需要调用frontend服务器提供的RPC服务。

