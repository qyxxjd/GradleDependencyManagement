## Gradle常用命令
windows下去掉前面的`./`
```
./gradlew           //下载更新gradle
./gradlew -v        //版本号
./gradlew assemble  //构建项目输出
./gradlew check     //运行检测和测试任务
./gradlew clean     //清除build文件夹
./gradlew build     //编译打包,包含debug、release
./gradlew assembleDebug     //编译并打debug包
./gradlew assembleRelease   //编译并打release包
./gradlew installRelease    //release打包并安装
./gradlew uninstallRelease  //卸载release包
./gradlew app:dependencies  //查看依赖关系
```


## Gradle依赖统一管理

项目根目录新建config.gradle文件，配置参考如下：
```gradle
def supportVersion = "24.2.1"
def rxBindingVersion = "0.4.0"
def retrofitVersion = "2.1.0"
def stethoVersion = "1.4.1"
def butterknifeVersion = "8.4.0"
def leakCanaryVersion = "1.4-beta2"
def blockCanaryVersion = "1.3.1"
def dagger2Version = "2.7"

ext {

    android = [
            compileSdkVersion: 24,
            buildToolsVersion: "24.0.2",
            applicationId    : "your package name",
            minSdkVersion    : 14,
            targetSdkVersion : 24,
            versionCode      : 1,
            versionName      : "1.0"
    ]

    //官方库
    supportV4 = "com.android.support:support-v4:${supportVersion}"
    supportAppcompatV7 = "com.android.support:appcompat-v7:${supportVersion}"
    supportDesign = "com.android.support:design:${supportVersion}"
    supportCardView = "com.android.support:cardview-v7:${supportVersion}"
    supportRecyclerView = "com.android.support:recyclerview-v7:${supportVersion}"
    supportGridLayout = "com.android.support:gridlayout-v7:${supportVersion}"
    supportAnnotations = "com.android.support:support-annotations:${supportVersion}"

    //基础项目
    basicProject = "com.classic.core:classic:2.2"
    //通用适配器
    commonAdapter = "com.classic.adapter:commonadapter:1.2"
    //方便的切换到：加载中视图、错误视图、空数据视图、网络异常视图、内容视图。
    mutipleStatusView = "com.classic.common:multiple-status-view:1.2"
    //动画
    nineoldandroids = "com.nineoldandroids:library:2.4.0"

    //图片加载
    glide = "com.github.bumptech.glide:glide:3.7.0"
    fresco = "com.facebook.fresco:fresco:0.12.+"
    picasso = "com.squareup.picasso:picasso:2.5.2"

    //json解析
    fastjson = "com.alibaba:fastjson:1.2.17"
    fastjsonAndroid = "com.alibaba:fastjson:1.1.54.android"

    //https://github.com/google/dagger
    dagger2 = "com.google.dagger:dagger:${dagger2Version}"
    dagger2Compiler = "com.google.dagger:dagger-compiler:${dagger2Version}"
    butterknife = "com.jakewharton:butterknife:${butterknifeVersion}"
    butterknifeCompiler = "com.jakewharton:butterknife-compiler:${butterknifeVersion}"

    //Rx家族，响应式编程
    rxJava = "io.reactivex:rxjava:1.2.0"
    rxAndroid = "io.reactivex:rxandroid:1.2.1"
    rxBinding = "com.jakewharton.rxbinding:rxbinding:${rxBindingVersion}"
    rxBindingSupportV4 = "com.jakewharton.rxbinding:rxbinding-support-v4:${rxBindingVersion}"
    rxBindingSupportAppcompatV7 = "com.jakewharton.rxbinding:rxbinding-appcompat-v7:${rxBindingVersion}"
    rxBindingSupportDesign = "com.jakewharton.rxbinding:rxbinding-design:${rxBindingVersion}"
    rxBindingSupportRecyclerView = "com.jakewharton.rxbinding:rxbinding-recyclerview-v7:${rxBindingVersion}"
    rxBindingLeanbackV17 = "com.jakewharton.rxbinding:rxbinding-leanback-v17:${rxBindingVersion}"
    //google开源的异步框架
    agera = "com.google.android.agera:agera:1.2.0-beta2"

    //网络请求
    retrofit = "com.squareup.retrofit2:retrofit:${retrofitVersion}"
    gsonForRetrofit = "com.squareup.retrofit2:converter-gson:${retrofitVersion}"
    rxJavaForRetrofit = "com.squareup.retrofit2:adapter-rxjava:${retrofitVersion}"
    okhttp = "com.squareup.okhttp3:okhttp:3.4.1"

    //facebook出品的网络调试神器
    stetho = "com.facebook.stetho:stetho:${stethoVersion}"
    stethoOkhttp = "com.facebook.stetho:stetho-okhttp3:${stethoVersion}"
    stethoUrlConnection = "com.facebook.stetho:stetho-urlconnection:${stethoVersion}"
    stethoJsRhino = "com.facebook.stetho:stetho-js-rhino:${stethoVersion}"

    //检测内存泄漏
    leakCanaryDebug = "com.squareup.leakcanary:leakcanary-android:${leakCanaryVersion}"
    leakCanaryRelease = "com.squareup.leakcanary:leakcanary-android-no-op:${leakCanaryVersion}"
    //检测UI卡顿
    blockCanaryDebug = "com.github.markzhai:blockcanary-android:${blockCanaryVersion}"
    blockCanaryRelease = "com.github.markzhai:blockcanary-no-op:${blockCanaryVersion}"

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
        classpath 'com.android.tools.build:gradle:2.1.3'
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
    ...
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

