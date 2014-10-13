title: Pomelo 一周之旅--星期一
date: 2013-06-03
tags: [node, pomelo]
---

##前言##
由于目前pomelo公开资料没有什么教程类的，所以就简单的写个学习笔记，用来记录一下。

##通读api##
个人认为，竟然要使用一个框架，对于框架提供的api必须要烂熟与心，pomelo的api 还是挺少的，所以量化一下，让初学者感觉读api不是那么可怕的事情。

<!--more-->
###7个大类###

Application (31 个方法)

由于这块的方法比较多，简单分一下类



环境

`getBase()；`

`set();`

`get`

`enabled();`

`disabled();`

`enbale();`

`disable();`

`configure();`

初始化

`start();`

`registerAdmin()`

`filter();`

`before()`

`after()`

`load()`

`loadConfig();`



组件相关
`route`



获取相关配置，组件方法
`getMaster()`

`getCurServer()`

`getServerId()`

`getServerType();`

`getServers();`

`getServersFromConfig();`

`getServerTypes();`

`getServerById();`

`getServerFromConfig();`

`getServersByType();`

`isFrontend()`

`isBackend()`

`isMaster()`

`addServers();`

`removerServers();`

下面几个大类，方法比较少就不分类了。

ChannelService （5 个方法）

Channel （6 个方法） 

LocalSessionService （ 4个方法）

LocalSession （6个方法）

SessionService （4个方法） 

Pomelo（1个方法）



##Pomelo : Hello world ##

安装好pomelo 要创建一个项目很简单：

`pomelo init heloworld`

ok,我们的一个helloworld 就这样完成了。

###客户端编写###
我们来看一下，客户端如何与服务端进行通信的
打开`web-server/public/index.html` 阅读`19-32`
``` js
     var pomelo = window.pomelo;
      var host = "127.0.0.1";
      var port = "3010";
      function show() {
        pomelo.init({
          host: host,
          port: port,
          log: true
        }, function() {
        pomelo.request("connector.entryHandler.entry", "hello pomelo", function(data) {
            alert(data.msg);
          });
        });
      }
```
从request请求`connnecor.entryHandler.entry`我们在game-server目录
`app/servers/connector/handler/entryHandler.js`
找到这么一个文件，打开这个文件以后我们发现了entry这个方法，现在我们能感叹，对于pomelo的通讯居然能够做到如此简单，为了验证我们的想法。我们在这个js文件里面加入
``` js
Handler.prototype.helloworld = function(msg, session, next){
    console.log(msg);
    console.log(session);
    next(null,  {code: 200, msg: 'Hello world!'});
}
```

接着客户端修改：
``` js
  pomelo.request("connector.entryHandler.helloworld", "hello pomelo", function(data) {
            alert(data.msg);
          });
        });
```

运行项目，然后就能看到我们成功完成服务端与客户端的通讯了。

今天，我们简单的搞明白了pomelo 如何创建项目，然后，客户端如何发起请求，已经服务端如何编写能够接受客户端请求的方法。

