---
title: Gradle 构建 android 应用常见问题解决指南
tags: android
categories:
  - android
date: 2013-09-30 00:00:00
---
## 前言
android gradle 插件已经发展到0.5.7,同时gradle 本身也到了1.8,相比两个月前,android gradle 更快,更完善,也更好用了,为了让各位androider 早日用上gradle这样的神器,特地写一篇关于gradle一些奇葩错误的解决指南.
<!--more-->

## 使用最新的gradle android插件
以前我们写的时候会这么写

```groovy
dependencies {
	classpath 'com.android.tools.build:gradle:0.5.0'
}
```

不过,由于android gradle 插件的开发还是很活跃的,而且目前而言,可能还存在一些我们不知道的坑,但是,别人踩过,后边,官方修复,为了不踩坑,我建议android gradle 始终保持最新版本,写法如下:

```groovy
dependencies {
	classpath 'com.android.tools.build:gradle:0.5+'
}
```

## 由于代码编码与编译环境编码不一致,导致构建失败

有时候,我们的代码使用utf-8 保存的,但是,进行gradle build 的环境是gbk这类的,这时候会包如下错误:

> 15: 错误: 编码GBK的不可映射字符
         * 鍑虹幇涓枃璇锋敞鎰?

这个时候我们就需要手动的设置编译时编码类型.

```groovy
tasks.withType(Compile) {
    options.encoding = "UTF-8"
}
apply plugin: 'android'
android {}
```

## android support v4 重复引用问题

```java
UNEXPECTED TOP-LEVEL EXCEPTION:
java.lang.IllegalArgumentException: already added: Landroid/support/v4/app/Activ
ityCompatHoneycomb;
        at com.android.dx.dex.file.ClassDefsSection.add(ClassDefsSection.java:12
3)
        at com.android.dx.dex.file.DexFile.add(DexFile.java:163)
        at com.android.dx.command.dexer.Main.processClass(Main.java:490)
        at com.android.dx.command.dexer.Main.processFileBytes(Main.java:459)
        at com.android.dx.command.dexer.Main.access$400(Main.java:67)
        at com.android.dx.command.dexer.Main$1.processFileBytes(Main.java:398)
        at com.android.dx.cf.direct.ClassPathOpener.processArchive(ClassPathOpen
er.java:245)
```

出现这个问题的原因一般是由于我们这样的写法导致:

>```groovy
dependencies {
	compile fileTree(dir: 'libs', include: '*.jar')
}
```

某个相同的jar包,被复制到了build目录导致重复编译使编译时失败,

由于这个问题android support v4 出现的比较多,所以同类型的都归类为v4 问题吧.

要避免这个问题,我们尽量少使用依赖某个目录下所有包,毕竟android项目不想java web项目动不动就有好几十jar 包依赖.要修复这个v4,原理很简单,可以使用依赖maven的写法.

>```groovy
dependencies {
    compile 'com.android.support:support-v4:13.0.0'
}
```

## 打包后缺少*.so文件

用指定依赖包的方式打包,我们会发现,最终打包后的jar没有了*.so文件,这个时候,我们需要自定义一个tasks,写如下:

```groovy
task copyNativeLibs(type: Copy) {
    from(new File('libs')) { include '**/*.so' }
    into new File(buildDir, 'native-libs')
}

tasks.withType(Compile) { compileTask -> compileTask.dependsOn copyNativeLibs }

clean.dependsOn 'cleanCopyNativeLibs'

tasks.withType(com.android.build.gradle.tasks.PackageApplication) { pkgTask ->
    pkgTask.jniDir new File(buildDir, 'native-libs')
}
```

这样,在编译时,就会自动把libs目录下的`**/*.so` 文件复制到apk里面了.

## 构建多渠道包

在最新版本的gradle 0.5.7 中,构建多渠道包比之前简单多了,在以前,你需要这么写:

```groovy
android {
    buildTypes {
     	hiapk {
     		packageNameSuffix ".hiapk"
     	}
     	playstore {
     		packageNameSuffix ".playstore"
    	}
     }
	sourceSets {
		hiapk {
			manifest.srcFile 'hiapk/AndroidManifest.xml'
		}
		playstore {
			manifest.srcFile 'hiapk/AndroidManifest.xml'
		}
	}
}
```

要替换某个类型的文件需要自己手动写,渠道多了,这代码量是可想而知的多,在0.5.7中,进行了一个约定规则,构建,渠道包你只需
```groovy
android {
    buildTypes {
     	hiapk {
     		packageNameSuffix ".hiapk"
     	}
     	playstore {
     		packageNameSuffix ".playstore"
    	}
     }
	sourceSets {
		 hiapk.setRoot('build-types/hiapk')
         playstore.setRoot('build-types/playstore')
	}
}
```

在项目的根目录下创建一个`build-types`的目录,在创建对应渠道的子目录,然后把一些,诸如要替换`AndroidManifest.xml`,里面友盟渠道号什么的,直接把xml复制进去就行,gradle在构建项目的时候,会自动的优先使用`build-types`下目录文件的目录,诸如,根据不同渠道,不同国家换个程序图标什么的,都只要放到目录下即可.








