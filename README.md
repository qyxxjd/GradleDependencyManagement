## Gradle依赖统一管理

项目根目录新建config.gradle文件，配置参考如下：
```gradle
def supportVersion = "23.1.1"
def rxBindingVersion = "0.4.0"
def greenDAOVersion = "2.1.0"

ext {

    android = [compileSdkVersion: 23,
               buildToolsVersion: "23.0.2",
               applicationId    : "com.classic.gradleconfig",
               minSdkVersion    : 14,
               targetSdkVersion : 23,
               versionCode      : 1,
               versionName      : "1.0"
               ]

    //官方库
    supportV4 = "com.android.support:support-v4:${supportVersion}"
    supportAppcompatV7 = "com.android.support:appcompat-v7:${supportVersion}"
    supportDesign = "com.android.support:design:${supportVersion}"
    supportCardView = "com.android.support:cardview-v7:${supportVersion}"
    supportRecyclerView = "com.android.support:recyclerview-v7:${supportVersion}"
    supportMultidex = "com.android.support:multidex:1.0.+"

    //图片加载
    glide = "com.github.bumptech.glide:glide:3.7.0"
    fresco = "com.facebook.fresco:fresco:0.9.0+"
    picasso = "com.squareup.picasso:picasso:2.5.2"

    //json解析
    fastjson = "com.alibaba:fastjson:1.2.8"

    //view注入
    dagger = "com.squareup.dagger:dagger:1.2.2"
    butterknife = "com.jakewharton:butterknife:7.0.1"

    //Rx家族，响应式编程
    rxJava = "io.reactivex:rxjava:1.1.1"
    rxAndroid = "io.reactivex:rxandroid:1.1.0"
    rxBinding = "com.jakewharton.rxbinding:rxbinding:${rxBindingVersion}"
    rxBindingSupportV4 = "com.jakewharton.rxbinding:rxbinding-support-v4:${rxBindingVersion}"
    rxBindingSupportAppcompatV7 = "com.jakewharton.rxbinding:rxbinding-appcompat-v7:${rxBindingVersion}"
    rxBindingSupportDesign = "com.jakewharton.rxbinding:rxbinding-design:${rxBindingVersion}"
    rxBindingSupportRecyclerView = "com.jakewharton.rxbinding:rxbinding-recyclerview-v7:${rxBindingVersion}"
    rxBindingLeanbackV17 = "com.jakewharton.rxbinding:rxbinding-leanback-v17:${rxBindingVersion}"

    //网络请求
    retrofit = "com.squareup.retrofit2:retrofit:2.0.0"
    okhttp = "com.squareup.okhttp3:okhttp:3.2.0"
    volley = "com.mcxiaoke.volley:library:1.0.19"

    //数据库
    sqlbrite = "com.squareup.sqlbrite:sqlbrite:0.6.2"
    greenDAO = "de.greenrobot:greendao:${greenDAOVersion}"
    greenDAOGenerator = "de.greenrobot:greendao-generator:${greenDAOVersion}"

}
```

然后在项目根目录的build.gradle文件中添加配置文件的引用
```gradle
//应用配置文件
apply from: "config.gradle"

buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:1.5.0'
    }
}

allprojects {
    repositories {
        jcenter()
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}
```

开始使用，在module的build.gradle文件中直接引用
```gradle
apply plugin: 'com.android.application'

android {
    compileSdkVersion rootProject.ext.android.compileSdkVersion
    buildToolsVersion rootProject.ext.android.buildToolsVersion

    defaultConfig {
        applicationId rootProject.ext.android.applicationId
        minSdkVersion rootProject.ext.android.minSdkVersion
        targetSdkVersion rootProject.ext.android.targetSdkVersion
        versionCode rootProject.ext.android.versionCode
        versionName rootProject.ext.android.versionName
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile rootProject.ext.supportV4
    compile rootProject.ext.supportAppcompatV7

    compile rootProject.ext.okhttp
    compile rootProject.ext.rxjava
    compile rootProject.ext.glide
    compile rootProject.ext.retrofit
    ...
}
```

## 参考资料
[Gradle依赖的统一管理 stormzhang](http://mp.weixin.qq.com/s?__biz=MzA4NTQwNDcyMA==&mid=402733201&idx=1&sn=052e12818fe937e28ef08331535a179e#rd)

