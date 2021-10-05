---
title: 用Gradle 构建你的android程序-依赖管理篇
tags: android
categories:
  - android
date: 2013-05-22 00:00:00
---
##前言##
续上一篇《用Gradle 构建你的android程序》，这次把上次没写的关于，如何用gralde 构建带有依赖的项目补全吧。

<!--more-->

##Gradle android 插件现况##
个人感觉还是说说，目前android gradle 插件的现况，如无意外应该是最新的。

目前最新的官方gradle android 是0.4，除了android 官方的gralde的插件，也有一些开发者很早以前开发的gradle 插件，不过现在基本不维护了，所以这里不对这些第三方的gradle插件进行介绍。

[android Gradle 0.4 插件maven中央库](http://search.maven.org/#artifactdetails|com.android.tools.build|gradle|0.4|jar),目前新的android gradle 构建系统基本完善，现在已知的问题有

1. 不支持android library 与 android library 的互相引用。
2. 不支持 NDK
3. 不支持android library 打包文件（*.aar） 的本地引用

如果，以上问题的你都碰到不到的话，从现在开始，用gradle来构建android程序是一个不错的选择。

##引用依赖##
**这里阅读的前提是你已经把上一篇已经看过。**

###本地依赖###
gradle 作为构建工具，能够很方便的使用本地jar包，以下为使用的代码块。

``` groovy localDependics
dependencies {
	//单文件依赖
	compile files('libs/android-support-v4.jar')
	//某个文件夹下面全部依赖
	compile fileTree(dir: 'libs', include: '*.jar')
}

android {
	
}
```


###远程依赖###
gradle 同时支持maven，ivy，由于ivy我没用过，所以用maven 作为例子，以下为代码块：
``` groovy remoteDependics
repositories {
	//从中央库里面获取依赖
	mavenCentral()
	//或者使用指定的本地maven 库
	maven{
		url "file://F:/githubrepo/releases"
	}
	//或者使用指定的远程maven库
	maven{
		url "https://github.com/youxiachai/youxiachai-mvn-repo/raw/master/releases"
	}
}

dependencies {
	//应用格式: packageName:artifactId:version
	compile 'com.google.android:support-v4:r13'
}

android {

}
```

###android library 依赖###
对于项目依赖 android library的话，就不是依赖一个jar，那么简单了，在这里需要使用gradle  mulit project 机制。
例子的话，我就不重复写了，具体参考上一篇提到的德国人写的例子。**记得把插件版本改为 0.4**
[https://github.com/Goddchen/Android-Gradle-Examples/tree/master/Gradle%20Library%20Projects 
](https://github.com/Goddchen/Android-Gradle-Examples/tree/master/Gradle%20Library%20Projects)

**注意对于android library `build.gradle` 记得要把**
> apply plugin: 'android' 改为 apply plugin: 'android-library'

####Mulit project 设置####
Mulit project 设置是gradle 约定的一种格式，如果你需要编译某个项目之前，要先编译另外一个项目的时候，就需要用到，结构如下图（来自于官方文档）：
<blockquote style="margin:0px 0px 0px 40px;border:none;padding:0px"><div><span style="line-height:1.6;font-size:10pt;font-style:normal"><font face="courier new, monospace">MyProject/</font></span></div><div><font face="courier new, monospace" style="font-style:normal">&nbsp;| settings.gradle</font></div><div><font face="courier new, monospace" style="font-style:normal">&nbsp;+ app/</font></div><div><font face="courier new, monospace" style="font-style:normal">&nbsp; &nbsp; | build.gradle</font></div><div><font face="courier new, monospace" style="font-style:normal">&nbsp;+ libraries/</font></div><div><font face="courier new, monospace" style="font-style:normal">&nbsp; &nbsp; + lib1/</font></div><div><font face="courier new, monospace" style="font-style:normal">&nbsp; &nbsp; &nbsp; &nbsp;| build.gradle</font></div><div><font face="courier new, monospace" style="font-style:normal">&nbsp; &nbsp; + lib2/</font></div><div><font face="courier new, monospace" style="font-style:normal">&nbsp; &nbsp; &nbsp; &nbsp;| build.gradle</font></div></blockquote>

你需要在你的workplace 目录下面创建settings.gradle 的文件，然后在里面写上：
> include ':app', ':libraries:lib1', ':libraries:lib2'

那样，gradle mutil project 就设置完毕。

对于app project 如果需要应用libraries 目录下的 lib1 ，你只需要在app project `build.gradle` 里面的依赖中这么写：

``` groovy android-library
compile project(':libraries:lib1')
```

即可完成，写完以后可以用`gradle AndroidDependencies` 来检查依赖状况。



###需要注意的地方###
``` python notice
buildscript {
    repositories {
	    mavenCentral()
	}
    
    dependencies {
        classpath 'com.android.tools.build:gradle:0.4'
    }
}
```

对于`buildscript{}` 在android gradle是用来预置插件环境，一般不建议把依赖写着里面，推荐的依赖写法是：
``` python notice
buildscript {
    repositories {
	    mavenCentral()
	}
    
    dependencies {
        classpath 'com.android.tools.build:gradle:0.4'
    }
}

repositories {
	//从中央库里面获取依赖
	mavenCentral()
	//或者使用指定的本地maven 库
	maven{
		url "file://F:/githubrepo/releases"
	}
}

dependencies {
	//应用格式: packageName:artifactId:version
	compile 'com.google.android:support-v4:r13'
}
```


##使用Maven 管理库##
gradle 对于包的管理，支持filesystem，maven，ivy，这里我重点说说如何利用maven 进行android 依赖包的管理


###利用Gradle 发布本地maven 库###
对于如何打包一个jar 包并且发布到maven，这是java 的基本知识，这里就不说了。

我们现在要学习的是，例如发布一个android library 包。

在过去，android library并没有一个很好的包管理方式，简单来说，在gradle出现以前，官方并没有一种用于管理android library 依赖包的方式，一般我们都是直接下载别人的android library project 源码进行集成，而对于第三方的android-maven-plugin 用的是apklib 格式。

而现在，官方终于推出一种android library的打包格式，扩展名为`*.aar`。前面提到，目前android gradle插件并不支持本地直接使用`*.aar`文件，不过，支持包管理库的引用方式，下面，我为大家说一下，怎么对android library 发布使用。

1. 打包android library 
> 对android library 进行打包直接在library项目下面使用`gradle build` 即可，然后，你就会在 build/libs 目录下看到两个`*.aar`文件，一个debug包用的，一个是release 下用的，看个人需求使用，这里我们用的是release 版本的 .aar 文件。

2. 发布脚本
> android library project 目录的  build/libs 下创建一个build.gradle 文件
``` groovy publish
apply plugin: 'maven'

group = 'com.youxiachai'

artifacts {
	//当前aar 文件名
	archives file('Gradlelib.aar')
}

uploadArchives {
	  repositories {
		mavenDeployer {
			repository(url: "file://F:/githubrepo/releases")
			pom.version  = 'r1'
			pom.artifactId = 'gradletest'
		}
	}
}
``` `gradle uploadArchives`即可完成包的发布。

完成以上两步就可以直接用maven 引用jar的依赖那样，引用android library 的依赖。


##扩展阅读##

[Google I/O 2013 上面介绍的android Gralde build System‎ （已经转载到优酷）](http://v.youku.com/v_show/id_XNTYwMzY0NDYw.html)

[Xavier Ducrohet](https://plus.google.com/+XavierDucrohet/posts) Android SDK Tech Lead,上面那个视频就是这个人演讲的。



[adt-dev社区](https://groups.google.com/forum/#!topic/adt-dev/) 因为用gradle 构建android 是新系统，一般而言有问题是搜索不到的，有问题还是上社区直接问吧，一般[Xavier Ducrohet](https://plus.google.com/+XavierDucrohet/posts) 都会帮你解决。
