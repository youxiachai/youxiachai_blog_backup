title: 用Gradle 构建你的android程序
date: 2013-05-19
tags: android
---
##前言##
android gradle 的插件终于把混淆代码的task集成进去了，加上最近，android studio 用的是gradle 来构建项目， 下定决心把android gralde 构建项目的用户指南全部看完， 让不会用gradle 的人也用gradle构建android项目，让打包（注意，打包和构建是两码事）多版本android不再痛苦。最后，题外话：珍惜生命，远离ant....
<!--more-->

##Gradle build android 历史##
[Android Tools 主页](http://tools.android.com) ，大概是今年2月份发布 adt21.1 的时候，忽然在主页发现了[New Build System](http://tools.android.com/tech-docs/new-build-system) 原来是可以用gradle 来构建android项目，至于[gradle](http://en.wikipedia.org/wiki/Gradle)是什么（既然点击进来看了应该都知道了吧。） 。然后，又看了一下[RoadMap](http://tools.android.com/tech-docs/new-build-system/roadmap) 那时候，还并不支持Proguard 打包，于是就没看了。。。

最近，android studio 发布，终于gradle 0.4 也跟着出来了，于是，先把gradle 学了一遍，然后把[Gradle Plugin User Guide](http://tools.android.com/tech-docs/new-build-system/user-guide)也认真阅读了一下，根据我的个人体验，如果你对gradle 毫无了解就去看[Gradle Plugin User Guide](http://tools.android.com/tech-docs/new-build-system/user-guide) 可能很多地方都一头雾水，但是并不妨碍你用gradle 打包android 应用，只是，出现问题，你就可能很头疼。不过，本篇博文就是让不会gradle 也能用上 gradle 打包android 程序，因为，我也不懂gradle，所以，我把我碰到的问题的解决方案都一一列出。

顺便贴上官方为什么使用gradle 的理由

* Domain Specific Language (DSL) to describe and manipulate the build logic
* Build files are Groovy based and allow mixing of declarative elements through the DSL and using code to manipulate the DSL elements to provide custom logic.
* Built-in dependency management through Maven and/or Ivy.
* Very flexible. Allows using best practices but doesn’t force its own way of doing things.
* Plugins can expose their own DSL and their own API for build files to use.
* Good Tooling API allowing IDE integration

##Gradle 基本概念##
首先我们学习几个gradle 的脚本语法，掌握了这几个语法，你就能非常简单的用gradle构建打包android项目了。
首先，我们来看下一个最简单android `build.gradle`

{% codeblock build.gradle lang:groovy %}
	
buildscript {
       
	 repositories {
            mavenCentral()
        }

        dependencies {
            classpath 'com.android.tools.build:gradle:0.4'
        }
    }

    apply plugin: 'android'

    android {
        compileSdkVersion 17
    }
{% endcodeblock %}

**英语的介绍都来自与 gradle官方文档， 主要后边的中文不是翻译，是补充介绍。。**

`buildscript{}`
> Configures the build script classpath for this project.
说白了就是设置脚本的运行环境


`repositories{}`
> Returns a handler to create repositories which are used for retrieving dependencies and uploading artifacts produced by the project. 大意就是支持java 依赖库管理（maven/ivy）,用于项目的依赖。这也是gradle 强力的地方。。。 


`dependencies{}`
> The dependency handler of this project. The returned dependency handler instance can be used for adding new dependencies. For accessing already declared dependencies, the configurations can be used.  依赖包的定义。支持maven/ivy，远程，本地库，也支持单文件，如果前面定义了`repositories{}`maven 库，使用maven的依赖（我没接触过ivy。。）的时候只需要按照用类似于`com.android.tools.build:gradle:0.4`，gradle 就会自动的往远程库下载相应的依赖。

`apply plugin:`
> 声明构建的项目类型，这里当然是android了。。。

`android{}`
> 设置编译android项目的参数，接下来，我们的构建android项目的所有配置都在这里完成。

##构建一个Gradle android项目##
首先，你要安装[Gradle 1.6](http://www.gradle.org/downloads) 并且，写进系统的环境变量里面，所有的命令都是默认你已经配好了gradle 的环境。而且，已经已经升级了android sdk 22

要用gradle构建你的有两种方式：（**build.gradle 放到项目目录下**）

1. 利用adt 22导出 build.gradle.
2. 复制别人写好的build.gradle 文件.
3. 根据gradle 规则，手写android 的build.gradle 文件。

个人推荐1,2 方法。。。。

一个android build.gradle 最基本基本文件
{% codeblock build.gradle lang:groovy %}
buildscript {

    repositories {
        mavenCentral()
    }

    dependencies {
        classpath 'com.android.tools.build:gradle:0.4'
    }
}

apply plugin: 'android'

dependencies {
}

android {

    compileSdkVersion 17
    buildToolsVersion "17"

    defaultConfig {
        minSdkVersion 8
        targetSdkVersion 17
    }
    sourceSets {
        main {
            manifest.srcFile 'AndroidManifest.xml'
            java.srcDirs = ['src']
            resources.srcDirs = ['src']
            aidl.srcDirs = ['src']
            renderscript.srcDirs = ['src']
            res.srcDirs = ['res']
            assets.srcDirs = ['assets']
        }

        instrumentTest.setRoot('tests')
    }
}
{% endcodeblock %}

接着在命令行cd 到项目目录下
> 例如: cd e:\workplace\andoridGradle

如果你是第一次使用gradle 构建android项目建议你先使用`gradle clean` 把android gradle 插件，还有相关依赖包下载下来并且对环境进行初始化，如果出错了，一般可能是下载超时，试多几次即可，最后你会看到如下提示：`BUILD SUCCESSFUL`

The TaskContainer.add() method has been deprecated and is scheduled to be remove
d in Gradle 2.0. Please use the create() method instead.

:clean UP-TO-DATE

BUILD SUCCESSFUL

Total time: 7.847 secs

完成以上的步骤，就可以正式使用gralde 构建你的android项目了。

然后使用`gradle build` 就完成了android 项目的构建了。如果，你是照着以上步骤走的话，你将会想项目目录里面看到一个build 的目录，里面就是用gradle 构建android项目的全部例如了，结构目录看附录。

最终打包的apk 就在build/apk 目录下了。然后，你会发现，两个apk 一个是
* [项目名]-debug-unaligned
* [项目名]-release-unsigned

如果以上内容你都掌握的话，接下来就将详细说说如何利用gralde 打包android apk。
##Gralde 打包参数详解##
上面说了一大堆东西，其实并不吸引人去使用gradle，如果只是构建项目的话，adt不是更合适吗？如果，你看完以下内容还是这么觉得的话，你就没必要折腾gradle了。。。。。。


###打签名包###

看附录 默认输出 *release* apk 是没有签名的，那么我们需要签名的很简单，只需要在android{}里面补充加上加上即可。完整[build.gradle 请点击我的gist](https://gist.github.com/youxiachai/5608223)
{% codeblock build.gradle lang:groovy %}
signingConfigs {
   myConfig{
     storeFile file("gradle.keystore")
    	storePassword "gradle"
    	keyAlias "gradle"
    	keyPassword "gradle"
    }
}
    
   buildTypes{
     release {
    	signingConfig  signingConfigs.myConfig
     } 
   }
 
{% endcodeblock  %}
然后，运行`gradle clean` `gradle build` ,这次在build/apk  你看到了多了一个[项目名]-release-unaligned， 从字面上面我就可以知道，这个只是没有进行zipAlign 优化的版本而已。而[项目名]-release 就是我们签名，并且zipAlign 的apk包了.

###打混淆包###

只需要在原来的基础上加上，完整的[proguad.gradle 代码](https://gist.github.com/youxiachai/5608223)
{% codeblock build.gradle lang:groovy %}
buildTypes{
   release {
   signingConfig  signingConfigs.myConfig
     runProguard true
     proguardFile 'proguard-android.txt'
   }
}

{% endcodeblock %}

`gradle clean`

`gradle build`

###打多渠道包(Product Flavor)###

现在来解释一下上一节的问题，**apk目录下的两个apk 的含义**

**为什么产生了两个apk？**

默认的android gralde 插件定义了两种apk 的类型**debug**, **release**，这两种类型的详细对比看附录。

这个是android gralde 插件 `buildTypes{}` 方法产生的，默认配置好了两个默认模板，当然你也可以修改，前面我们就是在修改默认的release 的配置，让输出release类型的的apk，具有签名和混淆。

对于多渠道包，android 插件提供了一个名为`Product Flavor{}` 的配置，用于进行多渠道打包。

例如，我的android应用有海外版，和国内版本，而且这两个版本的包名是不一样的！！（我就举两个市场的例子安装这个思路，你要打包100个不同的市场只是几行代码的事情。）。



你只需要在`android{}` 补充上
{% codeblock build.gradle lang:groovy %}
productFlavors {
	playstore {
			packageName='com.youxiachai.androidgradle.playstore'
	}
	hiapk {
			packageName='com.youxiachai.androidgradle.amazonappstore'
	}
}
{% endcodeblock %}   

然后`gradle clean`,`gradle build`,在build/apk 下面你会看到一堆的包，命名格式[项目名]-[渠道名]-release

**仅此而已?**

`Product Flavor{}` 不只是能改包名那么简单，还能够对编译的源码目录进行切换。

什么意思? 不知道各位有没有用过友盟做用户统计，如果，你用的是分发渠道分析，你需要修改AndroidManifest.xml 添加上
` <meta-data android:value="hiapk" android:name="UMENG_CHANNEL"/>`

如果，你很多渠道，，然后你就会很痛苦，现在用gradle 就非常舒服，你只需要在`android.sourceSets`指定我们的渠道名就行，android  gradle 插件，会自动打包！！！例如
{% codeblock build.gradle lang:groovy %}
sourceSets {
    main {
        manifest.srcFile 'AndroidManifest.xml'
        java.srcDirs = ['src']
        resources.srcDirs = ['src']
        aidl.srcDirs = ['src']
        renderscript.srcDirs = ['src']
        res.srcDirs = ['res']
        assets.srcDirs = ['assets']
    }
        
    hiapk {
      	manifest.srcFile 'hiapk/AndroidManifest.xml'
    }    	
       	playstore {
       		manifest.srcFile 'hiapk/AndroidManifest.xml'
    }
       
	instrumentTest.setRoot('tests')
        
}
{% endcodeblock  %}
然后运行`gradle clean`,`gradle build`,省下的时间去喝杯咖啡，睡个觉什么的都好。。。


###外部依赖###
android gradle 对于外部jar 包的应用支持maven/ivy 管理的包，也支持指定具体文件，前面已经在上文说过。上面演示的完整 build.gradle gist 里面也有写。你需要加上如下代码即可：
{% codeblock build.gradle lang:groovy %}
dependencies {
	compile files('libs/android-support-v4.jar')
}
{% endcodeblock %} 

##结语##

至此，对于用android gradle 构建android应用程序，打包android 程序，所需要的所有知识，在以上已经说明，只要你是认真看上面文章的，对于，如何打依赖于android library project 的包，可以看附录提供的那个德国人写的例子，而对于`build.gradle` 里面的代码你需要把`0.2`, 改为`0.4`即可。至于用gradle 运行android test case部分的教程，个人感觉写了也白写（我写过关于andorid 测试相关的文章，也录制过视频，所以有这个感觉。），估计不会有人关注，所以，如果你对用gradle 进行android test的话，可以看附录里面提供的官方gradle手册。



##扩展阅读##

 对于这部分内容，你读与不读，并不影响你使用gradle 打包android 项目。至于读了的好处就是你能够更好的使用gradle。。

* 完整的[Gradle Plugin User Guide](http://tools.android.com/tech-docs/new-build-system/user-guide) 其中里面有个错误是`compile files('libs/android-support-v4.jar')` 不是`compile file('libs/android-support-v4.jar')` 教程是基于android gradle0.3 ，在0.4中只是多了**混淆打包，这块已经在文中补充了。**

* 一个德国人写的[Android-Gradle-Examples](https://github.com/Goddchen/Android-Gradle-Examples)

* [`dependencies{}`](http://www.gradle.org/docs/current/userguide/artifact_dependencies_tutorial.html) 更多的介绍。

* **debug**, **release**,这两种类型的默认配置如下：
<table cellspacing="0" bordercolor="#888" border="1" style="border-collapse:collapse;border-color:rgb(136,136,136);border-width:1px"><tbody><tr><td style="width:198px;height:19px">&nbsp;Property name</td><td style="width:195px;height:19px">&nbsp;Default values for debug</td><td style="width:241px;height:19px">&nbsp;Default values for release / other</td></tr><tr><td style="width:198px;height:20px">&nbsp;<font face="courier new, monospace" color="#38761d"><b>debuggable</b></font></td><td style="width:195px;height:20px">&nbsp;true</td><td style="width:241px;height:20px">&nbsp;false</td></tr><tr><td style="width:198px;height:20px">&nbsp;<font face="courier new, monospace" color="#38761d"><b>jniDebugBuild</b></font></td><td style="width:195px;height:20px">&nbsp;false</td><td style="width:241px;height:20px">&nbsp;false</td></tr><tr><td style="width:198px;height:19px">&nbsp;<b><font face="courier new, monospace" color="#38761d">renderscriptDebugBuild</font></b></td><td style="width:195px;height:19px">&nbsp;false</td><td style="width:241px;height:19px">&nbsp;false</td></tr><tr><td style="width:198px;height:20px">&nbsp;<b><font face="courier new, monospace" color="#38761d">renderscriptOptimLevel</font></b></td><td style="width:195px;height:20px">&nbsp;3</td><td style="width:241px;height:20px">&nbsp;3</td></tr><tr><td style="width:198px;height:20px">&nbsp;<b><font face="courier new, monospace" color="#38761d">packageNameSuffix</font></b></td><td style="width:195px;height:20px">&nbsp;null</td><td style="width:241px;height:20px">&nbsp;null</td></tr><tr><td style="width:198px;height:20px">&nbsp;<b><font face="courier new, monospace" color="#38761d">versionNameSuffix</font></b></td><td style="width:195px;height:20px">&nbsp;null</td><td style="width:241px;height:20px">&nbsp;null</td></tr><tr><td style="width:198px;height:20px">&nbsp;<b><font face="courier new, monospace" color="#38761d">signingConfig</font></b></td><td style="width:195px;height:20px">&nbsp;android.signingConfigs.debug</td><td style="width:241px;height:20px">&nbsp;null</td></tr><tr><td style="width:198px;height:20px">&nbsp;<b><font face="courier new, monospace" color="#38761d">zipAlign</font></b></td><td style="width:195px;height:20px">&nbsp;false</td><td style="width:241px;height:20px">&nbsp;true</td></tr></tbody></table>


* **defaultConfig {}** 配置参数列表
<table cellspacing="0" bordercolor="#888" border="1" style="border-collapse:collapse;border-color:rgb(136,136,136);border-width:1px"><tbody><tr><td style="width:208px;height:19px">&nbsp;Property Name</td><td style="width:168px;height:19px">&nbsp;Default value in DSL object</td><td style="width:251px;height:19px">&nbsp;Default value</td></tr><tr><td style="width:208px;height:20px">&nbsp;<font face="courier new, monospace" color="#38761d"><b>versionCode</b></font></td><td style="width:168px;height:20px">&nbsp;-1</td><td style="width:251px;height:20px">&nbsp;value from manifest if present</td></tr><tr><td style="width:208px;height:20px">&nbsp;<font face="courier new, monospace" color="#38761d"><b>versionName</b></font></td><td style="width:168px;height:20px">&nbsp;null</td><td style="width:251px;height:20px">&nbsp;value from manifest if present</td></tr><tr><td style="width:208px;height:20px">&nbsp;<font face="courier new, monospace" color="#38761d"><b>minSdkVersion</b></font></td><td style="width:168px;height:20px">&nbsp;-1</td><td style="width:251px;height:20px">&nbsp;value from manifest if present</td></tr><tr><td style="width:208px;height:20px">&nbsp;<font face="courier new, monospace" color="#38761d"><b>targetSdkVersion</b></font></td><td style="width:168px;height:20px">&nbsp;-1</td><td style="width:251px;height:20px">&nbsp;value from manifest if present</td></tr><tr><td style="width:208px;height:20px">&nbsp;<font face="courier new, monospace" color="#38761d"><b>packageName</b></font></td><td style="width:168px;height:20px">&nbsp;null</td><td style="width:251px;height:20px">&nbsp;value from manifest if present</td></tr><tr><td style="width:208px;height:20px">&nbsp;<font face="courier new, monospace" color="#38761d"><b>testPackageName</b></font></td><td style="width:168px;height:20px">&nbsp;null</td><td style="width:251px;height:20px">&nbsp;app package name + “.test”</td></tr><tr><td style="width:208px;height:20px">&nbsp;<font face="courier new, monospace" color="#38761d"><b>testInstrumentationRunner</b></font></td><td style="width:168px;height:20px">&nbsp;null</td><td style="width:251px;height:20px">&nbsp;android.test.InstrumentationTestRunner</td></tr><tr><td style="width:208px;height:19px">&nbsp;<font face="courier new, monospace" color="#38761d"><b>signingConfig</b></font></td><td style="width:168px;height:19px">&nbsp;null</td><td style="width:251px;height:19px">&nbsp;null</td></tr>
<tr><td style="width:208px;height:20px">&nbsp;<font face="courier new, monospace" color="#38761d"><b>runProguard</b></font></td><td style="width:168px;height:20px">&nbsp;false</td><td style="width:251px;height:20px">&nbsp;false</td></tr>
<tr><td style="width:208px;height:19px">&nbsp;<font face="courier new, monospace" color="#38761d"><b>proguardFile</b></font></td><td style="width:168px;height:19px">&nbsp; 'proguard-android.txt' or 'proguard-android-optimize.txt'</td><td style="width:251px;height:19px">&nbsp; 'proguard-android.txt' or 'proguard-android-optimize.txt'</td></tr>
</tbody></table>

* build 结构目录
{% codeblock tree lang:shell %}
build/
├── apk
├── assets
│   ├── debug
│   └── release
├── classes
│   ├── debug
│   │   └── com
│   │       └── example
│   │           └── gradle
│   └── release
│       └── com
│           └── example
│               └── gradle
├── dependency-cache
│   ├── debug
│   └── release
├── incremental
│   ├── aidl
│   │   ├── debug
│   │   └── release
│   ├── dex
│   │   ├── debug
│   │   └── release
│   ├── mergeAssets
│   │   ├── debug
│   │   └── release
│   └── mergeResources
│       ├── debug
│       └── release
├── libs
├── manifests
│   ├── debug
│   └── release
├── res
│   ├── all
│   │   ├── debug
│   │   │   ├── drawable-hdpi
│   │   │   ├── drawable-mdpi
│   │   │   ├── drawable-xhdpi
│   │   │   ├── drawable-xxhdpi
│   │   │   ├── layout
│   │   │   ├── menu
│   │   │   ├── values
│   │   │   ├── values-sw720dp-land
│   │   │   ├── values-v11
│   │   │   └── values-v14
│   │   └── release
│   │       ├── drawable-hdpi
│   │       ├── drawable-mdpi
│   │       ├── drawable-xhdpi
│   │       ├── drawable-xxhdpi
│   │       ├── layout
│   │       ├── menu
│   │       ├── values
│   │       ├── values-sw720dp-land
│   │       ├── values-v11
│   │       └── values-v14
│   └── rs
│       ├── debug
│       └── release
├── source
│   ├── aidl
│   │   ├── debug
│   │   └── release
│   ├── buildConfig
│   │   ├── debug
│   │   │   └── com
│   │   │       └── example
│   │   │           └── gradle
│   │   └── release
│   │       └── com
│   │           └── example
│   │               └── gradle
│   ├── r
│   │   ├── debug
│   │   │   └── com
│   │   │       └── example
│   │   │           └── gradle
│   │   └── release
│   │       └── com
│   │           └── example
│   │               └── gradle
│   └── rs
│       ├── debug
│       └── release
└── symbols
    ├── debug
    └── release
88 directories
{% endcodeblock %}


##最后的吐槽##
吐槽一下。。。用ant脚本的（也许你没有接触过。。）。在以前你用ant 脚本打包apk的时候需要打包不同包名，你需要用ant 读取`AndroidManifest.xml` 然后又正则匹配替换里面packagename 参数。。虽然描述得过程很简单，你真去写的时候你就蛋疼了（对于一个ant外行人来说，个人感觉ant的学习曲线太陡峭了，如果是两年前的我，可能还写得出这样的ant脚本（当年费了很大的功夫学习了一个多星期），不过，因为很少用到（后来知道maven了。。果断放弃了ant，为什么不在android使用maven？ 因为，android 的maven 插件式非官方的，而且现在看来maven 的xml实在很复杂，看起来就头疼））*
