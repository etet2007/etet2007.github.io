---
layout:     post
title:      "Android Plugin for Gradle & Gradle"
subtitle:   "关于Android Plugin for Gradle & Gradle ，如何解决开启项目卡死"
date:       2017-11-01
author:     "StephenLau"
header-img: "imgpost-bg-android.jpg"
tags:
- Android
- Gradle
- Android Studio
---

# 关于Android Plugin for Gradle & Gradle ，如何解决开启项目卡死？

[Gradle](https://gradle.org/)是一个基于Apache Ant和Apache Maven概念的项目自动化构建工具。它使用一种基于Groovy的语言来声明项目设置，抛弃了基于XML的各种繁琐配置。Gradle和Android Studio其实没有必然的关系，是Android Studio通过Plugin for Gradle 最终使用了Gradle。

## 1. Gradle版本
可以在File > Project Structure > Project menu中设置。
还可以在项目中的以下文件中设置。
> {your project}/gradle/wrapper/gradle-wrapper.properties 
> 文件内容如下：

```groovy
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl = https\://services.gradle.org/distributions/gradle-4.1-all.zip
```
distributionUrl 决定了这个项目使用的gradle版本。

Android Studio打开一个工程时，首先会读取`gradle-wrapper.properties` 文件，从而知道这个工程需要哪个版本的gradle，然后就会去保存gradle的文件夹`GRADLE_USER_HOME` 去找看存不存在这个版本的gradle，不存在则会去`distributionUrl`去下载。
### Gradle不同操作系统下的保存位置
启动项目后，会从网络下载Gradle，保存在下面的目录： 
Linux:
> ~/.gradle/wrapper/dists

Windows:
> C:\users\{user name}\.gradle\wrapper\dists

Mac:
> ~/.gradle/wrapper/dists/

## 2. Plugin for Gradle版本

Gradle插件和Gradle是两个独立的东西，gradle插件版本是由项目最外层的build.gradle文件决定的。
app/build.gradle:
```groovy
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.0.0'

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}
```
在`classpath 'com.android.tools.build:gradle:3.0.0'`这个语句中设置，这个版本是和Android Studio的版本一致的，如3.0.0是对应Android Studio 3.0的。


## 3. 解决卡死的方法。

对应不同版本的Gradle，需要使用对应的Plugin for gradle. 
| Plugin version | Required Gradle version |
| -------------- | ----------------------- |
| 1.0.0 - 1.1.3  | 2.2.1 - 2.3             |
| 1.2.0 - 1.3.1  | 2.2.1 - 2.9             |
| 1.5.0          | 2.2.1 - 2.13            |
| 2.0.0 - 2.1.2  | 2.10 - 2.13             |
| 2.1.3 - 2.2.3  | 2.14.1+                 |
| 2.3.0+         | 3.3+                    |
| 3.0.0+         | 4.1+                    |

1. 修改项目中gradle、Plugin for Gradle版本。  
2. 打开项目 
  这个时候Android Studio将自动下载gradle。这时直接退出Android Studio，
3. 手动下载gradle
 > Gradle下载地址：<https://services.gradle.org/distributions/>

   从上面的地址下载对应版本的gradle，我使用迅雷下速度挺快的。将下载的.zip复制到Gradle保存位置相应版本文件夹中一串乱码的文件夹下，注意不用解压。
4. 重新开启Android Studio。
  Sync一下就可以了。


## 参考文章：

官方配置构建：<https://developer.android.com/studio/build/index.html?hl=zh-cn>

主要参考：<http://blog.csdn.net/fuchaosz/article/details/51567808>

Gradle全面的介绍，介绍得不错。：http://www.jianshu.com/p/9df3c3b6067a 

## 问题

/Applications/Android Studio.app/Contents/gradle是什么东东？！