---
title: Grunt 新手指南
tags: node
categories:
  - nodejs
date: 2013-08-31 00:00:00
---

##导言##
作为一个正在准备从java 后端转大前端,一直都有想着,在js 的世界里面有没有类似于maven或者gradle 的东西..然后,就找到了grunt 这玩意

<!--more-->
##Grunt是用来干什么的##
诸如ant,maven,gradle,make 之流的,那么我们为什么要学这么一个工具了,我们用IDE编程不是好好的吗,要让人去学这么一个工具,那么必然要有这个工具能够为我们搞定什么的原因.

选择Grunt原因

1. 管理我们的文件依赖
2. 随心所欲的批处理任务
3. 整合常用的前端工具,js混淆,文件合并压缩.

说了这么多,上面就是我们为什么要选择grunt.js 作为我们项目构建的工具,如果你没有任何项目构建的概念,我建议了就不要看有关grunt的任何资料了,包括本文.因为,你看不懂我接下来我要写东西,也看不懂任何有关grunt相关资料,所以,就不要浪费时间了.


##让Grunt 干活##
如果,你之前有接触过构建工具,或者你现在有项目构建的概念,那么任务(tasks)这个概念想必理解起来不会有太大的难度了.

###创建我们第一个任务###
只要在我们的Gruntfile.js 文件写上这么几句

``` js
module.exports = function (grunt) {
    grunt.registerTask('test', 'my first tasks', function () {
        grunt.log.write('Hello World!').ok();
    });
}

```

接着我们只要在当前目录运行
`grunt test`

就能看到控制台输出

> Hello World.

接下来咱们有个node 环境就可以想干嘛的就干嘛了..停住!如果只是这样,这跟我们写个shell脚本有什么区别呢?实际上grunt跟shell 脚本没什么区别,只是grunt有一个node 运行环境,可以比写shell脚本简单那么一些,如果你已经是shell脚本达人,我觉得没有再学grunt必要了.

###任务的任务###

有时候,我们有很多任务,不过这里任务,都可以归类为一中,我们就需要注册一个多任务来处理这种情况,例如,文件的操作就有,创建,打开,重命名,这些任务都可以归类为文件操作任务

``` js
module.exports = function (grunt) {
  grunt.initConfig({
    file: {
      create: 'source file',
      open: 'open file',
      delete: 'delete file'
    }
});
  grunt.registerMultiTask('file', 'Log stuff.', function () {
    grunt.log.writeln(this.target + ': ' + this.data);
  });
}
```

这个时候我们运行的时候,就会看到如下接口

`grunt file:create`
>create: 'source file'

`grunt file:open`
>open: 'open file'

`grunt file:delete`
>delete: 'delete file'

那么在我们自定义多任务的时候,可以通过`this.target` 获得当前任务命令,然后通过`this.data` 获取到我们的配置值,接下来就是发挥你的想象力的时候了.

##总结##
实际上grunt不是什么神奇的时候,它最不过是一个运行在node的一个命令行工具,可以方便我们用js写脚本而已.

##插件推荐##

1. grunt-contrib-watch
    * 监听文件修改
2. grunt-curl
    * 想curl 下载远程js
3. grunt-contrib-clean
    * 文件清理工具
4. grunt-contrib-cssmin
	* css压缩工具
5. grunt-contrib-copy
	* 文件复杂工具


