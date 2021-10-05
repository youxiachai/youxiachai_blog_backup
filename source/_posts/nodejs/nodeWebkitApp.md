---
title: 用node-webkit开发PC 客户端
tags: node
categories:
  - nodejs
date: 2013-07-03 00:00:00
---

##导言##
node-webkit 是一个很神奇的桌面客户端项目,正如这个项目的名字,这个项目是由node 和 webkit 构成,简单来说,就是你可以用HTML 5和 node 进行桌面客户端开发,而且客户端还是同时支持在 WIN,MAC,LINUX运行. 下面,就用一个简单的例子来展示一下node-webkit的魅力.
<!--more-->
##创建项目##

###本例子基于Grunt构建###
如果,你用过grunt,要创建一个node-webkit 非常简单,我已经写好了一个node-webkit的`grunt-init`的项目模板. 至于怎么安装这个模板,跟官方的教程一样.
如果你是windows 用户
> `md %USERPROFILE%\.grunt-init\node-webkit` <br />
`git clone git@github.com:youxiachai/grunt-init-node-webkit.git %USERPROFILE%\.grunt-init\node-webkit`

linux or mac
> git clone git@github.com:youxiachai/grunt-init-node-webkit.git ~/.grunt-init/node-webkit

你只需要用
>`grunt-init node-webit`

就可以创建完毕.

``` shell
├─app.nw
└─test
```

app.nw 这个目录就是我们准备要开始写的pc 客户端的项目文件夹,运行node-webkit项目很简单,只需要把node-webkit 的运行环境配置到环境变量,然后运行
> `nw app.nw` 就可以运行起来了.
![oscdesk1](/images/nodewebkit/nodewebkit1.jpg)

PS: 如果你不想接触grunt,不过我建议还是学一下grunt,你可以到[https://github.com/rogerwang/node-webkit#quick-start](https://github.com/rogerwang/node-webkit#quick-start) 学习如何启动一个node-webkit应用.



##效果图##
![oscdesk1](/images/nodewebkit/oscdesk1.jpg)

![oscdesk1](/images/nodewebkit/oscdesk2.jpg)

这个就是所谓的 win 8 风格的客户端了吧....界面用的框架是这货:[http://aozora.github.io/bootmetro/](http://aozora.github.io/bootmetro/) 90% 的时间都是调整界面...蛋疼死了......连个 win8 风格的progress 都没..让我非常伤心..也可能是alpha 的原因吧. 不过最后的效果,还是很难看,就凑合着过去吧....

###开发###
基于node-webkit 开发pc 客户端语言支持 `c/c++`,`html5`,`css3`, `js`,`node api`.好了,现在我们开始用html 5 + css3 写一个pc 客户端吧.
`node-webkit`本质就是一个可以跑node 的浏览器,所以,我们用web 开发的技巧来开发pc 客户端毫无问题.

首先,打开`toolbar`,在`package.json`文件里面有个`toolbar`的参数,设置为`true`即可,就会见到如下图所示:
> ![toolbar](/images/nodewebkit/toolbar.jpg)

点击那个三横线的按钮,一个chrome 风的调试窗口就出来了.
> ![console](/images/nodewebkit/console.jpg)

开发的时候,我们修改完文件,并不需要重新运行程序来看结果,我们,可以直接点击左边的刷新按钮即可看到我们修改的运行结果.用`node-webkit`开发客户端是不是很方便了!

那么接下来,要开发一个oschina pc 客户端,我们只需要知道,相关api 就行了,从android 客户端源码里面可以得到相关api...具体代码在`app/model/oschinaApi.js` 文件里面.

node-webkit,已经吧相关的安全限制已经去掉,所以说,用node-webkit开发pc客户端,用webkit 发的请求不受同源限制. 用node-webkit 开发一些restful 应用是非常舒服的事情,只要有个不错的界面.关于`node-webkit`的东西也就这么些了,剩下的就是web 开发,不在本文`node-webkit`范围内,所以就不再啰嗦..

##使用的开源项目##
界面: 

[http://aozora.github.io/bootmetro/](http://aozora.github.io/bootmetro/) 

[https://github.com/cubiq/iscroll](https://github.com/cubiq/iscroll)

模板引擎: 

[https://github.com/visionmedia/ejs](https://github.com/visionmedia/ejs)


##项目地址##
Github: 
> [https://github.com/youxiachai/osChinaDesktopClient](https://github.com/youxiachai/osChinaDesktopClient)

git@osc:
> [http://git.oschina.net/youxiachai/oschinadesktopclient](http://git.oschina.net/youxiachai/oschinadesktopclient)

 程序运行: windows用户之间去到`app.nw` 目录下运行 nw.exe 即可.
> cd app.nw <br/> nw.exe

linux 或者mac 用户 把除 index.html,package.json,app 以外的文件删除,然后将`node-webkit` 运行环境配到环境变量中运行
> nw app.nw