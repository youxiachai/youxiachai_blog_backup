title: Hello Kraken.js!
date: 2013-11-30
tags: node
---

## 前言

kraken.js 由paypal 公司开源的一个用于快速开发基于Express.js框架应用的快速开发工具, 因为kraken 并没有在Express.js基础上更改多少东西,只是在原来的express基础上补充了一些约定开发的规则, 让开发根据便捷.

<!--more-->

## 你好,世界!

要创建一个kraken 项目只需要非常简单的三步走:

1. 安装必备工具
>   Linux or Mac <br/>
> `sudo npm install -g yo generator-kraken` <br/>
> Windows <br/>
>` npm install -g yo generator-kraken`

2. 创建kraken项目 <br/>
只需要一行代码,然后,看着提示语,输入一些东西,一个项目就这样创建完毕.
> `yo kraken`

3. 运行kraken项目 <br />
还是只需要一行代码
> `npm start`

以上三行代码即可,完成一个kraken项目的创建了.

### 注意事项:

1. `yo kraken` 你输入项目名字的时候,会在当前项目创建与该项目名字一样的文件夹,记得`cd` 进去文件夹再去运行 `npm start`

2. 注意`NODE_ENV`的设置,kraken的配置是会根据当前`NODE_ENV`进行变化,所以如果跑不通的时候最好检查一下当前的`NODE_ENV`.默认情况下,`NODE_ENV`没有设置或者设置了`development`,启动的时候,`kraken` 会默认加载`./public/templates` 下的模板,设置了其他值的时候,就会去加载`./.build/templates` 而这个当你在`kraken` 项目目录下运行`grunt build` 就会出现`.build` 该目录了用于部署在`NODE_ENV`设置为`production`或者其他值的时候加载.

查看你当前系统的`NODE_ENV`环境

> Linux or Mac
>> `echo $NODE_ENV`

> Windows
>> `echo %NODE_ENV%`

## 约定开发

个人看法 `kraken` 与其说是一个框架好不如说它只不过提供了灵活,方便的用于构建Express应用的方式.


### 配置(/config)

`kraken` 在 `./config` 约定了两类配置文件:

1. `app.json` 用于配置,host, port, i18n ,express 等.
2. `middleware` 用于对默认中间件的配置,目前支持的中间件有`appsec`, `compiler`, `session`, `errorPages`, `static`, 详细的参数配置请阅读官方文档,这里就不赘述了.

除此以外,`kraken` 还约定支持根据`NODE_ENV`自动匹配相应的配置文件,规则是:

>`app-NODE_ENV.json`

例如, 当前`NODE_ENV`是`development`, 你在目录下有一个`app-development.json`的文件话,就会优先读取该文件的配置.

### 控制器(/controllers)

### 路由控制

`kraken` 默认会自动加载`./controllers` 下的文件,进行路由控制,个人觉得,这个真心省心.接下来的写法,与express 完全一样!

```js
module.exports = function (server) {
    server.get('/customer', function (req, res) {
       res.send('Hello World');
    });
};
```

如果,你是express的开发者,就会越感发现,`kraken`更多的是补充了express不足的地方,除此以外跟平常用express开发毫无区别.

### 自定义中间件 和 周期控制

`kraken` 将一个请求的周期定义为三级:


1. `app.requestStar` 请求开始
2. `app.requestBeforeRoute` 进行路由前
3. `app.requestAfterRoute` 路由后输出

整个实现异常简洁, 有兴趣的可以去阅读源码:

> `appcore.js` `line: 172 - 198`

定义一个中间件的方法与express的时候一样,只不过,现在多了一步,我们要把中间件放到哪个位置而已

约定在`./middleware/lib` 下建一个文件,beforeRouteMiddleware.js
```js
module.exports = function () {_
    var reqCounts = 0;
    return function (req, res, next) {
        req.counts = reqCounts | 0;
        reqCounts++;
        next();
    };
};
```

然后在: index.js

```js
var beforeRouteMiddleware =  require('./lib/middleware/beforeRouteMiddleware')

app.requestBeforeRoute = function requestBeforeRoute(server) {
    server.use(beforeRouteMiddleware());
};
```

与express的中间件功能一样,每请求一次,都会触发中间件. 与原生的express不同`krarken`提供了更有条例的中间件定义约定.

## 总结

`kraken` 还有一些比较实用的功能,例如安全,还有本地化,不过这些内容,官方文档已经写得很详细了,这里就不作过多的说明. 在我看来,`kraken` 只干了一件事,就是把express条理化了,`kraken`并没有对express进行更多的封装,所以,只要会express的入手`kraken` 就像喝水一样简单. 而`kraken` 提供的约定,能够更有效率的开发Node.js的web项目.
