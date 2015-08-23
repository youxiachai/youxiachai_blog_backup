title: Android Fragment Tips
date: 2015-05-15
tags: android
---
##前言

Android 3.0 以后的引入的fragment,现在基本已经成为开发android程序必不可少的东西,期间,虽然有人对fragment提出反对[advocating-against-android-fragments](http://corner.squareup.com/2014/10/advocating-against-android-fragments.html)

虽然如此,我们还是要学会使用fragment.

目前,对于fragment机制,其实来来去去也就是官方那几张图的复述,所以,本文着重说说fragment使用tips,希望能给大家一点启发

<!--more-->

## 使用support,不使用官方api

很多人慢慢的吧app的sdk提高到android 4.0,为了减少support包几百k的容量而去使用原生支持的fragment api.

实际上,这个不是很可取的做法,早期的fragment 实现是有不少bug的,而且,一些新增,但是很有用的api,并没有在早期sdk出现,例如,
```
public final FragmentManager getChildFragmentManager ()
```

这个获取fragment自身的fragmentmanager官方sdk要到4.2才支持,为了省几百k的字节让自己难受,实在得不偿失.

## 尽可能动态生成Fragment


## FragmentManager 的栈管理实践

### 栈监听

### 栈后退

### 栈任务

## FragmentTransaction 的操作

### add/show/hide

### replace/remove


## Fragment 特别方法

```java
setRetainInstance

setTargetFragment

startActivityForResult
```



