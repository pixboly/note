>Gradle 笔记
# 一、Gradle脚本基础全攻略
[http://blog.csdn.net/yanbober/article/details/49314255]
## 1、背景
在开始Gradle之前请务必保证自己已经初步了解了Groovy脚本，特别是闭包规则，如果还不了解Groovy则可以先看[《Groovy脚本基础全攻略》](http://blog.csdn.net/yanbober/article/details/49047515)这一篇博客速成一下Groovy基础，然后再看此文即可。关于Gradle速成干货基础详情也请参考[Geadle官方网站](https://gradle.org/)，不好意思我太Low了。



# 二、Android Gradle 看这一篇就够了
[http://android.walfud.com/android-gradle-%E7%9C%8B%E8%BF%99%E4%B8%80%E7%AF%87%E5%B0%B1%E5%A4%9F%E4%BA%86/]

## 1、Android Gradle 语法
目前, 大多数讲解 Gradle 的文章都是先从复杂的 Gradle 语法开始. 而实际上, 对于 Android 人员, 掌握这些语法细节并没有卵用, 我们仅需要能看懂, 随用随查即可. 那本文也是遵照 ‘实用’ 这个原则介绍 Android Gradle. 相信, 读过本文, 你至少应该不在畏惧 Build Script 了
如果你对 build.gradle 已经很熟悉, 那么直接参考 gooogle 官方的(Android Plugin DSL Reference)[http://google.github.io/android-gradle-dsl/current/] 即可.

### 1.1、Android Gradle项目概述
下面进入主题. 先来看下 As 帮我们生成的有关于 Gradle 的几个文件(夹).
![图片描述](http://note.youdao.com/yws/public/resource/4399b4cc0a8b342c45bd3f8e0c2a3407/E6F95C32377C40FAA9AF393F410F121A)

如上图, 标准的 As 项目中, 包含三大部分:

 - Top-level Gradle:用于配置所有 Module 的属性
 - Moudle-level Gradle: 配置独立 Moudle 的属性
 - Gradle Wrapper: 用于统一编译环境, 一般供 CI 使用
下面来具体看看.

### 1.2、Top-level Build Script
![](http://note.youdao.com/yws/public/resource/4399b4cc0a8b342c45bd3f8e0c2a3407/2BA28FBA43054C3DA39B952F0BFE80F5)

有几个概念, 都来自于 Gradle Reference:

gradle script 都是 configuration scripts. 这话的意思是说, 运行起来后, 每个脚本文件最终都会对应到一个程序中的对象, 这个对象叫做 delegate object. 比如, build.gradle 对应为程序中的 Project. 整个 Gradle 有三种类型的代理对象, 分别是:


|Type of script |	Delegates to instance of|
|---------------|--------------------------|
|Build script	|Project|
|Init script	|Gradle|
|Settings script	|Settings|

通过上面的知识我们可以知道, 在任何 build.gradle 中都有一个内置的变量 — project(就是代表这个 build.gradle 的 delegate object).
![](http://note.youdao.com/yws/public/resource/4399b4cc0a8b342c45bd3f8e0c2a3407/AB95A5C106F14AAD9AB1D2E76A8F4987)
我们可以通过 gradle -q properties 查看脚本中所有的内建属性.

下面, 我们一步一步来看看这个 Top-level Build Script:
#### 1.2.1、顶级build.gradle
**buildscript**
```
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.2.0'
 
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}
```

> buildscript { }

> Configures the build script classpath for this project.

> The given closure is executed against this project’s ScriptHandler. The ScriptHandler is passed to the closure as the closure’s delegate.

> Delegates to:
> ScriptHandler from buildscript

简单来说, buildscript 就是运行构建脚本所需要用到的依赖的 classpath. 其内部会把 configuration closure 传递给 ScriptHandler (你不用关心这是什么) 然后实现设置 dependencies(构建脚本运行所依赖的文件) 和 repositories(依赖文件的查找位置).
一般而言, 这个 script block 都写在开头, 声明这个脚本本身所需要的依赖


***allprojects***
```
allprojects {
    repositories {
        jcenter()
    }
}
```
> allprojects { }

> Configures this project and each of its sub-projects.

> This method executes the given closure against this project and its sub-projects. The target Project is passed to the closure as the closure’s delegate.

> Delegates to:
> Each Project in allprojects

这个 configuration closure 里的内容会被设置为当前 project 以及其所有 sub-project 中依赖文件的查找路径.

这里的写法意味着, 所有的 project 都会在 jcenter() 中寻找依赖. 除此之外, 还可以指定 flatDir, maven, ivy 等. 可以参考 [RepositoryHandler](https://docs.gradle.org/current/dsl/org.gradle.api.artifacts.dsl.RepositoryHandler.html).

***task clean***
```
task clean(type: Delete) {
    delete rootProject.buildDir
}
```
这是定义了一个新的 task, 叫做 clean. 其类型是 Delete. 实际上, Android Plugin 内置了 clean 方法, 该方法位于 module 中. Module 中内置的 clean 方法只会清理 Module 中的文件并删除 Module 中的 build 目录, 但是工程根目录下的 build 文件是没人清理的, 所以这里定义的 clean 方法即删除项目目录下的 build 文件夹.

#### 1.2.2、settings.gradle
顺便来看一下 settings.gradle:
```
include ':app'
```
这文件内容一看就懂, 是管理 sub-project. 凡是要涉及到编译的子项目, 都要写在这里, 这样 gradle 就按照这个配置递归编译子项目了.

### 1.3、Module-level Build Script

![](http://note.youdao.com/yws/public/resource/4399b4cc0a8b342c45bd3f8e0c2a3407/6DB93B1051A241899C5756F3AB04617F)

模块级 script 用于描述该模块的编译过程. 一般而言, Android 能用到的一共有三种:

 - Application: 对应 com.android.application. 编译的结果是一个 apk
 - Android Library: 对应 com.android.library. 编译结果为 aar
 - JavaLibrary: 对应的 java. 编译结果为 jar
 
你是不是好奇 android 一共有多少种插件? 可以看看 [google android plugin 的仓库](https://android.googlesource.com/platform/tools/build/+/master/gradle/src/main/groovy/com/android/build/gradle)

由于 Module-level Build Script 大部分都是相应插件自行实现的内容, 所以我们就不能再 gradle 文档中找了, 我们要到 google 系的 refs 中搜索 (见 refs).

你是不是好奇这个文件到底有多少个属性? 可以看看这个[文件的源码](https://android.googlesource.com/platform/tools/build/+/master/gradle/src/main/groovy/com/android/build/gradle/AppPlugin.groovy)

**ANDROID**
```
android {
    compileSdkVersion 24
    ....
}
```

其实 `application` 插件支持的属性远远比这个要多, 咱们来看一个全集:
```
apply plugin: 'com.android.application'
 
android {
    /**
     * 设置编译 sdk 和编译工具的版本
     */
    compileSdkVersion 24
    buildToolsVersion "24.0.3"
 
    /**
     * 关于签名, 请参考 google 官方文档: <a href="https://developer.android.com/studio/publish/app-signing.html#debug-mode">Sign Your App</a>
     */
    signingConfigs {
        /**
         * As 会自动帮我们使用 debug certificate 进行签名. 这个 debug certificate 每次安装 As 都会变,
         * 因此不适合作为发布之用.
         */
        debug {
        }
 
        /**
         * 由于 Module-level Build Script(本文件) 也要放在 VCS 中管理, 所以不将密码等信息写在这里.
         * 一般的做法是: 在本机设置环境变量, 然后通过下面代码中演示的这种方式读取.
         * 当然, 最佳实践也指导我们将 `gradle.properties` <strong><em>排除</em>在 VCS 之外</strong>,
         * 此时, 也在该文件中将密码设置为变量, 然后在此读取使用.
         */
        release {
            storeFile file("$System.env.STORE_FILE")
            storePassword "$System.env.STORE_PASSWORD"
            keyAlias "$System.env.KEY_ALIAS"
            keyPassword "$System.env.KEY_PASSWORD"
        }
    }
 
    /**
     * 为所有的 build variants 设置默认的值. 关于 build variant, 我们后面会用一张图片说明
     */
    defaultConfig {
        applicationId "com.walfud.myapplication"
        minSdkVersion 23
        targetSdkVersion 24
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
 
    /**
     * type 默认会有 debug 和 release. 不管你写不写都如此.
     * 通常, 我们在 debug 中保留默认值, release 中开启混淆, 并使用私有的签名
     */
    buildTypes {
        debug {
            // 使用默认值
        }
 
        release {
            // 混淆
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
 
            // 签名
            signingConfig signingConfigs.release
        }
    }
 
    /**
     * flavor 强调的是不同的版本, 比如付费版和免费版.
     * 在国内, 这个字段更多被用于区分不同的渠道, 即 360 渠道, 小米渠道等等.
     */
    productFlavors {
        m360 {}
        xiaomi {}
    }
 
    /**
     * 这个选项基本不用.
     * <a href="http://tools.android.com/tech-docs/new-build-system/user-guide/apk-splits">官方说</a>: 使用 splits 可以比使用 flavor 更加有效创建多 apk.
     * 目前而言, 仅支持 Density 和 ABIs 这两个分类.
     */
    splits {
        // 按屏幕尺寸
        density {
            enable true
 
            // 默认包含全部分辨率, 这里是剔除一些我们不要的
            exclude "ldpi", "mdpi", "xxxhdpi", "400dpi", "560dpi", "tvdpi"
        }
 
        // 按架构
        abi {
            enable true
 
            // 使用 `reset()` 后, 我们就相当于不包含任何架构,
            // 这种情况下我们就可以通过 `include` 指定想要使用的架构
            reset()
 
            include 'x86', 'armeabi-v7a'
            universalApk true       // 是否同时生成一个包含全部 Architecture 的包
        }
    }
}
 
/**
 * 这个项目的依赖
 */
dependencies {
    /**
     * `fileTree` 导入 libs 目录下的所有 jar 文件
     */
    compile fileTree(dir: 'libs', include: ['*.jar'])
 
    /**
     * 想导入本地 aar, 首先需要指明本地 aar 的位置, 如下 `repositories` 中所示, 我们把 aar 放在了
     * Module-level 的 libs 目录下. 然后引用这个文件即可.
     */
    compile(name: 'components', ext: 'aar')
}
 
/**
 * 配置了去哪里查找这个模块依赖文件
 */
repositories {
    flatDir {
        dirs 'libs'
    }
}
```

**Build Variant**
![](http://note.youdao.com/yws/public/resource/4399b4cc0a8b342c45bd3f8e0c2a3407/96BE23CFF9F949E583725D35E49028F7)

简单的说, 就是为了不同的渠道或者版本自动生成相应的 apk. 其算法是 buildType * productFlavor * density * abi 做一个笛卡尔积.

至于优先级, 参考 google 关于 Build Variants 的文档.

## 2、第二部分   —   AndroidStuido 里的 Gradle
### 2.1、自动下载
每一次重新导入工程时, 因为默认会使用 Use default gradle wrapper (recommended) 设置, 所以 As 会下载 gradle-wrapper.properties 中 distributionUrl 所指定的 gradle 版本. 这个过程在国内普遍巨慢无比. 我的建议是找到上述文件, 动手修改 distributionUrl 为已下载过的 gradle 版本(比如你之前已经下载过 gradle-2.14.1 了), 这样就能直接打开了.

实际上, 这个配置项位于 As 自动生成的配置文件(.idea/gradle.xml), 我们来看看:

对于新建 (‘File -> New -> New Project’)的项目,  As 是这样给我们生成的:
![](http://note.youdao.com/yws/public/resource/4399b4cc0a8b342c45bd3f8e0c2a3407/EBA766CA10324BA09A5181B3D04D978D)
它在设置中的表现是这样的:
![](http://note.youdao.com/yws/public/resource/4399b4cc0a8b342c45bd3f8e0c2a3407/4A8149733E3A40FCB77123DAA315F75A)
可见, As 帮我限定了 gradle 版本. 当然, 如果这个指定的 gradle 版本不存在的话, 一样会去下载.

再来看看被 ‘File -> Open’ (注意, 不是 Reopen) 打开的项目:
![](http://note.youdao.com/yws/public/resource/4399b4cc0a8b342c45bd3f8e0c2a3407/76329E8157B04FBA94F5CCD669FC2ACA)
![](http://note.youdao.com/yws/public/resource/4399b4cc0a8b342c45bd3f8e0c2a3407/A08E7DB361834AE5B78FF31A8D95AD8E)
也就是说, 每次你 Open 一个项目的时候, As 会忽略你之前设置的 gradle 版本, 而使用 wrapper 文件中所指定的版本. 所以你 clone 下来的代码第一次打开都会卡很久的原因, 就是因为他们在下载 wrapper 中的 gradle 啊…

### 2.2、gradle-wrapper.properties
AS 创建的工程, 在根目录下有 gradlew.bat 和 gradlew.sh 文件. 这两个文件会读取 gradle/wrapper/gradle-wrapper.properties 并使用其中指定的 gradle 版本进行编译. 一般而言这套机制用于 CI 服务器中来保障每次编译都在同样的环境下.
```
#Wed Nov 11 21:08:45 CST 2015
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-2.10-all.zip
```
GRADLE_USER_HOME 默认值是你的 USER_HOME/.gradle (win 下是 c:\Users\<username>\.gradle, linux 下是 ~/.gradle ).

对于 gradlew 而言, 如果上述路径没有找到可执行的 gradle 文件, 则会使用 distributionUrl 中所指定的 url 下载后在执行.

### 2.3、gradle.properties
主要作用配置一些与当前机器 gradle 编译相关的属性. 这些属性是每个编译机器根据自己情况决定的(比如, 分配多大内存), 因此不适合放在 git 中.

#### 2.3.1、property 分为两种
##### 2.3.1.1、SYSTEM PROPERTY
类似于系统的环境变量. 作为 jvm 启动参数.

a) Set
```
# gradle.properties
systemProp.xxx=yyy
```
system property 都以 systemProp.xxx 为模板.

只有 top-level gradle.property 才能设置 system property.

系统提供了如下几个内置变量:

org.gradle.daemon
是否开启 daemon. 一般来说, 本机编译建议打开, CI 上建议关闭.

org.gradle.java.home
指定 gradle 进程的 java home. 没什么卵用.

org.gradle.jvmargs
daemon 进程的 jvm 参数. 当你编译报错 OOM 的时候, 可以调整这个参数 (见后面的例子)

org.gradle.configureondemand
自行 google. 没卵用

org.gradle.parallel
project 之间并行编译

当然, 我们也可以通过命令行中指定 -D 来设置 system property.

b) Get
```
// build.gradle
System.properties['xxx']
```
##### 2.3.1.2、PROJECT PROPERTY
一般用作 project 内部的变量, 保存用户名密码之类的私有值.
a) Set
```
# gradle.properties
xxx=yyy
```
也可以通过命令行中指定 -P 来设置 project property
b) Get
```
// build.gradle
println xxx
```
#### 2.3.2、设置代理

```
systemProp.https.proxyHost=www.proxyhost.org
systemProp.https.proxyPort=8080
systemProp.https.proxyUser=userid
systemProp.https.proxyPassword=password
systemProp.https.nonProxyHosts=*.nonproxyrepos.com|localhost
```

#### 2.3.3、优先级 (下面的优先级更高)
project 目录下的 gradle.properties

$GRADLE_USER_HOME/gradle.properties

环境变量或者命令行中使用 -Dsystem.property 或 -Pproject.property设置

### 2.4、transitive = true
```
compile('com.crashlytics.sdk.android:answers:1.3.10@aar') {
    transitive = true;
}
```
简单地说, 由于该语句使用 @aar notation, 所以 gradle 只会下载这一个 aar 文件, 而不会顺带着下载这个 aar 所需要的依赖文件. 所以需要 transitive 让依赖能够自动被下载. 一般而言, 去掉 @aar 以及 { transitive = true } 不会有任何问题.

## 3、Refs:

一般而言, google 系的文章只会告诉你如何配置会产生怎样的效果, 而 gradle 系的文章会告诉你这么配置的原理是什么. 个人撰写的博客用于快速打通.

个人系:

深入理解Android（一）：Gradle详解
[http://www.infoq.com/cn/articles/android-in-depth-gradle]
google 系:

Android 官方: New Build System
[http://tools.android.com/tech-docs/new-build-system]
Android 官方: Configure Your Build 
[https://developer.android.com/studio/build/index.html]
Android 官方: Android Plugin for Gradle Release Notes
[https://developer.android.com/studio/releases/gradle-plugin.html]
Android Plugin DSL (有了这个, 妈妈再也不怕我找不到函数了)
[http://google.github.io/android-gradle-dsl/current/]
Android Plugin 源码仓库
[https://android.googlesource.com/platform/tools/build/+/master/gradle/src/main/groovy/com/android/build/gradle]
com.android.application 的源码
[https://android.googlesource.com/platform/tools/build/+/master/gradle/src/main/groovy/com/android/build/gradle/AppPlugin.groovy]
Android Plugin 的 Bintray 仓库
[https://bintray.com/android/android-tools/com.android.tools.build.gradle]
gradle 系:

Gradle Build Language Reference
[https://docs.gradle.org/current/dsl/]
gradle.properties 中的变量: The Build Environment
[https://docs.gradle.org/current/userguide/build_environment.html]