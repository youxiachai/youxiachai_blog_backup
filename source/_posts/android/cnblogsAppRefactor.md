title: 重构博客园Android App
date: 2013-06-20
tags: android
---

##前言##
第一个全功能的非官方android客户端已经过去一年了...目前貌似已经不再更新的样子?最近发现,在android 4.1上运行的时候,列表都不能滚动了..而且,原界面设计,也不适合放在android 平板上使用,看了一下源码,跟我的编写风格出入挺大的,于是,就写一个我的博客园android 客户端.

**ps: 本人在广州正在 找nodejs 工作 不知道有木有推荐一下? 联系邮箱:youxiachai@gmail.com**

<!--more-->
##客户端规划##
看了一下,博客园开放的API,没发现有闪存的API,所以没有目前暂时不打算实现关于用户信息这块的内容,目前提供来看用户登录最大的作用也就收藏一个文章,个人感觉意义不大....

###目标:###

1. 自适应android 手机和平板
2. 扁平化的设计风格
3. 文章自动离线保存
4. 支持代码样式的博客内文

然后花了昨天和今天,两天时间,终于把一个原型app 完成,看了一下,完成度还挺高的,首先要感谢[@walkingp](http://www.cnblogs.com/walkingp/) 的贡献.

###当前版本的进度:###

1. android 和平板的响应式设计
2. 完成新闻列表,和博客列表的api

编码花了两天,前天,写设计稿,单元测试,昨天敲代码,今天发布文档...

###TODOLIST###

1. 完善界面
2. 实现新闻内容和博文内容的显示 
3. 博文内容里面的代码支持样式(长期计划)

##自适应设计##
现在android 平板已经不少了,android其实提供了一套很好用于兼容,手机和平板的机制,让我们不需像ios 那样做两个app..

看图吧

###手机导航###
手机上显示的导航为抽屉式导航:

**以下均为示意图,吐槽难看,前面已经说过原因了...**

![抽屉式导航](/images/cnblogsapp/phoneNav.jpg)

新闻列表

![新闻列表](/images/cnblogsapp/phonenews.jpg)

博客列表

![博客列表](/images/cnblogsapp/phonebloglist.jpg)

###平板导航###
平板上显示为 actionbar Tabs 式导航:

新闻列表

![新闻列表](/images/cnblogsapp/tabletNewsList.jpg)

博客列表

![博客列表](/images/cnblogsapp/tabletbloglist.jpg)

有兴趣当白老鼠的可以下载打包好的APK([项目主页](https://github.com/youxiachai/CnBlogs4Android)))....不过,不保证能够完美运行在所有android上.....

下一次再见就是项目完成的时候了....目前没有ROADMAP....

##支持本项目##
如果,你对这个有点兴趣,愿意支持一下,没有什么比捐点线实在了...

[![](/img/pay_encourage.png)](http://me.alipay.com/youxilua)


##关于本项目用到的库##
这个项目基于gradle 构建(发现 0.4.2 还是有bug...作为保留工具,目前主力工具还是adt)...由于依赖库的位置问题,目前而言,还不能直接fork就能跑..而且也不建议这个时候下载,或者fork,因为,还有很多地方会有改动.

项目地址 : [https://github.com/youxiachai/CnBlogs4Android](https://github.com/youxiachai/CnBlogs4Android)

###本人写的类库###

ActionTitleBar : [https://github.com/youxiachai/ActionTitleBar](https://github.com/youxiachai/ActionTitleBar)

OneXListView : [https://github.com/youxiachai/OneXListview](https://github.com/youxiachai/OneXListview)

ajaxQuery : [https://github.com/youxiachai/ajaxAquery](https://github.com/youxiachai/ajaxAquery)

嗯..以上类库目前皆无文档....不过,以后会有的...


###公共类库###

SlidingMenu : [https://github.com/jfeinstein10/SlidingMenu](https://github.com/jfeinstein10/SlidingMenu)