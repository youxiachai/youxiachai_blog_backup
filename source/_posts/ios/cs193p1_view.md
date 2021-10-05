---
title: iOS 第三周学习笔记 View
tags: iOS
categories:
  - ios
date: 2015-09-21 00:00:00
---

## 前言

进入，iOS学习的第三周，这周犯懒，以至于，ios学习的进度，并没有什么大的进步，这周就只把tabview，学习了一遍，还有一些杂项，ios9 https的兼容，pod怎么用这类的。

<!--more-->

## TableView

需要学习的几个方法
```

override func numberOfSectionsInTableView(tableView: UITableView) -> Int {
        return 0
    }

 override func tableView(tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
 return 0
 }
   override func tableView(tableView: UITableView, cellForRowAtIndexPath indexPath: NSIndexPath) -> UITableViewCell {
        let cell = tableView.dequeueReusableCellWithIdentifier(Storyboard.CellReuseIdentifier, forIndexPath: indexPath)

        return cell
    }

 override func tableView(tableView: UITableView, didSelectRowAtIndexPath indexPath: NSIndexPath) {}
```

待续。。。








