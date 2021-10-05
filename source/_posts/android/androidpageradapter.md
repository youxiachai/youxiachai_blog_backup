---
title: Android AdapterView 源码分析以及其相关回收机制的分析
tags: android
categories:
  - android
date: 2013-05-10 00:00:00
---

##前言##
忽然，发现，网上的公开资料都是教你怎么继承一个baseadapter，然后重写那几个方法，再调用相关view的 setAdpater()方法， 接着，你的item 就显示在手机屏幕上了。很少有人关注android adpater模式机制的实现原理，比较深入的也不过是说说adapter getview()中的回收情况。今天把相关的源码看了一遍，把自己的理解记录下来。
<!-- more -->

##AdpaterView 概览##

[AdpaterView](https://developer.android.com/reference/android/widget/AdapterView.html)
> api手册的说明：An AdapterView is a view whose children are determined by an Adapter.

实际上android里面ListView, GridView, Spinner ， Gallery等view都是基于设计模式上的设配器模式实现的，只要熟悉设配器模式的相关知识，就知道如何从源码里面找到相关的实现线索。

##认识AdapterView##
源码链接[https://github.com/android/platform_frameworks_base/blob/master/core/java/android/widget/AdapterView.java](https://github.com/android/platform_frameworks_base/blob/master/core/java/android/widget/AdapterView.java)

要理解listview等的实现，其父类是不得不看。源码有1200多行。阅读完AdapterView,能搞明白以下问题

1. 响应数据的更改。
>（793 - 842）

2. 知道点击view的时候，获得对应的位置.
>（593 - 615）


###响应数据的更改###
这里假设你已经打开了[AdpaterView](https://github.com/android/platform_frameworks_base/blob/master/core/java/android/widget/AdapterView.java) 的 793 到 842 行。。

在我刚开始用adapterview 的时候，最让我费劲的就是，为什么我调用adpater 的 notifyDataSetChanged() 就能更新view 的状态了呢，然后跟调用notifyDataSetInvalidated() 两者之间又有什么区别呢？以前，找了一下资料，没找到很详细的说明，现在从源码里面找答案的话，就很清晰了。

首先，我们要明白一种设计模式：[观察者设计模式](https://zh.wikipedia.org/wiki/%E8%A7%82%E5%AF%9F%E8%80%85%E6%A8%A1%E5%BC%8F)。

我相信你，应该能明白观察者模式是个什么样的实现了。。。

AdapterView 之所以能对Adapter 的数据更新进行响应，就是因为其在Adapter上注册了一个数据观察者（AdapterDataSetObserver(793 - 842 )）的内部类，所以，我们只要对adpater 状态的改变发送一个通知，就能让AdapterView调用相应的方法了。

> DataSetObservable 的源码，记得要把其父类也看了。 [https://github.com/android/platform_frameworks_base/blob/master/core/java/android/database/DataSetObservable.java](https://github.com/android/platform_frameworks_base/blob/master/core/java/android/database/DataSetObservable.java)

现在我们就能解决我们一开始的疑问notifyDataSetChanged() 与notifyDataSetInvalidated() 具体回到AdapterView 产生什么影响？

我们对比一下`onChange()` 与 `onInvalidated()` 方法，就能对比得出，前者会对当前位置的状态进行同步，而后者会重置所有位置的状态。从代码的注释里面还可以获取得到更多的信息。

这样，我们以后调用notifyDataSetChanged()和notifyDataSetInvalidated() 就更加明白会发生什么情况了。

###点击item 怎么能够获取到当前的位置###
这里假设你已经打开了[AdpaterView](https://github.com/android/platform_frameworks_base/blob/master/core/java/android/widget/AdapterView.java) 的 593 - 615 行。。

对于`getPositionForView()` 这个方法，你肯定没用过，要搞明白为什么我们能够获取到adapterView 里面item view对应的位置，我们需要看
其直接子类：[AbsListView.class](https://github.com/android/platform_frameworks_base/blob/master/core/java/android/widget/AbsListView.java)
>  源码相关：（2130-2197） （2196 - 2279）

这里又用到一种设计模式：[委托模式](http://zh.wikipedia.org/wiki/%E5%A7%94%E6%89%98%E6%A8%A1%E5%BC%8F)

假设你已经搞懂委托模式的概念，首先我们来看源码（2130 - 2197）。

从`obtainView()` 方法名中我们可以知道，这是一个用于生成itemView的方法。把这块代码看完，以后，会不会有个疑问呢（先不用管回收那块）？ **position** 到哪里了？我们可以看到这个方法实际上并没有对我们的itemview 设置了任何的监听器，那为什么最后能对我们的itemview的动作进行反应呢？ 

接下来我们看：源码（2196 - 2279）

从代码里面我们可以看出这是一个委托类，对item 的动作进行初始化，以及响应对应的操作，从源码里面我们可以获知得到，一个item view 为什么能对click，longclick，select 动作进行响应，然后，通过调用`performItemClick()` 最终把事件调用到AdapterView（292-303）的`performItemClick()` 里面的监听器方法.

如果，你对委托模式不熟的话，要明白这里的话，需要花点时间。




##认识 AbsListView 回收机制##
> 源码： [AbsListView.class](https://github.com/android/platform_frameworks_base/blob/master/core/java/android/widget/AbsListView.java)

长期以来，都有这么一个说法，listview 会自动把不可见的view进行回收，但是长期以来，我都没看到有人对其回收机制进行分析说明

###回收执行者：RecycleBin###
我们回到之前看过的[AbsListView.class](https://github.com/android/platform_frameworks_base/blob/master/core/java/android/widget/AbsListView.java)  
>`obtainView()（2130-2197）`

你会看到一个
>`mRecycler` 的变量。

接下来，通过搜索我们可以得知这个变量是在（308）进行初始化，这是一个内部类的
>RecycleBin的实例（6139 - 6507） 

看到这类，我们大致可以知道，这个类是这个absListView 回收机制的实现者。

>请 跳转到（6139）

现在，我们来看一下这个类的注释，大体的意思这个类是用来帮助复用view的，用2个不同级别的方式进行存储（The RecycleBin has two levels of storage）（个人感觉描述得挺变扭的，还是看原文好了。。）

1. ActiveViews ： 一开始显示在屏幕的view
2. ScrapViews： 潜在的一些可以让adpater 使用的old views。

然后，注释里面已经说了，ActiveViews 怎么变成 ScrapViews。就注释提供的信息这里我们有两个疑问。

1. 什么时候产生 ActiveViews。
2. 什么时候产生 ScrapViews。

这要把这两点搞清楚了，整个回收体系也就清楚了。

###AbsListView的回收机制具体实现###
从RecycleBin类的注释里面我们获知，回收机制的第一步就是屏幕的view 放在ActiveViews，然后通过对ActiveViews进行降级变成ScrapViews,然后通过scrapViews 进行view 的复用

通过，一番的检索，我们在[Listview.class](https://github.com/android/platform_frameworks_base/blob/master/core/java/android/widget/ListView.java)(1562行里面找到`fillActiveViews()`的调用）。

我们观察一下[Listview.class](https://github.com/android/platform_frameworks_base/blob/master/core/java/android/widget/ListView.java)（1460 - 1713） 看一下`layoutChildren()`这个方法是干嘛用的。

当我们看到（1550）行的时候，就会发现了这个回收类的赋值。接下来我们看下
listview是如何利用回收机制：

1. 当数据发生改变的时候，把当前的view放到scrapviews里面，否则标记为activeViews（1557 - 1562）
2. `recycleBin.removeSkippedScrap();` 移除所有old views
3. ` recycleBin.scrapActiveViews();` 刷新缓存，将当前的ActiveVies 移动到 ScrapViews。

这里干了些事情呢？我们回到（1557 - 1562） 我们可以看到一个变量dataChanged，从单词的意思我们就可以，这里的优化规则就是基于数据是否有变化，我们通过搜索成员变量`mDataChanged`在 (1693) 的时候变成了false 接着我们在`makeAndAddView`（1751 - 1775）发现了这个变量的使用。

阅读（1756 - 1766） 我们可以看到回收机制的第一次使用，如果数据没有发生改变，通过判断ActiveViews(这些些view来自（1557 - 1562）) 列表里面有没有当前 活动view，有的话直接复用已经存在的view。这样的好处就是直接复用当前已经存在的view，不需要通过adapter.getview()里面获取子view。

好了，接下来我们来看下`makeAndAddView`（1751 - 1775） 是如何通过`adapter.getview()`中 获取到view。我们回到[AbsListView.class](https://github.com/android/platform_frameworks_base/blob/master/core/java/android/widget/AbsListView.java)（2130 - 2194）

在 （2134） 中我们看到一个很神秘的方法` scrapView = mRecycler.getTransientStateView(position);` 从单词的意思里面我们可以得知这是获取一个瞬间状态的view，这里就有个疑问什么是瞬间状态的view？通过对源码的层层分析终于在View 类的 hasTransientState()方法里面找到描述。从描述中我们得知这个方法是用来标记这个view的瞬时状态，用来告诉app无需关心其保存和恢复。从注释中，官方告诉我这种具有瞬时状态的view，用于在view动画播放等情况中。

那么，我们就可以明白这句话优化的是absListView 的列表动画.

接着阅读到一下代码的时候，我就困惑了
>`scrapView = mRecycler.getScrapView(position);`

从这行代码里面我们可知，复用的review是跟位置有关的，我们回去在看看(ListView 1557-1563)
>            if (dataChanged) {
                for (int i = 0; i < childCount; i++) {
                    recycleBin.addScrapView(getChildAt(i), firstPosition+i);
                }
            } else {
                recycleBin.fillActiveViews(childCount, firstPosition);
            }

我们可以发现，实际上这里放进回收类里面的只有当前的显示的view，并没有产生当前屏幕没有的view，但是，实际使用中，当我们进行滚屏的时候，显示下个view的时候，就已经能发现getView 第二个参数已经不为null了，那实际实现在哪里了，我们通过搜索用到RecycleBin 的方法，找到

>`layoutChildren()`

>`scrollListItemsBy()`

>`onMeasure()`

>`measureHeightOfChildren()`

通过查看
>`scrollListItemsBy()` 

我们就能够明白，当我们进行滚屏的时候，在listview 移除item view 的时候，把移除的item view放进了
>`  recycleBin.addScrapView(last, mFirstPosition+lastIndex);`

于是生成下一个view的时候就能够复用之前的view了，搞清楚这个机制以后我们回到
> [AbsListView.class](https://github.com/android/platform_frameworks_base/blob/master/core/java/android/widget/AbsListView.java)（2139 - 2168）

接下来代码， 解答了我们一个经典的adapter 优化方法的由来
>       View child;
        if (scrapView != null) {
            child = mAdapter.getView(position, scrapView, this);

            if (child.getImportantForAccessibility() == IMPORTANT_FOR_ACCESSIBILITY_AUTO) {
                child.setImportantForAccessibility(IMPORTANT_FOR_ACCESSIBILITY_YES);
            }

            if (child != scrapView) {
                mRecycler.addScrapView(scrapView, position);
                if (mCacheColorHint != 0) {
                    child.setDrawingCacheBackgroundColor(mCacheColorHint);
                }
            } else {
                isScrap[0] = true;
                child.dispatchFinishTemporaryDetach();
            }
        } else {
            child = mAdapter.getView(position, null, this);

            if (child.getImportantForAccessibility() == IMPORTANT_FOR_ACCESSIBILITY_AUTO) {
                child.setImportantForAccessibility(IMPORTANT_FOR_ACCESSIBILITY_YES);
            }

            if (mCacheColorHint != 0) {
                child.setDrawingCacheBackgroundColor(mCacheColorHint);
            }
        }
 
实际上所谓的优化，就是通过利用已有产生的View进行复用，减少在Adapter.getView()进行类的实例化操作优化性能。
>从某年google io的文档中我们得知这个回收机制的效率能够提供listview 300%的效率。

接着我们还明白了
> getView(int position, View convertView, ViewGroup parent) 这个三个参数的由来了。

通过，对回收机制的分析，我们可以查看
> listview scrollListItemsBy()

的时候应该注意到，实际上不可见的 item 是会被自动移除，那样为什么当滚动过多的item的时候会发生oom的情况了？

在我们阅读完整个回收机制的时候，我们会发现回收机制实际上是通过在内存里面缓存view对象，让listview能够快速的获取view使listview的显示流畅。而导致OOM的问题也出在这里，由于整个回收机制把所有的imageview中的bitmap对象也保存下来，在进行不断的滑屏操作中，RecycleBin 类越来越大，最终导致OOM 的发生。

当然，根据整个思路，要避免OOM实际上也很简单，我们只需要在虚拟机中开辟一个内存块，专门用于保存bitmap对象的 map对象（一般而言用LRU算法实现），所有的imageview的应用都通过这个map 对象进行引用，当这个map对象大于一定程度的时候释放部分bitmap，这就可以保证RecycleBin在保存这些imageview的时候，而这些imageview里面的bitmap对象时通过一个固定的内存块里面获取，只要我们开辟的用于引用的bitmap 的内存块的大小合理，那样就永远也不会发生oom了。




至于其他继承自AbsListView 的View 其回收机制都一样。。


##感想##
花了，几个小时，把AdapterView 相关源码看完，大致计算了行数有3w 来行代码了，当然，不会是一行不漏的看过去。 这里分享一个看源码的方法。首先，有接口和，抽象类的地方，一定要把所有方法看全，这一块基本上是属于要一行不漏的看完。实际上这些接口，和抽象类是我们看源码重要的索引，那些4，5k行的代码，实际上，里面的关键，都是这些接口，和相应的抽象类的扩展。




