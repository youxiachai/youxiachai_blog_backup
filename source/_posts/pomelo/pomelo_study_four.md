---
title: Pomelo 一周之旅--星期四
tags:
  - node
  - pomelo
categories:
  - pomelo
date: 2013-06-06 00:00:00
---

##前言##
今天我们介绍一下Channel 广播机制和RPC 的使用。
<!--more-->

##Channel##
对于一个游戏服务器，而言，把消息推送给玩家，这是一个很基础的功能，在pomelo 里面用Channel 进行消息的推送服务，要进行消息的推送，Channel提供了两种方式：

* 匿名Channel
* 具名Channel


###匿名Channel###
什么是匿名Channel？匿名Channel就是直接使用channelService进行消息推送，在api 中提供了两种方式

这种是指定用户Session 里面的绑定的UID（`session.bind(uid);`）推送到那个session uid 的方式有四个参数

* route String type
* msg Object type
* uids Array Type 
> `[{uid: userId, sid:  frontendServerId}]` 注意数组里面每个对象的属性
* cb - cb(err) 错误的回调
例子:
``` js
var uidArray = new Array();
uidObject.uid = "session uid";
uidObject.sid = "connector-server-1";
uidArray.push(uidObject);

channelService.pushMessageByUids('onMsg',{msg:msg},uidArray,function(err){
       if(err){
           console.log(err);
           return;
       }
    });
```


第二种就是把消息广播到所有连接在frontend 服务器的客户端.

* stype String type
> 指定我们需要广播的frontend 类型，注意这里不是frontend id 而是类型，例如`connector` 如果你配了多台服务器，消息会广播到所有连接在这种类型frontend的客户端上。
* route String type
> 如 'onMsg'
* msg Object type
* opts Object type
> 自0.4.x 的配置只有一个参数 opts.binded Boolean type
> true 根据session 的uid 进行广播，false 根据session的id 进行广播
* cb

``` js

channelService.broadcast('connector' ,'onMsg', msg, {binded: true}, function(err){
       if(err){
           console.log(err);
       }
    });

```
	

###具名Channel###
具名Channel 就是我们在pomelo 创建一个推送房间。用于维护需要长期订阅关系的业务，例如，聊天的频道，注意使用具名Channel 如果那个Channel不在使用，需要显式调用销毁接口。

例子：

``` js

 //创建Channel
var channelName = 'allPushChannel';
var channel = this.channelService.getChannel
//把用户添加到channel 里面
if(!!channel){
        channel.add(uid, sid);
}
```

``` js
 //根据Channel 名字推送消息
    var channelName = 'allPushChannel';
    var pushChannel = this.channelService.getChannel(channelName, false);

    pushChannel.pushMessage('onMsg',{msg: msg}, function(err){
        if(err){
            console.log(err);
        }else{
            console.log('push ok');
        }
    });
```

以上就是pomelo 有关推送的全部内容，用pomelo进行消息的推送就是这么简单！

##RPC使用##
从pomelo 框架图里面我们可以知道，pomelo 是一个多进程相互协作的环境。关于这方面的pomelo是如何实现的可以阅读官方的[Pomelo Framework](https://github.com/NetEase/pomelo/wiki/Pomelo-Framework)

这里不再对pomelo如何实现rpc 进行描述，针对原文档的一些不清晰的地方进行补充。

如何使用rpc 服务，让frontend 能够调用backend 的方法？

要实现这个目的很简单。
根据Pomelo 的相关阅读。首先在handler 同级目录下创建一个remote目录，创建一个`backendRemote`文件(具体可以参考分布式聊天的例子)

值得注意的是，我们调用远程方法的时候，第一个参数需要是Session值。
``` js
//远程服务端
backend.kick = function(uid, sid){

}

//调用远程服务的时候，我们要需要从app 获得rpc服务
var rpc = app.rpc;

//代理端的完整写法
rpc.frontend.kick = function(session, uid, sid){

}

```
以上就是pomelo 多进程相互协作的使用方式


