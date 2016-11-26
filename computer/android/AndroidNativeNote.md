> AndroidStudio NDK开发

# 一、开发环境

URL:http://mp.weixin.qq.com/s?__biz=MjM5NDkxMTgyNw==&mid=2653057617&idx=1&sn=9e71c8ceb84ee95c71f4d6f7a45e8158#wechat_redirect

## 1、直接使用.so库

和eclipse不同，.so库是放到jniLibs目录下面，如图：

![img](http://mmbiz.qpic.cn/mmbiz/GVyeDObNlrFlv8XlfEv3B4nYsIFeRzET8JvstMCFuICR1WwEQSZn0BhU8licOiaVicZnQqvJiaficBWYJdoTWum29ag/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

使用方式和Eclipse中一样，直接使用System.loadLibrary("libName")加载库。

## 2、**使用C/C++源码**

### 2.1、**下载安装配置NDK**

首先是下载NDK，可以自己下载NDK，然后解压出来，然后指定NDK目录，在local.properties文件中配置NDK的路径，如图：

![img](http://mmbiz.qpic.cn/mmbiz/GVyeDObNlrFlv8XlfEv3B4nYsIFeRzETqIHGic2nJdOcoBsZEHTNlpXOibf8icQ6nK2EK2E7bwRwliaJNZibTXOOHnQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

你也可以在Android Studio中设置一下，让Android Studio自己下载对应版本的NDK。步骤如下：

    ● 在菜单栏找到"File"-"Settings"，打开设置界面；

    ● 找到"Appearance & Behavior"-"System Settings"-"Android SDK"选项，然后切换到"SDK Tool"选项卡；

    ● 然后找到NDK打钩；

    ● 点击"Apply"按钮，然后在弹出窗口中点击"OK"，即可自动下载；

    ● 等待自动下载安装完成，点击"Finish"按钮完成安装。

如下图所示：

![img](http://mmbiz.qpic.cn/mmbiz/GVyeDObNlrFlv8XlfEv3B4nYsIFeRzETOzE3PLiaHIW7iapl3B8hxxOfaYehYWAwzUlAYIwO41AkI0XtRFTdgcpg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

Android Studio默认安装NDK目录是在SDK目录下，安装完成后，local.properties文件中NDK路径设置也将自动更新



### 2.2、**Gradle添加NDK模块**

打开app模组下的build.gradle文件，在defaultConfig模块下添加ndk模块，如图：

![img](http://mmbiz.qpic.cn/mmbiz/GVyeDObNlrFlv8XlfEv3B4nYsIFeRzETlyiaDyCBteRxySdGQJYoKQJF4h5gUQNs15LDHmJfq8pSD5YBzFLpK3g/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

- 其中moduleName是编译的.so的模组名称，就是原先Eclipse开发里Android.mk文件中LOCAL_MODULE变量，和System.loadLibrary()加载.so时的名称对应。例如moduleName配置为"JniTest"，则.so文件名称为"libJniTest.so"，加载时，名称为System.loadLibrary("JniTest")；
- abiFilters指定要分配的平台，如果未指定，则将编译所有支持的平台。目前支持的平台有"armeabi"、"armeabi-v7a"、"arm64-v8a"、"mips"、"mips64"、"x86"、"x86_64"这七个；
- ldLibs是要链接的库，就是原先Android.mk里LOCAL_LDLIBS变量指定的库。



### 2.3、**添加C/C++文件**

默认情况下，C/C++文件一般放在[module]/src/main/jni/目录下，如图：

![img](http://mmbiz.qpic.cn/mmbiz/GVyeDObNlrFlv8XlfEv3B4nYsIFeRzETEDVmFdrzds2oPHbS0znNqibL4r9Pgg4jCtHXINBoicBWzwYcubzicIH8A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

当然，你也可以修改build.gradle配置，指定其他路径。在"android.sourceSets.main"模块里，使用"jni.srcDirs"指定jni的路径，如下图：

![img](http://mmbiz.qpic.cn/mmbiz/GVyeDObNlrFlv8XlfEv3B4nYsIFeRzETDGL7oeCSpadeUKyKibwxS5robJXyuficDAniarjesiafb0EUBFvc7zMAEQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

至此，Android Studio下开发JNI的基本配置就结束了，下一篇将介绍一下Android Studio下C/C++代码编写与编译。

## 3、添加自己的类

### 3.1、添加native方法    

首先我们新建一个类，例如取名叫"JniUtil"，然后新建一个native方法，用来实现字符串拼接，如下图：

![img](http://mmbiz.qpic.cn/mmbiz/GVyeDObNlrFlv8XlfEv3B4nYsIFeRzETV9Qiccoiar09LBFHnPSDKqgOYAlKSmuaJAvv3Agoic57dibFibUL40gYd0A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

### 3.2、生成头文件

**1. 生成class文件**

Build一下工程，Build成功后，会在app\build\intermediates\classes\debug目录下会自动生成所有类的class文件，如下图：

![img](http://mmbiz.qpic.cn/mmbiz/GVyeDObNlrFlv8XlfEv3B4nYsIFeRzET51WeA1uSVtOCuSiaE2LlvmjTI8WWic6hNdj0cpeB0ibenfrlSEsphSA2Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

**2. 在Terminal中切换到debug目录**

在Android Studio上找到Terminal标签页，然后通过命令切换到app\build\intermediates\classes\debug目录。

![img](http://mmbiz.qpic.cn/mmbiz/GVyeDObNlrFlv8XlfEv3B4nYsIFeRzETymMAiczzDQUWYs95owO4EDeia0ZKib4AicUxGo6Y1z4BtR2ql6pFkGnuibA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

**3. 生成头文件**

通过"javah -jni"命令，生成头文件，我们要生成com\samonxu\jnitest目录下的JniUtil.class文件对应的头文件，命令如下图所示：

![img](http://mmbiz.qpic.cn/mmbiz/GVyeDObNlrFlv8XlfEv3B4nYsIFeRzETcicMyRqe3R2SXp0ibPuMIwny6dbXGPPhrUcXjcRaCymyuYRE5WgAxFTA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)我们就会发现，debug目录下多了一个.h文件。

![img](http://mmbiz.qpic.cn/mmbiz/GVyeDObNlrFlv8XlfEv3B4nYsIFeRzET3MwtBmxDYLibobiariaFAtngxMARXA99YLoXO274vWjs67Nkkk4vGoxEg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

若查看这个.h文件的内容，你会发现我们在JniUtil类中定义的native方法对应的C/C++函数，在头文件中已经声明好了。

![img](http://mmbiz.qpic.cn/mmbiz/GVyeDObNlrFlv8XlfEv3B4nYsIFeRzETDO2vtz5RfPxwhQicS1iboo8zheic22z1DE0cHx8ffUzK6btYG4DADL6KA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

然后我们将这个.h文件复制到我们的jni目录下。

![img](http://mmbiz.qpic.cn/mmbiz/GVyeDObNlrFlv8XlfEv3B4nYsIFeRzET0bHVichKSalCeUYdWpvqghicQ8FmxGyLUYU494SJgRTsQqMLYC4WlyKg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

记得在Terminal中退出到主目录，否则Rebuild工程的时候，无法执行clean操作。

![img](http://mmbiz.qpic.cn/mmbiz/GVyeDObNlrFlv8XlfEv3B4nYsIFeRzETqa6Ry8gwdSYjK6qM6IRmRDVA25qia1VzXKv9CCUbcgVWMsHj0CD9wZw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

### 3.3、实现头文件中声明的函数

在jni目录下，新建一个.c文件，例如取名叫"jnitest.c"；

![img](http://mmbiz.qpic.cn/mmbiz/GVyeDObNlrFlv8XlfEv3B4nYsIFeRzETw3icpd9BMB1c2Oo2M1kCmcCjhL3omDjBDTNdiaCRBYAdBC90jpA4Fmvw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

 编写代码，实现函数Java_com_samonxu_jnitest_JniUtil_append()

![img](http://mmbiz.qpic.cn/mmbiz/GVyeDObNlrFlv8XlfEv3B4nYsIFeRzET8syPpJh4PjiaRUzUnUZtTlCOw449oFHnKqNsxqyqGiaSu1RtqtdVnPJw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

### 3.4、调用native方法

首先在JniUtil类中添加一段static的代码，加载我们的.so库。

![img](http://mmbiz.qpic.cn/mmbiz/GVyeDObNlrFlv8XlfEv3B4nYsIFeRzETcj17WHA0srHA9ntNM6BGPic6RFyhVVHZvYNeSOab3olV2qxwTx1icf6w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

然后创建个Activity，调用append()方法，将"abc"和"123"的拼接结果显示到一个TextView上。

![img](http://mmbiz.qpic.cn/mmbiz/GVyeDObNlrFlv8XlfEv3B4nYsIFeRzETWZ1hDFh3nRWRg2ndr8o4EXFibknHkx9bveelwtNMs4pdRavf5LoOfMg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

    

### 3.5、编译运行程序

编译并运行程序，结果如下图：

![img](http://mmbiz.qpic.cn/mmbiz/GVyeDObNlrFlv8XlfEv3B4nYsIFeRzET0VPRMf92jv8icRNWIpodlJP5iadC6iajuVUqwUfibEmZH9XMFX8Mnm5wPQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1)

至此，一个简单的JNI程序就完成了。



























