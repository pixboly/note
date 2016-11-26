> Groovy笔记
--
[Groovy脚本基础全攻略](http://blog.csdn.net/yanbober/article/details/49047515)
--
# 一、背景

## 1、简介
Groovy脚本基于Java且拓展了Java，所以从某种程度来说掌握Java是学习Groovy的前提，故本文适用于不熟悉Groovy却想快速得到Groovy核心基础干货的Java开发者（注意是Java），因为我的目的不是深入学习Groovy语言，所以本文基本都是靠代码来解释，这样最直观，同时也够干货基础入门Groovy的特点和结构。
开始介绍前先给一个大法，[《官方权威指南》](http://www.groovy-lang.org/index.html)英文好的可以直接略过本文后续内容，我需要的只是Groovy皮毛；再次向Groovy的标志致敬!

Groovy是一种动态语言，它和Java类似（算是Java的升级版，但是又具备脚本语言的特点），都在Java虚拟机中运行。当运行Groovy脚本时它会先被编译成Java类字节码，然后通过JVM虚拟机执行这个Java字节码类。

## 2、快速安装指南
安装Groovy在各种Bash下都是通用的，具体如下命令就可搞定：
```
$ curl -s get.sdkman.io | bash
$ source "$HOME/.sdkman/bin/sdkman-init.sh"
$ sdk install groovy
$ groovy -version
//至此就可以享用了！
```
# 二、语法基础
## 1、注释
Groovy的单行注释、多行注释、文档注释基本都和Java一样，没啥特殊的，不再细说。只有一种特殊的单行注释需要留意一下即可。如下：
```
#!/usr/bin/env groovy
println "Hello from the shebang line"
```
这种注释通常是用来给UNIX系统声明允许脚本运行的类型的，一般都是固定写法，没啥讲究的。

## 2、关键字
Groovy有如下一些关键字，我们些代码命名时要注意：

as、assert、break、case、catch、class、const、continue、def、default、do、else、enum、extends、false、finally、for、goto、if、implements、import、in、instanceof、interface、new、null、package、return、super、switch、this、throw、throws、trait、true、try、while

这玩意和其他语言一样，没啥特殊的，自行脑补。

## 3、标识符
对于Groovy的标示符和Java还是有些共同点和区别的，特别是引用标示符的区别，具体可以往下看。

### 3.1、普通标识符
普通标识符定义和C语言类似，只能以字母、美元符、下划线开始，不能以数字开头。如下例子：
```
//正确
def name
def $name
def name_type
def foo.assert
//错误
def 5type
def a+b
```

### 3.2、引用标识符
引用标识符出现在点后的表达式中，我们可以如下一样使用：
```
def map = [:]
//引用标示符中出现空格也是对的
map."an identifier with a space and double quotes" = "ALLOWED"
//引用标示符中出现横线也是对的
map.'with-dash-signs-and-single-quotes' = "ALLOWED"

assert map."an identifier with a space and double quotes" == "ALLOWED"
assert map.'with-dash-signs-and-single-quotes' == "ALLOWED"
```


# QAQ、附录与问题
## 1、安装完成运行报错
执行
```
groovy -version
```
报错信息：
```
root@ubuntu-H55M-S2H:/home/tangpengxiang/test/testgroovy# groovy
java.lang.SecurityException: Prohibited package name: java.lang
	at java.lang.ClassLoader.preDefineClass(ClassLoader.java:662)
	at java.lang.ClassLoader.defineClass(ClassLoader.java:761)
	at java.security.SecureClassLoader.defineClass(SecureClassLoader.java:142)
	at java.net.URLClassLoader.defineClass(URLClassLoader.java:467)
	at java.net.URLClassLoader.access$100(URLClassLoader.java:73)
	at java.net.URLClassLoader$1.run(URLClassLoader.java:368)
	at java.net.URLClassLoader$1.run(URLClassLoader.java:362)
	at java.security.AccessController.doPrivileged(Native Method)
	at java.net.URLClassLoader.findClass(URLClassLoader.java:361)
	at org.codehaus.groovy.tools.RootLoader.oldFindClass(RootLoader.java:175)
	at org.codehaus.groovy.tools.RootLoader.loadClass(RootLoader.java:147)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:357)
	at java.lang.ClassLoader.defineClass1(Native Method)
	at java.lang.ClassLoader.defineClass(ClassLoader.java:763)
	at java.security.SecureClassLoader.defineClass(SecureClassLoader.java:142)
	at java.net.URLClassLoader.defineClass(URLClassLoader.java:467)
	at java.net.URLClassLoader.access$100(URLClassLoader.java:73)
	at java.net.URLClassLoader$1.run(URLClassLoader.java:368)
	at java.net.URLClassLoader$1.run(URLClassLoader.java:362)
	at java.security.AccessController.doPrivileged(Native Method)
	at java.net.URLClassLoader.findClass(URLClassLoader.java:361)
	at org.codehaus.groovy.tools.RootLoader.oldFindClass(RootLoader.java:175)
	at org.codehaus.groovy.tools.RootLoader.loadClass(RootLoader.java:147)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:357)
	at org.codehaus.groovy.tools.GroovyStarter.rootLoader(GroovyStarter.java:99)
	at org.codehaus.groovy.tools.GroovyStarter.main(GroovyStarter.java:131)
```
[处理链接](http://cache.baiducontent.com/c?m=9d78d513d99601f205a9df690d6796314813c0387a9cc7140f83d919c4261d1a1a3af7fc677c1f5e95833e7000dc5441baae6b27200357e6c19fd4148dac925f7ed57829701e844a0fd11db29e06789c64db4de9d958b4f1e732e5ef85829814088c185230d1a7c91c5b499678f06273a3fbc313450456edbb2765891b2478c67015e150f9966e3d58d6e1dd2a14906e837611e1f166ec6b05b564fe59447a&p=847ec9298d904ead01bd9b7e0e1190&newp=c374c25e8c904ead08e2947d0a4a89231610db2151d7d31f6b82c825d7331b001c3bbfb423231105d8c6796700ab4f5eeaf73c75330123a3dda5c91d9fb4c57479&user=baidu&fm=sc&query=groovy+SecurityException&qid=a798602a0002dacd&p1=3)
```
groovy 报 SecurityException: Prohibited package name: java.lang

发表于：2009年6月22日 | 分类：java | 标签： classpath, groovy | views(4,626)
版权信息: 可以任意转载, 转载时请务必以超链接形式标明文章原文出处, 即下面的声明.

原文出处：http://blog.chenlb.com/2009/06/groovy-say-securityexception-prohibited-package-name-java-lang.html

今天想在一个 linux 服务器上安装 groovy，照着以前的博文安装：Groovy 安装，然后测试下是否安装成功？

groovy -v
java.lang.SecurityException: Prohibited package name: java.lang
...
报错，然后找到 解决grails报java.lang.SecurityException:Prohibited问题

说要把 rt.jar 去了。可能是 groovy 加载的东西与 java 默认的不一样。原来 CLASSPATH 是：

export JAVA_HOME=/home/java
export CLASSPATH=$CLASSPATH:$JAVA_HOME/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin
把 rt.jar 从 CLASSPATH 中除去就可以了。
```
## 2、LINUX修改环境变量
> [链接地址](http://www.cnblogs.com/samcn/archive/2011/03/16/1986248.html)

