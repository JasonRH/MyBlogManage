---
title: Android Studio 配置签名和编译自命名
categories: 前端
tags:
  - Android
abbrlink: 59951
date: 2019-09-07 09:16:00
---

```

android {

def releaseTime() {
    return new Date().format("yyMMdd", TimeZone.getTimeZone("UTC"))
}

signingConfigs {
        myConfig {
            storeFile file(RELEASE_STORE_FILE)
            storePassword RELEASE_KEY_PASSWORD
            keyAlias RELEASE_KEY_ALIAS
            keyPassword RELEASE_STORE_PASSWORD
        }
    }

 buildTypes {
        release {
            signingConfig signingConfigs.myConfig
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            //混淆
            minifyEnabled true
            //Zipalign优化
            zipAlignEnabled true
            // 移除无用的resource文件
            shrinkResources true
        }
        debug {
            signingConfig signingConfigs.myConfig
            applicationIdSuffix '.debug'
            versionNameSuffix 'debug'
            //混淆
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
            //Zipalign优化
            zipAlignEnabled false
            // 移除无用的resource文件
            shrinkResources false
        }
        android.applicationVariants.all { variant ->
            variant.outputs.all {
                def enName = '阅读'
                outputFileName = "${enName}_V${defaultConfig.versionName}_${releaseTime()}${name}.apk"
            }
        }

    }

    
    
}
···

#注意：signingConfigs 要放在 buildTypes 前面
```

```
#gradle.properties文件中添加jks信息

RELEASE_STORE_FILE=../release.jks
RELEASE_STORE_PASSWORD=******
RELEASE_KEY_ALIAS=**
RELEASE_KEY_PASSWORD=******

```

```
#根据RELEASE_STORE_FILE 配置的路径，将release.jks 文件放到项目根目录下
```