---
title: 'Android生成aar包时,引用的其它aar无法打包问题'
categories: 前端
tags:
  - Android
abbrlink: 27803
date: 2021-03-03 16:50:00
---


Add snippet below to your root build script file:
```
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.kezong:fat-aar:1.3.4'
    }
}
```
Add snippet below to the build.gradle of your android library:
```
apply plugin: 'com.kezong.fat-aar'
```
Step 2: Embed dependencies
Declare embed for the dependencies you want to merge in build.gradle.

The usage is similar to implementation, like this:
```
dependencies {
    implementation fileTree(dir: 'libs', include: '*.jar')
    // java dependency
    embed project(':lib-java')
    // aar dependency
    embed project(':lib-aar')
    // aar dependency
    embed project(':lib-aar2')
    // local full aar dependency, just build in flavor1
    flavor1Embed project(':lib-aar-local')
    // local full aar dependency, just build in debug
    debugEmbed (name:'lib-aar-local2', ext:'aar')
    // remote jar dependency
    embed 'com.google.guava:guava:20.0'
    // remote aar dependency
    embed 'com.facebook.fresco:fresco:1.11.0'
    // don't want to embed in
    implementation('androidx.appcompat:appcompat:1.2.0')
}
```

地址：https://github.com/kezong/fat-aar-android