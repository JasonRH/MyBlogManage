---
title: Moudle library中添加aar
categories: 移动端
tags:
  - Android
abbrlink: 21449
date: 2021-03-13 14:29:00
---


1.所在模块的build.gradle文件中
```
android {
    repositories {
        flatDir {
            dirs 'libs'
        }
    }

}

dependencies {
    implementation(name: 'lfilepickerlibrary-release', ext: 'aar')
}
```

2.多个模块引用时，项目的根build.gradle中统一添加
```
allprojects {
    repositories {
        google()
        jcenter()
        maven { url "https://jitpack.io" }
        flatDir {
            dirs '../dr_home/libs'
        }
    }
}
```



