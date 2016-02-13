title: iOS 第二周学习笔记 Controller
date: 2015-09-13
tags: iOS
---

## 前言

进入，iOS学习的第二周，这周挺忙的，以至于这周学习iOS的时间就不多了，这周估计也就只能搞懂怎么用好controller

<!--more-->

## 回顾

只看Stanford c193p 对于iOS的学习还是远远不够的，除了Stanford的课程，我还发现了，官方提供的入门课程也很值得学习，用于检验对Stanford的教程究竟掌握了多少还是有帮助的

[https://developer.apple.com/library/prerelease/ios/referencelibrary/GettingStarted/DevelopiOSAppsSwift](https://developer.apple.com/library/prerelease/ios/referencelibrary/GettingStarted/DevelopiOSAppsSwift)


## 导航

一个应用最核心的一个地方，就是要如何控制页面的跳转，Stanford的教程就重点说了一个Split view controller, 这个是远远不够的，我们还需要认真去读官方文档，学习每种controller的用法

> [官方Controller文档](https://developer.apple.com/library/ios/documentation/WindowsViews/Conceptual/ViewControllerCatalog/Introduction.html#//apple_ref/doc/uid/TP40011313-CH1-SW1)

### Segue

就我目前所知两个controller 之前进行交互式的时候，我们要熟悉controller的两个方法

#### 全自动

```swift
override func prepareForSegue(segue: UIStoryboardSegue, sender: AnyObject?) {}
```

如果，我们只使用storyboard控制按钮跳转到另外一个controller，只要搞懂这个方法的用法就行了。

当我们使用storyboard 连好线后，点击那个button，系统就会自动的调用这个方法

对于这个方法，我们需要搞懂UIStoryboardSegue 这个类的用法

关于这个类，我们关键需要知道

* UIStoryboardSegue.destinationViewController
* UIStoryboardSegue.identifier

第一个UIStoryboardSegue.destinationViewController，这个告诉了，我们跳转的controller 是一个什么类型的controller，一般，我们这个时候就会使用as? 转型成我们目标conroller，然后开始对这个目标controller进行操作

第二个UIStoryboardSegue.identifier，这个是segue唯一标识，我们，在storyboard每连一条线，都需要设置id。用于设别我们是点击的是哪个button触发了这次跳转

#### 手动

除了在storyboard手动的对每个button连线跳转controller，当然也可以我们手动触发controller，这个时候，我们就需要以下这个方法

```swift
performSegueWithIdentifier(identifier: String?, sender: AnyObject?)

```

调用这个方法前，记得先在controller，对目标controller 进行连线，这次的连线是两个controller之间。

### 一些要注意的地方

#### 千万不要在prepareForSegue期间对outlet进行直接赋值

如果，你需要在跳转的时候，对目标controller进行一些outlet初始化，注意，不要直接赋值，因为prepareForSegue的时候，你的outlet还没实例化。

要需要赋值的话，这里介绍两种方式

##### viewWillAppear

在controller 显示的时候，进行修改
```swift
@IBOutlet weak var segueTitle: UILabel!
var helloSegue: String = "default"
override func viewWillAppear(animated: Bool) {
        super.viewWillAppear(animated)
        segueTitle.text = helloSegue
    }
```

#### 使用观察者模式

这个是我比较需要的方式，利用didSet这个特性，对outlet进行修改

```swift
@IBOutlet weak var segueTitle: UILabel! {
        didSet{
            segueTitle.text = helloSegue
        }
 }
var helloSegue: String = "default"
```


### 异步

当我们，网络读取图片的时候，这是一个比较耗时的操作，一般而言，我们是不会在主线程做这种事情，这个时候，我们就需要知道如何把一个操作放到另外一个线程操作，然后再把数据返回回来，在主线程进行显示。





