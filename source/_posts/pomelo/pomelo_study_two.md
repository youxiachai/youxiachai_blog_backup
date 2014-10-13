title: Pomelo 一周之旅--星期二
date: 2013-06-04
tags: [node, pomelo]
---

##前言##
昨天，简要的介绍了客户端如何发起对Pomelo的请求和处理pomelo响应，今天，我们说一下，Pomelo服务端如何处理请求响应以及如何开始我们服务端代码的编写。
<!--more-->

##Pomelo 请求与响应##

Pomelo请求响应流程图。

![](/images/pomelo/two/request_response.png)

在pomelo 请求响应模型中，它只有三层。

1. 发起请求与响应的客户端。
2. 接受，响应请求的Frontend。
3. 处理Frontend 请求与响应的Backend。

在昨天，我们已经知道了如何利用frontend进行与客户端的通信，那么什么是Backend?

我先来看一下官方的定义：

**Frontend（connector）**

* 用于面向客户端的连接
* 维护Session信息
* 分发请求给Backend
* 推送消息给客户端

**Backend**

* 处理来自于Frontend的请求
* 通过Channel 或者 Response 推送请求给frontend
* Rpc 服务

从以上定义中，我们可以这么认为，Frontend 是用于面向客户端请求的服务器，用于维护当前的连接数以及让客户端能够访问Backend 的桥梁，而Backend 用于处理游戏的逻辑。

为了方便了解，我们可以阅读官方提供的框架图。

![](/images/pomelo/two/framework.png)

###创建Frontend和Backend###
Pomelo 有一个特点就是通过对目录和命名约定进行来进行对组件的创建。现在我们去到`./game-server/app/servers/` 我们所有关于frontend和backend的代码都有在该目录下创建。

**创建规则**
###Frontend的创建###
``` bash
servers
├─connector
│  └─handler
│          entryHandler.js
```
第一级目录为servers Type，就是定义这个在服务端处于什么类型，例如，我们要创建一个`gate` 类型的服务用于负载均衡的话，我们只需要创建一个把`connector` 改为`gate`。

第二级目录约定为handler,所有处理请求的js文件都放在该目录下面。

`entryHandler.js` 这个文件为我们处理请求和响应的文件。

以上规则就是我们在Pomelo 里面要响应对客户端请求的步骤。然后，我们只需要在配置文件上面做一些修改，Pomelo在启动的时候就会加载我们创建好的frontend了。（详情阅读**声明使用Frontend和Backend**）

对于客户端而言只要在请求的时候对应以上规则即可`阅读hello world项目 /public/index.html 大概19 -36行代码`

``` js
   pomelo.request("connector.entryHandler.entry", "hello pomelo", function(data) {
                alert(data.msg);
          });
```

假如我们常见了一个gate的 frontend 用于负载均衡我们在客户端只需要做如下修改就能进行对gate的访问了。

``` js
   pomelo.request("gate.entryHandler.entry", "hello pomelo", function(data) {
                alert(data.msg);
          });
```

###Backend的创建###
``` bash
servers
└─testBackend
    ├─handler
    │      entryHandler.js
    │
    └─remote
            testBackendremote.js
```
规则与frontend基本一致，不过多了一个remote目录，用于RPC 的处理。然后，我们只需要在配置文件上面做相关配置，pomelo在启动的时候就会加载我们创建好的Backend。（详情阅读**声明使用frontend，backend**）

接着客户端如果要访问Backend 需要先连接接Frontend 再从回调中发起对Backend的访问。
>  client -> frontend -> backend 这个过程不可越过

`hello world项目 /public/index.html 大概19 -36行代码`增加。
``` js
     pomelo.request("connector.entryHandler.entry", "hello frontend", 	function(data) {
         pomelo.request("testBackend.entryHandler.entry", "hello backend", function(data) {
                    alert(data.msg);
                });
            });
```

做了以上的修改以后，客户端就能对服务端的Backend发起请求并且响应。

##配置Frontend和Backend##
上面我们知道了如何在服务端创建Frontend，Backend规则，那么Pomelo如何加载我们的定义好的的frontend,和backend 呢？

要让pomelo 加载我们创建好的frontend和backend 需要在 `./game-server/config/servers.json` 作以下修改。**因为development与production的设置是一样的这里就以development为例**

``` js
{
    "development": {
        "connector": [
            {
                "id": "connector-server-1",
                "host": "127.0.0.1",
                "port": 3150,
                "clientPort": 3010,
                "frontend": true
            }
        ],
        "testBackend": [
            {
                "id": "test-server-1",
                "host": "127.0.0.1",
                "port": 3151
            }
        ]
    }
}
```

###Frontend配置说明###

对于frontend 对象而言
``` js
"connector": [
            {
                "id": "connector-server-1",
                "host": "127.0.0.1",
                "port": 3150,
                "clientPort": 3010,
                "frontend": true
            }
        ]
```
* id 就是该frontend的在服务器的名字
* host 服务器的地址
* port 服务器的端口号
* clientPort 客户端用于连接的端口号
* frontend 是否是一个frontend

###Backend配置说明###
backend对象基本与frontend一致，只是少了面向客户端端口的声明
``` js
"testBackend": [
            {
                "id": "test-server-1",
                "host": "127.0.0.1",
                "port": 3151
            }
        ]
```
* id 就是该frontend的在服务器的名字
* host 服务器的地址
* port 服务器的端口号

###添加服务器###
对于Pomelo而言，添加服务器只需要在配置文件中作如下修改，这样我们就添加多了一台connector的服务器了，如果要添加一台backend服务器，也是一样的道理。
``` js
"connector": [
            {
                "id": "connector-server-1",
                "host": "127.0.0.1",
                "port": 3150,
                "clientPort": 3010,
                "frontend": true
            }， 
			{
                "id": "connector-server-2",
                "host": "127.0.0.1",
                "port": 3151,
                "clientPort": 3011,
                "frontend": true
            }
        ]
```


##总结##
今天，对于Pomelo的理解：

**服务端**

* 如何进行请求响应。
* 如何创建我们一个frontend或者backend。
* 如何通过配置文件让pomelo加载我们的frontend和backend。

**客户端**

* 客户端如何发起对backend请求。


##资料来源##
[pomelo 官方wiki](https://github.com/NetEase/pomelo/wiki/)