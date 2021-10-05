---
title: 2014 Hangjs 见闻流水账第一天
tags: node
categories:
  - nodejs
date: 2014-06-24 00:00:00
---

## 前言

6月21日~6月22日, 第一次跑远门去参加一个大会(广州 -> 杭州),本来打算,在火车的回来的路上,把这两天的东西记录一下,不过,火车上的环境实在恶劣,同时也高估了自己的专注力,所以,最后还是决定回来再写吧,还可以先看看,别人是怎么写的.在动笔之前,看了一下别人写的,所以,直接略过会议的一些流程,对这个会议的流程有兴趣的可以去看附录的传送门,我觉得他们已经把我本来想写的东西都写了,然后,就直接针对,每个slide说说自己的看法,正如标题所说,就是个流水账...

<!--more-->

## Slide

每个slide我都会根据自己的理解重新命名一次,用于表达自己的第一看法,主观意见,不喜可吐槽,但是不要喷,就算要喷请轻碰...

### 如何造一个好用的"轮子"

这次hangjs的第一场是由[严清老师](http://weibo.com/zensh)带来的关于[thenjs异步编程的实现原理和优缺点](http://2014.jsconf.cn/slides/%E4%B8%A5%E6%B8%85-JavaScript%E5%BC%82%E6%AD%A5%E5%BA%93%E5%8E%9F%E7%90%86%E5%8F%8A%E5%AF%B9%E6%AF%94.key.zip)分享.

其实关于JS的异步编程可以算得上是烂大街的主题了,严清老师觉得各家的的异步实现的轮子不好,于是[thenjs](https://github.com/teambition/then.js)就诞生.不过,严清老师能实现一个不错的库,但是,在我看来在一个只有40多分钟的slide里大谈具体代码的实现原理,并不合适,花大量时间谈具体的代码实现,在一个只有40多分钟的slide里头是很枯燥的事情(而且,我也不觉得这玩意能够在40分钟里头说清),毕竟这个东西是开源的,粗略的说说实现就好,你对实现感兴趣,去看源码吧!接下来用实际案例说说这个轮子的实现跟其他家轮子的实现有什么不同的地方,这样的不同,带来了什么好的地方.这样,我就对thenjs的异步实现非常感兴趣,从而对异步实现原理有更高层次的理解.毕竟这是一个时间很短的slide,希望能够多说一些能够启发性的东西,而不是具体怎么写.

如同前面说的js异步编程其实是烂大街的话题,我感觉大家更多想看到各种案例实践,而且我也一开始是以为标题的优缺点会是严清老师,会大说特说在实际开发中,用现用的异步库如何如何被坑,然后决心自己造轮子,用各种血的现实告诉大家,thenjs是如何好用...这样的异步实现是多么的优秀...结果到优缺点的比较主要还是各种基于代码的实现比较....没有任何炫酷的实际案例比较...

### 不明觉厉的Node.js 内核嵌入开发

这个slide 是最近很火的atom编辑器主要作者之一的[赵成](https://github.com/zcbenz) 的分享,原标题是 [Atom编辑器嵌入Node.js引擎实践](http://2014.jsconf.cn/slides/Practice%20on%20embedding%20Node.js%20into%20Atom%20Editor.pdf),当时,看到这个标题,我就觉得,我听这个slide就是打打酱油了,因为不可能听懂,作为一个凡人码农,从来都没看过Node.js是怎么实现的,然后,这个slide一上来就跟我们说,如何改造Node.js的引擎,实在是太高端了...实在玩不过来,不过我相信,会场几百号人肯定有人能听懂的,只是我比较low而已...

不过,一个面向JS程序员的大会,讲如何让Chromium能够运行v8,我不敢表示大多数,但是有一点可以肯定是,很多Node.js程序员并不会编译Chromium,那么更不用说,知道原理后,要动手操作Node.js跟Chromium合体了.作为对atom-shell最了解的人说,面对的大部分JS程序员,我觉得这个slide还不如多多分享一下怎么用atom-shell 快速开发一些跨平台app技巧,然后用一些具体案例对比一下[node-webkit](https://github.com/rogerwang/node-webkit/)这两者的优缺点(关于这个优缺点在这次slide上有提到过,不过并没细讲...).

PS:这次的演讲者作为github员工,说了一些关于github的内部事情,atom本来是打算不开源的,但是,不开源的阵营的主要领导者,因为某次丑闻导致离职,于是atom就开源了...

#### 视频传送门

|在线播放|视频下载|
|:--|:--|
|[土豆](http://www.tudou.com/programs/view/JU112RMWPIQ/)|[又拍云](http://hangjs.b0.upaiyun.com/videos/03.%E8%B5%B5%E6%88%90%EF%BC%9AAtom%E7%BC%96%E8%BE%91%E5%99%A8%E5%B5%8C%E5%85%A5Node.js%E5%BC%95%E6%93%8E%E5%AE%9E%E8%B7%B5.mov)|

### Node.js 的胶水时代

这次的演讲者[赫门](https://github.com/threeday0905)带来的是前几个月就开始有讨论的[前后端分离实践](http://ued.taobao.org/blog/2014/04/full-stack-development-with-nodejs/).

这次的slide,通过两次大战来递进的说目前前后端实践的情况,这个我听着挺带感的,因为,我正好就是[第一次前后端分离大战](http://2014.jsconf.cn/slides/herman-taobaoweb/index.html#/48)的实践者,然后[碰到的问题](http://2014.jsconf.cn/slides/herman-taobaoweb/index.html#/51)也跟slide说的一样.

不过,[第二次前后端分离大战](http://2014.jsconf.cn/slides/herman-taobaoweb/index.html#/57),虽然标题的是第二次,但是,我觉得本质只是第一次前后端分离大战的一个特殊存在,针对的是你的后端业务不是用Node.js来写,于是通过Node.js对后端业务进行一个合并优化,让客户端干少一点事情,提升客户端的响应速度.所以,Node.js就像胶水一样把两个分离的东西粘在了一起.毕竟很多系统都用了十几年了,不太可能再用Nodejs重写一遍,不过利用Node.js作为胶水的功能,针对移动应用的场景对现有系统进行优化,是个不错的实践思路.

#### 视频传送门

|在线播放|视频下载|
|:--|:--|
|[土豆](http://www.tudou.com/programs/view/_PON-LV6X2E/))|[又拍云](http://hangjs.b0.upaiyun.com/videos/04.%E8%B5%AB%E9%97%A8-%E6%B7%98%E5%AE%9D%E5%89%8D%E5%90%8E%E7%AB%AF%E5%88%86%E7%A6%BB%E5%AE%9E%E8%B7%B5.mov)|


### 不存在的服务,存在的使用

下午第一场slide是由台湾开发者[蘇 培欣](https://github.com/peihsinsu)带来的是,[Google BigQuery API Node.js实践](http://2014.jsconf.cn/slides/JSConf%20-%20Google%20BigQuery%20API%20Node.js%E5%AF%A6%E4%BD%9C%E8%A8%98%E9%8C%84.pdf) 看到这个标题,应该明白我为什么对这个slide起了这么一个名字了吧?

这个slide其实也没什么好说的,就是讲如何使用[bigquery](https://github.com/peihsinsu/bigquery) 这个用Node.js写的模组(模块),进行google big query的查询.

不过,通过这个slide知道了Google bigquery 这个服务,倒是一个不错的收获,对于有大数据需求,但是没有空折腾各种优化的精力,用这个服务倒是省心不少,对了,要速度快,访问google的黑科技是必须要掌握的!

#### 视频传送门

|在线播放|视频下载|
|:--|:--|
|[土豆](http://www.tudou.com/programs/view/205h9BvigGs/)|[又拍云](http://hangjs.b0.upaiyun.com/videos/05.%E8%8B%8F%E5%9F%B9%E6%AC%A3-Google%20BigQuery%20API%20Node.js%20%E5%AE%9E%E8%B7%B5.mov)|


### 用管道串流你的思想

这次slide是由James Halliday来演讲,github是[substack](https://github.com/substack)平时,在使用库的时候有关注作者的习惯的话,对于这个github名字应该很熟悉了吧!

外国人的演讲,你就不要指望外国人能够跟你用中文演讲了.第一次在现场听外国人的演讲,果然是近乎完全听不懂orz.关于[@substack](https://twitter.com/substack)的很多趣闻,已经有很多人说过了,这里就不跑题了,还是说说这次slide的收获吧.

虽然,听不懂说什么,但是不妨碍我看得懂PPT说什么..

在出发去hangjs的时候正好在使用gulp的时候看了一本substack写的Node.js流编程实践[stream-handbook](https://github.com/substack/stream-handbook),然后这次slide,substack 现场编程show了一把这个思想的具体应用,直到现在我还在消化着,等消化得差不多,在额外写一篇分享吧.

最后介绍了一个挺好玩的东西[voxeljs](http://voxeljs.com/) 基本上就是一个类似于Minecraft 的H5 游戏,不过,可以在上面编程让游戏变得很好玩.

#### 视频传送门

|在线播放|视频下载|
|:--|:--|
|[土豆](http://www.tudou.com/programs/view/svwrLe0ZTWs/)|[又拍云](http://hangjs.b0.upaiyun.com/videos/06.James%20Halliday-Peer-Directed%20Collaborative%20Projects.mov)|

### 曲高和寡

这场slide由来自于中科院的[@belleveinvis](http://weibo.com/belleveinvis)分享如何写一个代码生成器的原理,标题: Patrisika - Theoretical and Practical Code Generation.在听这个的slide的过程中,我和[吴老师](http://weibo.com/spuout),表示各种听不懂.因为听不懂,也不好做什么评价了.

本来,我觉得在一个面向js程序员的大会上讲如何写一个代码生成器是否合适主题?不过,后边翻了一下微博就找到同吐槽看不懂,不过有个看懂了的回复说其实是符合主题的.

> [跟js同步异步转换有关，所以在这里讲也不过分](http://weibo.com/2039445353/Ba2Fao0N8) 具体内容点击传送门吧.

PS: 翻了一下微博,还是有人有不错的收获例如 [题叶](http://weibo.com/1651843872/BabaT3RU8)

只能说自己的水平不行..orz


#### 视频传送门

|在线播放|视频下载|
|:--|:--|
|[土豆](http://www.tudou.com/programs/view/Wm1gC_2kx-k/)|[又拍云](http://hangjs.b0.upaiyun.com/videos/08.Belleve%20Invis%20-%20Patrisika%20-%20Theoretical%20and%20Practical%20Code%20Generation.mov)|

### 并不新鲜的快速构建MVC应用

接下来是[芋头](http://weibo.com/676588498)分享的 [如何快速构建MVC应用](http://2014.jsconf.cn/slides/Rabbit.js-MVC.pdf).

实际上这个slide主要是讲[Rabbit.js](https://github.com/xinyu198736/Rabbit.js) 的设计思想,传统的mvc 加上一套约定的web框架,跟paypal 的[kraken-js](https://github.com/krakenjs/kraken-js)有点类似,不过有个特点就是Rabbit.js 封装了一个Sql 和 nosql的模型定义,可以让你的代码在mysql 和 mongodb 都能够运行的很好.

芋头在slide上一再强调Rabbit.js还没正式发布,然后,各位有兴趣的话可以看着学习一下,看能不能对自己的开发有所启发.


#### 视频传送门

|在线播放|视频下载|
|:--|:--|
|[土豆](http://www.tudou.com/programs/view/qEKEHAeNHTI/)|[又拍云](http://hangjs.b0.upaiyun.com/videos/07.%E8%8A%8B%E5%A4%B4-%E5%A6%82%E4%BD%95%E5%BF%AB%E9%80%9F%E6%9E%84%E5%BB%BAMVC%E5%BA%94%E7%94%A8.mov)|

### 原标题存在的矛盾性

今天最后一场slide是由百度工程师[@berg](http://weibo.com/berg) 带来的目前关于如何让H5的应用运行得很流畅的解决方案: [BlendUI - 让轻应用如Native般流畅](http://2014.jsconf.cn/slides/BlendUI.pdf)

在这个slide开讲前,我对这个slide其实挺期待的,因为,我一直都有关注h5在app上的应用,对于用h5做的应用,最无解的问题就是动画的流畅性,对于现在,IOS 的原生webview性能已经很好了,在未来的ios8 webview的js引擎终于用上跟safari 一样的引擎,并且支持webgl,然后,随着硬件的各种升级,最终让基于H5的应用达到本地应用的流畅性(未来两年内应该可以实现),不过这都是未来,现在还是老老实实写本地应用吧.

然后看到这个标题,心里还是挺期待在当前硬件无法得到解决的情况下,是否存在什么黑科技,让H5应用达到一个飞越?

在听这场slide的时候,果然,不存在什么黑科技! BlendUI 的解决方案,并不是打算死磕纯DOM黑科技实现,而是,应地制宜用了一些技巧.原生DOM动画,不流畅,那么动画这部分,我不用DOM来实现了,直接上本地,这下动画切换的流畅问题就完美解决了.每个页面就是一个webview,页面的切换就是两个webview的本地切换,效果妥妥的流畅! 多webview 内存占用多? 现在千元手机都2G内存了,妥妥的!而且开多个webview其实并没有那么耗内存!

于是BlendUI 如Native般流畅 实际上因为动画部分就是native, 那还不是如Native般流畅....所以,我说这个标题存在的矛盾性就在这里.不过,可能是我对轻应用的了解不一样,我对轻应用的了解的就是能用纯H5实现,不套壳能用浏览器运行,套壳就是APP,只要有webview控件的平台都能支持.因为轻嘛,所以,跨平台运行妥妥的。

对于用多webview的动画切换来解决H5类型应用的方案,其实早在一年前我就有尝试过,在这里会有几个坑的,后边after party 找berg实机体验了一把,看来这个坑berg就目前这个版本并没有解决,关于这点,后边after party在展开说.

> 小插曲: 在提问环节,有个人问berg,目前百度轻应用有没有什么明星应用,如果没记错的话,berg回答的是还没有...


## After party

对于每届中国JS开发者大会,最有价值的就是这个after party了,可以面对面的找各种大牛进行交流.

### BlendUI

我找到了berg,借手机实机体验了一把BlendUI,安装blendui 的轻应用是一台小米2, 有2G 内存.我针对过去研究碰到的坑,立马测试了一下,于是还是碰到了.总结如下:

* 有一定机率webview和原生控件同时存在一个页面. 这个坑很怪异,挺难重现的,就是你的webview有张图片,你点击这个图片用原生控件来显示,在某种操作的情况下,这个原生显示图片的控件会和webview在你没点击图片的时候同时显示.

* webview 丢失.这个坑就很好比较容易重现.表现是当你,在点击webview的某张图片后,是显示一个覆盖这个webview图片滑动控件,然后快速后退,在快速点击某个item,重复快速的操作前面的步骤,就会有一定几率无论你点那个item也不会显示webview而是一直显示这个滑动图片控件.

不过BlendUI 并没有正式发布,所以以上bug可能到时候已经修复了.对了,不知道到时候,在目前千元手机1G内存下的表现又会是如何呢?

### 随便转转

我线下的即兴交流能力实在是太差了,跟berg 简单交流了一下,就是走走听一下,大家都在讨论什么,那些围着外国友人的交流的实在高端,简单的听了一下,表示无力....

期间跟一个大众点评的员工简单交流了一下,发现,大众点评在一些小业务上也用上了Node.js,而且,还用上了docker.

PS: 我的交流能力有待提高啊....

## 附录

其他人的hangjs 参会记录

[梁杰_numbbbbb 技术大会到底该怎么听？](http://weibo.com/p/1001603724334156923311)

[民工精髓V 杭JS参会记录](http://weibo.com/p/1001603724961759021973)

[lisposter Hang-JS-2014](http://zhuli.me/hang-js-2014/)

[鹄思乱想 2014 杭JS 杂谈](http://www.thinkingincrowd.me/2014/06/25/hangjs-2014/)

[fsiaonma 继《京JS》后，再遇《杭JS》](https://github.com/fsiaonma/fsiaonma.blog.com/issues/5)