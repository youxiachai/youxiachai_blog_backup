---
title: 2014 Hangjs 见闻流水账第二天
tags: node
categories:
  - nodejs
date: 2014-06-29 00:00:00
---

## 前言

第一天传送门: [2014 Hangjs 见闻流水账第一天 ](http://cnodejs.org/topic/53aa6d6dc3ee0b58206c7b3f)

写作风格跟第一天还是一样的.


<!--more-->

## Slide

每个slide我都会根据自己的理解重新命名一次,用于表达自己的第一看法,主观意见,不喜可吐槽,但是不要喷,就算要喷请轻碰...

### angular 大法好

今天第一场slide是由[Sofish](http://weibo.com/isofish)带来的关于如何[优化你的Angular Web App](http://sofi.sh/2412).

作为一名angular用户,这次slide分享的切换路由状态的监听事件,是个不错的收获,之前的loading状态都是到处定义开始-结束和标识,或者自定义一个服务来进行全局控制.有内置的话就不需要每次碰到都自己写了.

不过,对于angular的代码组织的说明,还是通用方案(例如使用[angular-seed](https://github.com/angular/angular-seed/)),这个通用方案的最大的弊病就在于,随着项目的业务越来越复杂,单文件的代码量会越来越大,后来Q&A也有人提了我同样的问题,不过,Sofish 简单回答带过.

对于没有去hangjs的同学,这个slide关于angular的优化很值得一看...

PS: 这个PPT在现场看的时候,异常辛苦(字完全看不清),吴老师吐槽这个slide的文字背景对比度太没考虑现场的投影设备环境了...

#### 视频传送门

|在线播放|视频下载|
|:--|:--|
|[土豆](http://www.tudou.com/programs/view/oMuGAFbPnf8/)|[又拍云](http://hangjs.b0.upaiyun.com/videos/09.Sofish-%E4%BC%98%E5%8C%96%E4%BD%A0%E7%9A%84Angular%20Web%20App.mov)|

### 如何成为一名优秀的githuber

这次slide由[郭宇](http://weibo.com/137601206),分享的[开源项目的管理与维护](https://github.com/jsconfcn/hangjs/blob/gh-pages/slides/GuoYu-%E5%BC%80%E6%BA%90%E9%A1%B9%E7%9B%AE%E7%9A%84%E7%AE%A1%E7%90%86%E4%B8%8E%E7%BB%B4%E6%8A%A4-%E6%88%91%E7%9A%84%E5%BC%80%E6%BA%90%E4%B8%80%E5%B9%B4.md)

作为一名github的活跃用户,这场由github 超活跃的用户带来的演讲,有不少启发的地方.

#### 开源项目的兴奋点

[郭宇](http://weibo.com/137601206) star 数最大的项目就是那个命令行的[豆瓣电台](https://github.com/turingou/douban.fm) 现在是978, 其实,我是万万没想到(这个douban.fm,我是从刚开始看到现在,我还贡献了几个PR),原来大家对于能够用命令行播放在线音乐的兴趣是这么大的,不知道是真解决了自己的需求,还只是为了炫酷?

我认识的一部分github朋友,为什么开坑,不填坑.很多原因都是因为,没人关注啊.写了也没什么反响,没有了自我满足那块(不少开坑的作者,就是冲着自我满足),也就没有动力去填坑了.所以,如果是要打算开一个能够自我满足的坑,在开坑的时候,最后,看一下当前的IT热点,例如,ios 的swift.这样的开坑,获得各种眼球的机会大大挣大.可能有人会说,这样是不是不负责任的行为?

我个人看法,作为一个凡人,获得一些小小的自我满足,其实也没什么,如果你能够针对问题提issue,而不是看地图炮就好,其实,不少开坑的作者,还是很喜欢有人针对他的坑,提一些意见.不过,开坑的话,有时候难免会碰到几个莫民奇妙的喷子,对于这些喷子,一开始倒是挺生气的,不过,后边会发现,随着关注度提高到一定程度,喷子必有,也没必要为了纠结这几个喷子,而影响了正常的心情.

#### 开源项目的自我满足

如何开坑(建立开源项目),其实是个很大的问题,我认识的不少拥有github账号的朋友,很多就是一些僵尸,甚至几年来,github活动主页全部是0.个人觉得,这种现象的产生,是因为,他们很多时候都没法从写代码中获得到足够的满足,连自己的都没法满足,又如何能够满足他人呢?又如何能够把开坑当成一种有趣的事情来做呢?个人看法,切勿地图炮....

在自我满足达到一定程度,就会自然而然的负起责任来.

#### 视频传送门

|在线播放|视频下载|
|:--|:--|
|[土豆](http://www.tudou.com/programs/view/ZSV7-gydgsQ/)|[又拍云](http://hangjs.b0.upaiyun.com/videos/10.%E9%83%AD%E5%AE%87-%E5%BC%80%E6%BA%90%E9%A1%B9%E7%9B%AE%E7%9A%84%E7%AE%A1%E7%90%86%E4%B8%8E%E7%BB%B4%E6%8A%A4.mov)|


### 思考如何组织管理大型JS项目

由EF 教育的[Mikael Karon](https://github.com/mikaelkaron)带来的[Massive Javascript Development](http://2014.jsconf.cn/slides/mikaelkaron-massivejs/massive-js.html)

老实说,完全没听懂.后来还是看[民工精髓V](http://weibo.com/sharpmaster) 的微博,才知道说的是一些很高深的内容

> …这老外讲的架构方面的主题，可能因为我平时比较关注这方面，所以毫无压力听懂说的每个东西，然后还在微博上实时翻译记录了。手机打字速度太慢了，下次来听还是得带电脑。老外讲的要点就是，对于大型项目，要模块化，开发阶段小粒度，部署阶段大粒度。真精辟，我也这么想。

> [这个老外讲的切中大型工程的要点:模块化，开发阶段小粒度，部署阶段大粒度](http://weibo.com/1858846672/Ba9NkEPoO)

有兴趣的去看看

#### 视频传送门

|在线播放|视频下载|
|:--|:--|
|[土豆](http://www.tudou.com/programs/view/fqKtbaP0DIA/)|[又拍云](http://hangjs.b0.upaiyun.com/videos/11.Mikael%20Karon%20-%20Massive%20Javascript%20Development2.mov)|

### 并不诱人的轮子 -- TroopJS

还是EF 教育的人,[Garry Yao](https://github.com/garryyao),关于[Scalable Web Application with TroopJS](http://2014.jsconf.cn/slides/garryyao-troopjs/scalable-web-application-with-troopjs.html)

这个slide完全没听进去...

#### 视频传送门

|在线播放|视频下载|
|:--|:--|
|[土豆](http://www.tudou.com/programs/view/JKNKWW_yZrw/)|[又拍云](http://hangjs.b0.upaiyun.com/videos/12.Garry%20Yao%20-%20Scalable%20Web%20Application%20with%20TroopJS.mov)|

### 考虑我们是有追求的程序员

下午第一场,是人见人黑的[玉伯](http://weibo.com/lifesinger)的[如何持续技术学习](http://2014.jsconf.cn/slides/how-to-continue-to-grow-up.pdf)

如果,你还没有自己的一套自我管理观的话,玉伯这个slide应该对你能够有所启发.

我对这个slide归纳就是:控制输入,主动思考,最后自我输出.

#### 视频传送门

|在线播放|视频下载|
|:--|:--|
|[土豆](http://www.tudou.com/programs/view/VUzRIPY6Es8/)|[又拍云](http://hangjs.b0.upaiyun.com/videos/13.%E7%8E%89%E4%BC%AF%20%E5%A6%82%E4%BD%95%E6%8C%81%E7%BB%AD%E6%8A%80%E6%9C%AF%E5%AD%A6%E4%B9%A0.mov)|

### 让你的服务快快快

台湾的[Caesar Chi](https://github.com/clonn)带来的[Node.js 與多方服務串接實務](http://2014.jsconf.cn/slides/nodejs_api_connect_jsconfcn_hangjs.pdf)

看这个slide题目,你第一反应可能又是准备水一场了吧.因为,连接多个服务商,无非就是各种认证接入,不过,实际上这个slide并没有把重点放在串接上.

前面,一部分时间讲如何将对接服务进行模块化实践,后边就大谈,如何提高服务端对客户端响应.其中,的一个实践就是

> 先响应,后处理

客户端发起了一个请求,服务端立马反应给客户端,让客户端知道服务端在处理了,你去干点的吧,结果由服务端主动推送.这样做的好处就是可以让客户端不用等待.

当然,缺点也有,就是增加了对异常的处理复杂性.举个例子,你提交了一个表单,然后,服务器告诉你,正在处理,于是你去做点别的东西,或者离开,很不幸的是,你表单某个字段有问题,这是,服务器推送了一个异常给你,但是,你已经离开原来页面了,于是,你不知道你的填写是有问题的.还有一个问题就是维持长连接的资源消耗.

#### 视频传送门

|在线播放|视频下载|
|:--|:--|
|[土豆](http://www.tudou.com/programs/view/h8TuqilJq0w/)|[又拍云](http://hangjs.b0.upaiyun.com/videos/14.Caesar%20Chi%20Node.js%20%E8%88%87%E5%A4%9A%E6%96%B9%E6%9C%8D%E5%8B%99%E4%B8%B2%E6%8E%A5%E5%AF%A6%E5%8B%99.mov)|

### Node.js是如何进化为NodeOs

由NodeOs 作者[Jacob Groundwater](https://github.com/groundwater),分享的[five-lines 深入浅出 node 命令行工具](http://2014.jsconf.cn/slides/five-lines.pdf)

这个slide应该是开场几个小时前定下来的,之前日程表上面是没有的,NodeOs记得上一年,大家有过一段讨论,后来就沉了,当时,听到介绍的时候,立马去看了一下[NodeOs](https://github.com/NodeOS/NodeOS)的github,发现了个[docker image](https://github.com/NodeOS/NodeOS-Docker),看来还是有点进度的.

不过,这次slide挺有意思的,正如标题所说,深入浅出,通过对node基本的一些特性(执行shell命令),最后慢慢的做出复杂的东西.例如打造一个Node.js 风格的操作系统,让全部命令执行输出JSON化.

其实,很多语言都有执行shell命令的api,但是,为什么之前没有人想到要搞个JavaOs,PythonOs...等等(可能也有但是我不知道).这个slide带给我最大的收获,就是要勇于去想象!

就算是这个想象有点不切实际,但是如果是理论上可行的,为啥不尝试在想象的时候去实践一下?

而在Node.js 上实际上还真出现了不少想象力丰富的东西,除了前面提到的NodeOs,还有[Node上的jvm虚拟机](https://github.com/YaroslavGaponov/node-jvm),让Node.js做类似于arduino 的[tessel](https://tessel.io/),有些朋友可能会觉得这些东西的可能是个傻x想法,但是,现实是这些东西不但被人想出来,而且还都做出来了,我觉得,当人不愿意去进行想象的时候,也就停止了进步.忽然,想起了老罗的那句语录.

> 不被嘲笑的梦想，是不值得去实现的

当一个程序员失去想象力的时候,可能也是步向平庸的开始了.


#### 视频传送门

|在线播放|视频下载|
|:--|:--|
|[土豆](http://www.tudou.com/programs/view/3-3PegdbS6E/)|[又拍云](http://hangjs.b0.upaiyun.com/videos/15.Jacob%20Groundwater%20and%20dead-horse%20-%20five-lines.mov)|

### c# 和Node.js 相亲相爱

这么多slide里头,唯一一个用windows(其他都用Mac)进行分享的,微软美女程序员[Iris](http://irisclasson.com/) 带来的[Edge.js](https://github.com/tjanczuk/edge)

实际上就是一个让c#程序里头能够跑Node.js,在Node.js里头能够跑c#,个人不怎么喜欢这玩意,后来也跟老雷讨论了一下这东西,老雷也表示不怎么喜欢.我们俩不怎么喜欢的原因,可能都是因为我们都没接触过c#.

因为,不怎么感兴趣,所以没什么好说的.


#### 视频传送门

|在线播放|视频下载|
|:--|:--|
|[土豆](http://www.tudou.com/programs/view/cdedTXM5y28/)|[又拍云](http://hangjs.b0.upaiyun.com/videos/16.Iris%20-%20Edge.js.mov)|


### 手机传感器在移动web的实践

天猫前端[鬼道-徐凯](http://weibo.com/777865156)带来的[Hybrid API](http://2014.jsconf.cn/slides/luics-hybrid-api.html)这是一个你只看题目,怎么也不会猜到要准备说的什么内容的分享(当时看日程表的时候,Hybrid Api? 这是啥东西啊...).

这个slide分享的是,手机传感器在移动web的使用实践.你可能会惊讶,现在的手机浏览器能够支持传感器了?目前来看,手机浏览器还没支持w3c关于传感器的实现,但是,不妨碍我们的想象力啊,于是,我们可以通过改造手机的web控件,从而让移动web支持手机传感器,然后,利用这个特性,来开发一些特别有趣的东西.

后边,鬼道就分享天猫app是如何利用这个特性,进行真实项目实践.总得来说,这是一个很具有想象力的分享!

#### 视频传送门

|在线播放|视频下载|
|:--|:--|
|[土豆](http://www.tudou.com/programs/view/1-Ltio6NGaU/)|[又拍云](http://hangjs.b0.upaiyun.com/videos/17.%E9%AC%BC%E9%81%93%20Hybrid%20API.mov)|


### 用Node.js玩转Storm

Luying Li(没找到相关的个人主页)带来的[Storm - Distributed and fault-tolerant realtime computation](http://2014.jsconf.cn/slides/Introduction-To-Storm.pdf)分享.

其实看日程表的时候,看到Storm混进来,顿时在想,这玩意不是用Java写的吗,跟JS有什么关系?会不会跟[Node.js 與多方服務串接實務](http://2014.jsconf.cn/slides/nodejs_api_connect_jsconfcn_hangjs.pdf)带来一些意外的惊喜?不过这次,真没什么意外惊喜了.

大部分时间都是介绍Storm是个什么,举的例子也全部都是Java写的.最后,补充了一下,Storm的实现支持任何语言,Node.js当然不在话下,于是最后贴了两个Node.js实现.那么举例子的时候..为啥不用JS写...感觉好没说服力..这个场子毕竟是JS....你确不用js来举例子.


### 来日方长的前端技术革命

最后一场slide由来自于云适配的[陈本峰](http://weibo.com/chenbenfeng)分享的[Web Components标准：前端开发的新一次技术革命](http://2014.jsconf.cn/slides/WebComponents.JSConf.pdf)

由于,这个话题,我不怎么感兴趣,然后这场slide的ppt还没放上来,所以,我都忘得差不多这场slide究竟说了些什么..只是印象中知道,这个是几年后才有可能实现的标准.

#### 视频传送门

|在线播放|视频下载|
|:--|:--|
|[土豆](http://www.tudou.com/programs/view/pKlqYK8s6bY/)|[又拍云](http://hangjs.b0.upaiyun.com/videos/19.%E9%99%88%E6%9C%AC%E5%B3%B0%20-%20Web%20Components%E6%A0%87%E5%87%86%EF%BC%9A%E5%89%8D%E7%AB%AF%E5%BC%80%E5%8F%91%E7%9A%84%E6%96%B0%E4%B8%80%E6%AC%A1%E6%8A%80%E6%9C%AF%E9%9D%A9%E5%91%BD.mov)|


## 总结

这次两天的Hangjs, 面基了不少一直在网上有交流但是,从来没见过面的朋友,特别是有机会一起同行的[老雷](http://weibo.com/ucdok),在我看来,参加这类技术大会,还是以认识和交流,各种平时耳闻但不见面的人为主最好,听slide对自己有用的就认真听听,这次hangjs我最大的软肋就是不够主动,没有随便逮住一个人就说,你好,能够简单认识一下吗?这是我的github,来扩大一下自己的人脉起码也可以为自己的[github](https://github.com/youxiachai)涨涨粉...).





## 附录

其他人的hangjs 参会记录

[梁杰_numbbbbb 技术大会到底该怎么听？](http://weibo.com/p/1001603724334156923311)

[民工精髓V 杭JS参会记录](http://weibo.com/p/1001603724961759021973)

[lisposter Hang-JS-2014](http://zhuli.me/hang-js-2014/)

[鹄思乱想 2014 杭JS 杂谈](http://www.thinkingincrowd.me/2014/06/25/hangjs-2014/)

[fsiaonma 继《京JS》后，再遇《杭JS》](https://github.com/fsiaonma/fsiaonma.blog.com/issues/5)