title:  Android 应用的动画实践--View Animation篇
date: 2013-05-30
tags: android
---

##前言##
尝试搜索了一下android 动画的中文资料，很多都是一些枯燥的翻译api的一些文档，很少有系统讲解如何利用动画开发一个应用的资料，忽然，发现很多应用也不怎么注重动画在app的应用，想了想，自己尝试总结一下吧。因为，本人也不是什么动画制作师，没法把动画做得很绚丽，只好，利用内置的效果，进行简单加工，如何发挥，由各位的创意来定。鉴于，很多有关的android的动画资料里面，都是堆代码的，所以，[全部代码均放在了github上面，查看完整代码可以移步到github上面去](https://github.com/youxiachai/AnimUtils)。


**特地说明一下，由于android 模拟器和录制工具的原因，例子展示中的gif 的抽筋播放效果不等同于实际效果，自己脑补把抽筋的部分去掉**
<!--more-->
##android 动画基础##

在Android 里你能够使用的动画效果：

* 平移

* 缩放

* 旋转

* 透明


以上动画的基本使用就是本文的内容了。由于，本人的能力问题，实在搞不出让人眼前一亮的动画，就凑合着看着吧。不过，那些令人赞叹的动画效果的基础就是这些。

###Interpolators（插值器）###

一般而言，要做动画的，需要封装点物理公式，用作为计算帧与帧间的数值计算，不过，如果，只是，为了搞些动画让app好用一些，倒不需要搞得这么复杂，android 官方api 已经封装好了一些常用的动画插值器。

默认内置7种类型的插值器，个人觉得，如果只是应用里面的一些动画的话这7个就够用了。

1. AccelerateInterpolator
>  加速 <br/>
> ![AccelerateInterpolator](/images/androidanim/AccelerateInterpolator.gif)

2. Decelerate
>  减速 <br/>
> ![decelerate](/images/androidanim/decelerate.gif)

3. AccelerateDecelerateInterpolator
> 开始，和结尾都很慢，但是，中间加速<br/>
> ![accelerate_decelerate](/images/androidanim/accelerate_decelerate.gif)

4. AnticipateInterpolator
> 开始向后一点，然后，往前抛<br/>
> ![anticipate](/images/androidanim/anticipate.gif)

5. OvershootInterpolator
> 往前抛超过一点，然后返回来<br/>
> ![overshoot](/images/androidanim/overshoot.gif)

6. AnticipateOvershootInterpolator
> 开始向后一点，往前抛过点，然后返回来<br/>
> ![anticipate_overshoot](/images/androidanim/anticipate_overshoot.gif)

7. BounceInterpolator
> 结束的时候弹一下<br/>
> ![bounce](/images/androidanim/bounce.gif)

8. LinearInterpolator
> 匀速

以上动画都源自android官方api demo，用eclipse adt android 选择例子项目导航，然后，选择APIDEMOS 就能创建（什么没听说过？现在知道了吧。。。）

好了，虽然截取的gif 动画播放起来有点抽筋的感觉，接下来我们该如何在应用中使用这些知识呢？

###组合动画###
**目前讲解动画api 的资料比较多，这里就不在重复那些基础的知识了！**

现在让我们学习一下，如何利用，平移，缩放，旋转创造出让人眼前一亮的动画.

为了，更有目的的使用动画，下面假想一个使用场景。

####假想：商品购物车案例####
>Notice :为了方便看效果，动画延时时间将会设置的比较长。特地说明一下：假想就是随便想，切勿对号入座。

任务：

为了，让商城app有更好的交互效果，决定对购物车控件和商品控件上面加一些动画效果。

购物车动画设计方案：

利用，透明，平移，对购物车的出现和离开增加动画交互效果。

[经过一番努力效果如下(凑合着看吧。。)：](https://github.com/youxiachai/AnimUtils/blob/master/AnimUtils/res/anim/in_translate_top.xml)
>![anim1](/images/androidanim/anim1.gif)

####相关知识点####
一些动画常用的通用基础属性：
>Notice: 所谓通用就是说所有动画标签都适用于这些属性

* `android:duration` 设置动画播放的时间
* `android:startOffset` 设置动画的开始播放时间
* `andorid:interpolator` 设置动画的插值器
* `android:repeatCount` 动画播放的常用次数
* `android:repeatMode` 动画重播的模式，即从头到尾，从头到尾，还是从头到尾，在从尾到头。

透明的使用:

`<alpha />`
> value 从 0 （透明） 到 1 （不透明）
在android中透明主要用于对view 淡入，淡出的效果控制主要有两个属性

* `android:fromAlpha` view在动画开始的透明度。
* `android:toAlpha`  view在动画结束的透明度。

平移的使用：

`<translate />`
> 支持使用 %，如 “50%“ 获取的是这个view的百分之50，除此之外还有另外一种写法：”50%p“ 意思是获取这个view的上一级view的百分之50 当然，指定特定值也是支持的“22.2”，不过为了兼容更多的android设备建议还是使用百分比的值。

* `android:fromXDelta` 

*  `android：fromYDelta`
>from?Delta 意思是开始的轴线

* `android:toXDelta`
 
*  `android：toYDelta` 
> to?Delta 意思是结束的轴线

这次的方案展示了两个插值器的使用：

用于出现的：BounceInterpolator

用于离开的： AnticipateInterpolator


###什么是插值器？###
所谓插值器就是用于数值的起始间的变化，就是相当于一个类似于物理引擎的东西。android官方内置了一些简单常用的数值变换，让我们，不需要去学习相关的物理知识。

例如：

开始值为1，结束值为 100.那么我们如何控制变化这个值的变化过程呢？这里就是插值器的使用。

一般匀速的话就是：

1,2,3,4,5...100。 然后我们就会看到物体以一个匀速的速度进行平移操作。

那么我们需要物体像汽车那样加速度的前进，我们可以用加速插值器，我们从1到100的过程，就会是：

1,2,4,5,8，16.。。。。100 展示在我们面前的view对象就会以一个加速度的形式进行平移。

有很多应用开发者并不熟悉动画制作的一些基础知识，可能不太明白。现在，通过对源码进行分析，来彻底搞明白这个概念。

我们分析一些Interpolator 类树：

从api文档[TimeInterpolator](https://developer.android.com/reference/android/animation/TimeInterpolator.html) 我们可以知道，这个插值器的实现只有一个方法：

> `getInterpolation(float t);`

然后我们挑选前面用过的[BounceInterpolator](https://github.com/android/platform_frameworks_base/blob/master/core/java/android/view/animation/BounceInterpolator.java) 看下，它是如何实现这个方法。如果感兴趣的，可以按照这种方法，把其他几个插值器的实现都看一遍。

最后我们会发现，插值器的作用就是返回值。

接着我们来看下[Animation line:869](https://github.com/android/platform_frameworks_base/blob/master/core/java/android/view/animation/Animation.java) 是怎么用这个接口的. 

看完这这几个地方，相信应该对android 动画框架怎么对值进行变换的原理应该有所了解。

有了以上知识，我们对android的动画框架基本上已经完全了解，现在，我们利用学到的知识，进行更好的动画设计。

我们接着刚才的案例，着手设计商品控件的动画设计

商品动画设计：

这次，我们学习一个新的动画标签缩放(`<scale>`)

效果如下：

![shop1](/images/androidanim/shop1.gif)

`<scale />`
> 使view 大点或者小点

* `android:fromXScale` 

* `android：fromYScale`
>from?Scale 意思是开始轴线的缩放比例（默认 1.0）

* `android:toXScale`
 
*  `android：toYScale` 
> to?Scale 意思是结束轴线的缩放比例（默认 1.0）

* `android:pivotX`

* `android:pivotX`
>  旋转用的轴点坐标

最后我们把购物车的动画，和商品的动画在组合起来。效果如下：

添加商品的时候，如果购物车还没出现，先出现购物车显示的动画，在进行商品的动画播放。[具体实现 line: 77 -104](https://github.com/youxiachai/AnimUtils/blob/master/AnimUtilsExample/src/com/youxiachai/animutils/example/MainActivity.java)

![shop2](/images/androidanim/shop2.gif)

这次我们学习一下如何监听动画的动作，对于`AnimationListener()`主要有三个

* `onAnimationStart(Animation animation)`

* `onAnimationRepeat(Animation animation)`

* `onAnimationEnd(Animation animation)`










