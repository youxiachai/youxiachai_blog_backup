title: 用nodejs 改造一个移动版本的网站
date: 2013-05-05
tags: node
---

##前言##
在浏览移动版本的oschina的时候,发现,怎么要找不到我最喜欢的翻译频道,正好我作为一个打杂的会一点node, 正愁着拿着node 不知道干什么东西好,就试着用node 做一个壳的移动版本翻译频道,如果你只对代码有兴趣的话,可以直接去 下载下来运行看看效果[https://github.com/youxiachai/nodeScrapeOscTranslationChannel](https://github.com/youxiachai/nodeScrapeOscTranslationChannel)
<!-- more -->

##准备##
其实,所谓的套壳,就是我们俗称的采集类网站,把别人网站的数据,变成自己的网站,虽然,不是上得了台面的东西,不过,如果不是用现成的采集工具,而是自己动手来干的话,你会对dom树的操作,网页的处理有更好的理解.基于某种考虑,特别写上.

###运用的技术与库###
1. [nodejs](http://nodejs.org/)
2. [jsdom](https://github.com/tmpvar/jsdom)
3. [hashmap](https://github.com/flesler/hashmap)
4. [express](https://github.com/visionmedia/express)
5. [mkdirp](https://github.com/substack/node-mkdirp)
6. [downloader](https://github.com/lockerfish/Downloader)

###分析###
我们要从外部改造一个网站,首先需要熟悉我们要改造对象的网站结构,将oschina 翻译频道进行草稿化,如下图
![](/images/nodescrapingpage/oschina2.jpg)

经过我简单分析以后然后转换为移动版本的话
![](/images/nodescrapingpage/oschina3.jpg)

在我的设计中只保留了分类,和列表,而在接下来的代码实现中,我只实现了列表的部分...


##译文列表部分##
[翻译频道译文列表的解析转换代码](https://gist.github.com/youxiachai/5521332) 请移步到gist 查看..为了方便阅读,修改了一下跟最后源码的实现会有点不同.

幸好翻译频道的结构挺简单的,由于刚上手js不久,这个第一版的dom解析代码还可以进行简化,虽然,现在这个版本挺难看的但是,可以跑起来.

这段代码的主要干了以下事情:

1. 迭代每个div.article 结点获取列表的信息,并且用`<li />`标签进行包装
2. 把链接转换为相对链接.

最终的效果:左边为原页面,右边为移动版本

![原来的页面](/images/nodescrapingpage/oschina5.png)
![改造好的页面](/images/nodescrapingpage/oschina4.png)

好了,这就完成了web -> mobile 页面的转换,接下来我们转化一下内容页.

##译文部分

草稿部分忽略,拍照什么的挺麻烦的..
[翻译频道译文内容的解析转换代码](https://gist.github.com/youxiachai/5521332) 请移步到gist 查看..为了方便阅读,修改了一下跟最后源码的实现会有点不同.

这部分就比较简单了,dom的操作

1. 获取译文内容
2. 移除了译者信息..

最终的效果:左边为原页面,右边为移动版本

![原来的页面](/images/nodescrapingpage/oschina6.png)
![改造好的页面](/images/nodescrapingpage/oschina7.png)

内容方面我们就搞定了.下面的部分就是如何部署一个套壳的网站

##建立属于自己的移动网站##
前提: 对express 有一定了解

要web 化很简单,只需要把刚才的解析代码放到路由里面即可,详细实现看源码..

`app.get('/', callback);`

`app.get('/translate/:title', callback);`

最终演示用地址挂在我自己的服务器上:[演示网址:http://goo.gl/K3Dc8](http://goo.gl/K3Dc8) **用了google的短网址服务可能有转换慢,或者无法访问的情况**


最近,kindle入华貌似变成了事实,特此贴上kindle浏览的效果..图片压缩了一下,可能效果差了不少,不过对于kindle3 而言中文字体的确很难看,有kpw可否贴下?

![原来的页面](/images/nodescrapingpage/osckindle1.gif)
![改造好的页面](/images/nodescrapingpage/osckindle2.gif)



##展望##
由于整个程序虽然代码不多,不过需要的知识的广度不少,例如,dom树,jsdom ,express, html5, 每个知识都只是用了那么一点...写起来真不好下手,有兴趣的朋友,可以fork 我github的项目,地址,开头就给了.

当然,这个程序是一个半成品(一个晚上的代码,再花了一个晚上写这篇博文),很多东西都还没加上...接下来,我应该会着手实现webapp离线化....