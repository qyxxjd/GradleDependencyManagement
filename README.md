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
def supportVersion = "28.0.0"
def rxBindingVersion = "2.2.0"
def retrofitVersion = "2.5.0"
def stethoVersion = "1.5.0"
def butterKnifeVersion = "9.0.0-rc2"
def leakCanaryVersion = "1.6.2"
def blockCanaryVersion = "1.5.0"
def dagger2Version = "2.19"
def okHttpVersion = "3.12.0"
def glideVersion = "4.8.0"

ext {

    android = [
            compileSdkVersion: 28,
            buildToolsVersion: "28.0.3",
            applicationId    : "your package name",
            minSdkVersion    : 16,
            targetSdkVersion : 28,
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

    //图片加载
    glide = "com.github.bumptech.glide:glide:${glideVersion}"
    glideCompiler = "com.github.bumptech.glide:compiler:${glideVersion}"
    fresco = "com.facebook.fresco:fresco:1.11.0"

    //https://github.com/google/dagger
    dagger2 = "com.google.dagger:dagger:${dagger2Version}"
    dagger2Compiler = "com.google.dagger:dagger-compiler:${dagger2Version}"
    butterknife = "com.jakewharton:butterknife:${butterknifeVersion}"
    butterknifeCompiler = "com.jakewharton:butterknife-compiler:${butterknifeVersion}"

    //Rx家族，响应式编程
    rxJava = "io.reactivex.rxjava2:rxjava:2.2.4"
    rxAndroid = "io.reactivex.rxjava2:rxandroid:2.1.0"
    rxBinding = "com.jakewharton.rxbinding2:rxbinding:${rxBindingVersion}"
    rxBindingSupportV4 = "com.jakewharton.rxbinding2:rxbinding-support-v4:${rxBindingVersion}"
    rxBindingSupportAppcompatV7 = "com.jakewharton.rxbinding2:rxbinding-appcompat-v7:${rxBindingVersion}"
    rxBindingSupportDesign = "com.jakewharton.rxbinding2:rxbinding-design:${rxBindingVersion}"
    rxBindingSupportRecyclerView = "com.jakewharton.rxbinding2:rxbinding-recyclerview-v7:${rxBindingVersion}"
    rxBindingLeanbackV17 = "com.jakewharton.rxbinding2:rxbinding-leanback-v17:${rxBindingVersion}"

    //网络请求
    retrofit = "com.squareup.retrofit2:retrofit:${retrofitVersion}"
    gsonForRetrofit = "com.squareup.retrofit2:converter-gson:${retrofitVersion}"
    rxJavaForRetrofit = "com.squareup.retrofit2:adapter-rxjava2:${retrofitVersion}"
    okhttp = "com.squareup.okhttp3:okhttp:${okhttpVersion}"
    okhttpLoggingInterceptor = "com.squareup.okhttp3:logging-interceptor:${okhttpVersion}"

    //facebook出品的网络调试神器
    stetho = "com.facebook.stetho:stetho:${stethoVersion}"
    stethoOkhttp = "com.facebook.stetho:stetho-okhttp3:${stethoVersion}"
    stethoUrlConnection = "com.facebook.stetho:stetho-urlconnection:${stethoVersion}"
    stethoJsRhino = "com.facebook.stetho:stetho-js-rhino:${stethoVersion}"
}
```

然后在项目根目录的build.gradle文件中添加配置文件的引用
```gradle
//应用配置文件
apply from: "config.gradle"

buildscript {
    repositories {
        google()
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.2.1'
    }
}

allprojects {
    repositories {
        google()
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

    compile rootProject.ext.okHttp
    compile rootProject.ext.rxJava
    compile rootProject.ext.glide
    compile rootProject.ext.retrofit
    ...
}
```
