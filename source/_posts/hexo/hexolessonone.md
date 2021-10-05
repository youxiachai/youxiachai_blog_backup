---
title: Hexo 简明入门教程（一）
tags: hexo
categories:
  - hexo
date: 2013-04-04 05:00:00
---

##导言##
对于个人独立博客的搭建，或者一些产品网站的介绍我个人比较推崇直接用静态网站生成器来完成这个事情，对于，静态网页部署方便，浏览速度快。

以下为部分静态网站生成器简要列表

###Ruby###
1. Jekyll （github 默认pages 引擎）
2. Octopress （兼容jekyll）

###Python###
1. Hyde Jekyll的Python语言实现版本
2. Cyrax 使用Jinja2模板引擎的生成器

###PHP###
1. Phrozn PHP语言实现的静态网站

###JS###
1. Hexo

<!-- more -->
如果你只是想了解什么是静态网站生成器，

##Hexo 介绍##
[Hexo](https://github.com/tommy351/hexo) 是一款基于node 的静态博客网站生成器

作者 ：[@tommy351](https://twitter.com/tommy351)是一个台湾的在校大学生。。。

相比其他的静态网页生成器而言有着，生成静态网页最快，插件丰富（已经移植了Octopress 插件）。


关于如何建立一个Hello World级别的Hexo 官方github主页已经很清晰的说明了，不做重复，直接跳过。

[https://github.com/tommy351/hexo](https://github.com/tommy351/hexo)

##配置介绍##
在学会了，如何运行Hello World 的Hexo 我们要开始进行自定义化了，首先我们要了解hexo的静态化规则。
打开项目文件的根目录你会看到这个文件：

_config.yml

关于设定的文档 [http://zespia.tw/hexo/zh-CN/docs/configure.html](http://zespia.tw/hexo/zh-CN/docs/configure.html)

这里值得注意的配置参数

###部分配置说明###
#####URL 部分####
**root** 这个参数是用于配置网站的根目录，与最终生成的网页资源链接相关的。例如有一个js文件默认的

`root: /`

最终网页里面的资源文件会链接到 

`/fancybox/jquery.fancybox.pack.js`

改为 `root: hello`

`/hello/fancybox/jquery.fancybox.pack.js`

**permalink** 这个是用于设置文件的存放规则。例如

默认的配置 `:year/:month/:day/:title/`

最终生成的文章存放于public 文件下的
2013/04/04/xxx.html

改为`:year-:month-:day/:title/` 将会变成2013-04-04/xxx.html

需要改变文件的存放规则记得注意。

###写作###

[官网的入门资料](http://zespia.tw/hexo/zh-CN/docs/writing.html)

由于，整个网页的生成规则是基于目标的关系，所以在写作部分你只要专注于文章的编写就行，写好的文章直接放在`source/_posts/` 下即可。更多的设定记得认真参考官方的设定。

**任一页面生成**

有些时候，对于非文章类的页面，例如一个aboutme的页面，其实我们只有直接放到`source/` 即可，路径的规则由创建的文件夹路径一致。

**Read more长度的控制**

hexo 的readmore 是由自己在写文章的时候设定的，在文章正文里面部分的合适位置加上`<!-- more -->` 首页的预览就会到标识的位置

###总结###
上面提到的内容，已经足够利用hexo搭建一个完整的博客网站，下次，我会说说如何自定义hexo的主题。

实例网站：该教程的实例演示 [http://blog.gfdsa.net](http://blog.gfdsa.net) 部署在github pages 上