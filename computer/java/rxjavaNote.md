RxJava学习

概述、

# 一、深入浅出RxJava

[**大头鬼****Bruce**](http://blog.csdn.net/lzyzsd) [深入浅出RxJava](http://blog.csdn.net/lzyzsd/article/details/41833541)

## 1、基础篇

[RxJava](https://github.com/ReactiveX/RxJava)正在Android开发者中变的越来越流行。唯一的问题就是上手不容易，尤其是大部分人之前都是使用命令式编程语言。但是一旦你弄明白了，你就会发现RxJava真是太棒了。
这里仅仅是帮助你了解RxJava，整个系列共有四篇文章，希望你看完这四篇文章之后能够了解RxJava背后的思想，并且喜欢上RxJava。 

### 1.1、基础

RxJava最核心的两个东西是Observables（被观察者，事件源）和Subscribers（观察者）。Observables发出一系列事件，Subscribers处理这些事件。这里的事件可以是任何你感兴趣的东西（触摸事件，web接口调用返回的数据。。。）
一个Observable可以发出零个或者多个事件，知道结束或者出错。每发出一个事件，就会调用它的Subscriber的onNext方法，最后调用Subscriber.onNext()或者Subscriber.onError()结束。

Rxjava的看起来很想设计模式中的观察者模式，但是有一点明显不同，那就是如果一个Observerble没有任何的的Subscriber，那么这个Observable是不会发出任何事件的。

Hello World

创建一个Observable对象很简单，直接调用Observable.create即可

```java
Observable<String> myObservable = Observable.create(  
    new Observable.OnSubscribe<String>() {  
        @Override  
        public void call(Subscriber<? super String> sub) {  
            sub.onNext("Hello, world!");  
            sub.onCompleted();  
        }  
    }  
);
```

这里定义的Observable对象仅仅发出一个Hello World字符串，然后就结束了。接着我们创建一个Subscriber来处理Observable对象发出的字符串。

```Java
Subscriber<String> mySubscriber = new Subscriber<String>() {  
    @Override  
    public void onNext(String s) { System.out.println(s); }  
  
    @Override  
    public void onCompleted() { }  
  
    @Override  
    public void onError(Throwable e) { }  
};  	
```

这里subscriber仅仅就是打印observable发出的字符串。通过subscribe函数就可以将我们定义的myObservable对象和mySubscriber对象关联起来，这样就完成了subscriber对observable的订阅。

```java
myObservable.subscribe(mySubscriber);
```

一旦mySubscriber订阅了myObservable，myObservable就是调用mySubscriber对象的onNext和onComplete方法，mySubscriber就会打印出Hello World！

```Java
/**
 * hello world测试
 */
private void testRxHelloWorld() {
    //定义一个事件源
    Observable<String> myObservable = Observable.create(new Observable.OnSubscribe<String>(){
        @Override
        public void call(Subscriber<? super String> subscriber) {
            subscriber.onNext("Hello,world!");
            subscriber.onCompleted();
        }
    });
    //定义一个订阅者
    Subscriber<String> mySubscriber = new Subscriber<String>() {
        @Override
        public void onCompleted() {
        }
        @Override
        public void onError(Throwable e) {
        }
        @Override
        public void onNext(String s) {
            Log.d(TAG,"mySubscriber:" + s);
        }
    };
    //事件订阅订阅者
    myObservable.subscribe(mySubscriber);
}
```

```Shell
07-03 10:53:52.915 4671-4671/com.pix.test D/MainActivity: mySubscriber:Hello,world!
```

**更简洁的代码**

是不是觉得仅仅为了打印一个hello world要写这么多代码太啰嗦？我这里主要是为了展示RxJava背后的原理而采用了这种比较啰嗦的写法，RxJava其实提供了很多便捷的函数来帮助我们减少代码。

 首先来看看如何简化Observable对象的创建过程。RxJava内置了很多简化创建Observable对象的函数，比如Observable.just就是用来创建只发出一个事件就结束的Observable对象，上面创建Observable对象的代码可以简化为一行

```java
Observable<String> myObservable = Observable.just("Hello, world!");
```

接下来看看如何简化Subscriber，上面的例子中，我们其实并不关心OnComplete和OnError，我们只需要在onNext的时候做一些处理，这时候就可以使用Action1类:

```Java
Action1<String> onNextAction = new Action1<String>() {  
    @Override  
    public void call(String s) {  
        System.out.println(s);  
    }  
}; 
```

这里要注意，要用简化的必须和简化的对应，如果一个简化一个不简化，则不能完成调用。
这是由于内部机制决定的。
因为不简化的时候Observabler调用onNext()和onCompleted(),onError()和Subscriber中是对应的。而简化的时候Observabler的just()和Action1中的call()方法是对应的。

```java
 //简化事件源
 Observable<String> justObservable = Observable.just("hello just observable");
//简化订阅者
 Action1<String> justAction = new Action1<String>() {
     @Override
     public void call(String s) {
         Log.d(TAG,"justAction.call(),s:" +s);
     }
 };
 //关联事件
 justObservable.subscribe(justAction);
```

subscribe方法有一个重载版本，接受三个Action1类型的参数，分别对应OnNext，OnComplete， OnError函数。

```java
myObservable.subscribe(onNextAction, onErrorAction, onCompleteAction); 
```

这里我们并不关心onError和onComplete，所以只需要第一个参数就可以

```Java
myObservable.subscribe(onNextAction);  
// Outputs "Hello, world!"
```

上面的代码最终可以写成这样

```java
Observable.just("Hello, world!")  
    .subscribe(new Action1<String>() {  
        @Override  
        public void call(String s) {  
              System.out.println(s);  
        }  
    });  
```

使用java8的lambda可以使代码更简洁

```java
Observable.just("Hello, world!")  
    .subscribe(s -> System.out.println(s));  
```

Android开发中，强烈推荐使用[retrolambda](https://github.com/evant/gradle-retrolambda)这个gradle插件，这样你就可以在你的代码中使用lambda了。

### 1.2、变换

让我们做一些更有趣的事情吧！ 

比如我想在hello world中加上我的签名，你可能会想到去修改Observable对象：

```Java
Observable.just("Hello, world! -Dan")  
    .subscribe(s -> System.out.println(s));
```

如果你能够改变Observable对象，这当然是可以的，

但是如果你不能修改Observable对象呢？

比如Observable对象是第三方库提供的？

比如我的Observable对象被多个Subscriber订阅，但是我只想在对某个订阅者做修改呢？

 那么在Subscriber中对事件进行修改怎么样呢？比如下面的代码：

```java
Observable.just("Hello, world!")  
    .subscribe(s -> System.out.println(s + " -Dan"));
```

这种方式仍然不能让人满意，因为我希望我的Subscribers越轻量越好，因为我有可能会在mainThread中运行subscriber。另外，根据响应式函数编程的概念，Subscribers更应该做的事情是“响应”，响应Observable发出的事件，而不是去修改。如果我能在某些中间步骤中对“Hello World！”进行变换是不是很酷？

**操作符（Operators)**

操作符就是为了解决对Observable对象的变换的问题，操作符用于在Observable和最终的Subscriber之间修改Observable发出的事件。RxJava提供了很多很有用的操作符。
比如map操作符，就是用来把把一个事件转换为另一个事件的。

```java
Observable.just("Hello, world!")  
  .map(new Func1<String, String>() {  
      @Override  
      public String call(String s) {  
          return s + " -Dan";  
      }  
  })  
  .subscribe(s -> System.out.println(s));  
```

使用lambda可以简化为

```java
Observable.just("Hello, world!")  
    .map(s -> s + " -Dan")  
    .subscribe(s -> System.out.println(s));  
```

是不是很酷？map()操作符就是用于变换Observable对象的，map操作符返回一个Observable对象，这样就可以实现链式调用，在一个Observable对象上多次使用map操作符，最终将最简洁的数据传递给Subscriber对象。

```Java
/**
 * 测试Rxjava的操作符
 */
private void testOperator() {
    Observable.just("hell operator!")
            //变换
            .map(new Func1<String, String>() {
                @Override
                public String call(String s) {
                    return s + "--pix";
                }
            })
            .subscribe(new Action1<String>() {
                @Override
                public void call(String s) {
                    Log.d(TAG, "testOperator.call(),s:" + s);
                }
            });
}
```

**map操作符进阶**
map操作符更有趣的一点是它不必返回Observable对象返回的类型，你可以使用map操作符返回一个发出新的数据类型的observable对象。 比如上面的例子中，subscriber并不关心返回的字符串，而是想要字符串的hash值:

```java
Observable.just("Hello, world!")  
    .map(new Func1<String, Integer>() {  
        @Override  
        public Integer call(String s) {  
            return s.hashCode();  
        }  
    })  
    .subscribe(i -> System.out.println(Integer.toString(i)));  
```

```java
//返回值变换，观察者和订阅者值不同的传递,eg.取得字符串哈希值
Observable.just("hello")
        .map(new Func1<String, Integer>() {
            @Override
            public Integer call(String s) {
                return s.hashCode();
            }
        })
        .subscribe(new Action1<Integer>() {
            @Override
            public void call(Integer integer) {
                Log.d(TAG,"testOperator(),String hashCode:" + integer);
            }
        });
```

很有趣吧？我们初始的Observable返回的是字符串，最终的Subscriber收到的却是Integer，当然使用lambda可以进一步简化代码：

```java
Observable.just("Hello, world!")  
    .map(s -> s.hashCode())  
    .subscribe(i -> System.out.println(Integer.toString(i)));
```

前面说过，Subscriber做的事情越少越好，我们再增加一个map操作符

```java
Observable.just("Hello, world!")  
    .map(s -> s.hashCode())  
    .map(i -> Integer.toString(i))  
    .subscribe(s -> System.out.println(s)); 
```

不服？
是不是觉得我们的例子太简单，不足以说服你？你需要明白下面的两点:

  1.Observable和Subscriber可以做任何事情 Observable可以是一个数据库查询，Subscriber用来显示查询结果；Observable可以是屏幕上的点击事件，Subscriber用来响应点击事件；Observable可以是一个网络请求，Subscriber用来显示请求结果。  

2.Observable和Subscriber是独立于中间的变换过程的。 在Observable和Subscriber中间可以增减任何数量的map。整个系统是高度可组合的，操作数据是一个很简单的过程。

## 2、操作符

### 2.1、map操作符

在第一篇blog中，我介绍了RxJava的一些基础知识，同时也介绍了map()操作符。当然如果你并没有意愿去使用RxJava我一点都不诧异，毕竟才接触了这么点。看完这篇blog，我相信你肯定想立即在你的项目中使用RxJava了，这篇blog将介绍许多RxJava中的操作符，RxJava的强大性就来自于它所定义的操作符。

首先先看一个例子：
准备工作
假设我有这样一个方法： 这个方法根据输入的字符串返回一个网站的url列表（啊哈，搜索引擎）

```java
Observable<List<String>> query(String text);   
```

现在我希望构建一个健壮系统，它可以查询字符串并且显示结果。

根据上一篇blog的内容，我们可能会写出下面的代码：

```java
query("Hello, world!")  
    .subscribe(urls -> {  
        for (String url : urls) {  
            System.out.println(url);  
        }  
    }); 
```

这种代码当然是不能容忍的，因为上面的代码使我们丧失了变化数据流的能力。

一旦我们想要更改每一个URL，只能在Subscriber中来做。我们竟然没有使用如此酷的map()操作符！！！
当然，我可以使用map操作符，map的输入是urls列表，处理的时候还是要for each遍历，一样很蛋疼。 万幸，还有Observable.from()方法，它接收一个集合作为输入，然后每次输出一个元素给subscriber:

```java
Observable.from("url1", "url2", "url3")  
    .subscribe(url -> System.out.println(url)); 
```

我们来把这个方法使用到刚才的场景：

```Java
query("Hello, world!")  
    .subscribe(urls -> {  
        Observable.from(urls)  
            .subscribe(url -> System.out.println(url));  
    });  
```

写成完全代码：

```Java
/**
 * 高级操作符测试
 */
private void testOperator2() {
    //多个输入多个输出
    Observable.just("aa", "bb", "cc").subscribe(new Action1<String>() {
        @Override
        public void call(String s) {
            Log.d(TAG, "testOperator2(),s:" + s);
        }
    });

    //嵌套式
    Observable.just("hello world").subscribe(new Action1<String>() {
        @Override
        public void call(String s) {
            List<String> list = new ArrayList<String>();
            list.add(s + "aaa");
            list.add(s + "bbb");
            list.add(s + "ccc");
            list.add(s + "ddd");
            Observable.from(list).subscribe(new Action1<String>() {
                @Override
                public void call(String s) {
                    Log.d(TAG, "testOperator2(),s:" + s);
                }
            });
        }
    });
}
```

虽然去掉了for each循环，但是代码依然看起来很乱。多个嵌套的subscription不仅看起来很丑，难以修改，更严重的是它会破坏某些我们现在还没有讲到的RxJava的特性。

### 2.2、flatMap()操作符

救星来了,他就是flatMap()。
Observable.flatMap()接收一个Observable的输出作为输入，同时输出另外一个Observable。直接看代码：

```Java
query("Hello, world!")  
    .flatMap(new Func1<List<String>, Observable<String>>() {  
        @Override  
        public Observable<String> call(List<String> urls) {  
            return Observable.from(urls);  
        }  
    })  
    .subscribe(url -> System.out.println(url)); 
```

这里我贴出了整个的函数代码，以方便你了解发生了什么，使用lambda可以大大简化代码长度

```Java
query("Hello, world!")  
    .flatMap(urls -> Observable.from(urls))  
    .subscribe(url -> System.out.println(url)); 
```

flatMap()是不是看起来很奇怪？为什么它要返回另外一个Observable呢？理解flatMap的关键点在于，flatMap输出的新的Observable正是我们在Subscriber想要接收的。现在Subscriber不再收到List<String>，而是收到一些列单个的字符串，就像Observable.from()的输出一样。

```Java
//展平测试
Observable.just("hello world")
    .flatMap(new Func1<String, Observable<String>>() {
        @Override
        public Observable<String> call(String s) {
            List<String> list = new ArrayList<String>();
            list.add(s + "aaa");
            list.add(s + "bbb");
            list.add(s + "ccc");
            list.add(s + "ddd");
            return Observable.from(list);
        }
    })
    .subscribe(new Action1<String>() {
    @Override
    public void call(String s) {
        Log.d(TAG,"flatMap(),s:" +s);
    }
});
```

这部分也是我当初学RxJava的时候最难理解的部分，一旦我突然领悟了，RxJava的很多疑问也就一并解决了。

flatMap()实在不能更赞了，它可以返回任何它想返回的Observable对象。
比如下面的方法：

```Java
// 返回网站的标题，如果404了就返回null  
Observable<String> getTitle(String URL);  
```

接着前面的例子，现在我不想打印URL了，而是要打印收到的每个网站的标题。问题来了，我的方法每次只能传入一个URL，并且返回值不是一个String，而是一个输出String的Observabl对象。使用flatMap()可以简单的解决这个问题。

```java
query("Hello, world!")  
    .flatMap(urls -> Observable.from(urls))  
    .flatMap(new Func1<String, Observable<String>>() {  
        @Override  
        public Observable<String> call(String url) {  
            return getTitle(url);  
        }  
    })  
    .subscribe(title -> System.out.println(title));
```

使用lambda:

```java
query("Hello, world!")  
    .flatMap(urls -> Observable.from(urls))  
    .flatMap(url -> getTitle(url))  
    .subscribe(title -> System.out.println(title)); 
```

是不是感觉很不可思议？我竟然能将多个独立的返回Observable对象的方法组合在一起！帅呆了！ 不止这些，我还将两个API的调用组合到一个链式调用中了。我们可以将任意多个API调用链接起来。大家应该都应该知道同步所有的API调用，然后将所有API调用的回调结果组合成需要展示的数据是一件多么蛋疼的事情。这里我们成功的避免了callback hell（多层嵌套的回调，导致代码难以阅读维护）。现在所有的逻辑都包装成了这种简单的响应式调用。

### 2.3、其他操作符

目前为止，我们已经接触了两个操作符，RxJava中还有更多的操作符，那么我们如何使用其他的操作符来改进我们的代码呢？

getTitle()返回null如果url不存在。我们不想输出"null"，那么我们可以从返回的title列表中过滤掉null值！

```java
query("Hello, world!")  
    .flatMap(urls -> Observable.from(urls))  
    .flatMap(url -> getTitle(url))  
    .filter(title -> title != null)  
    .subscribe(title -> System.out.println(title));
```

filter()输出和输入相同的元素，并且会过滤掉那些不满足检查条件的。

如果我们只想要最多5个结果：

```java
query("Hello, world!")  
    .flatMap(urls -> Observable.from(urls))  
    .flatMap(url -> getTitle(url))  
    .filter(title -> title != null)  
    .take(5)  
    .subscribe(title -> System.out.println(title));  
```

take()输出最多指定数量的结果。

如果我们想在打印之前，把每个标题保存到磁盘：

```java
query("Hello, world!")  
    .flatMap(urls -> Observable.from(urls))  
    .flatMap(url -> getTitle(url))  
    .filter(title -> title != null)  
    .take(5)  
    .doOnNext(title -> saveTitle(title))  
    .subscribe(title -> System.out.println(title));  
```

doOnNext()允许我们在每次输出一个元素之前做一些额外的事情，比如这里的保存标题。  看到这里操作数据流是多么简单了么。你可以添加任意多的操作，并且不会搞乱你的代码。  RxJava包含了大量的操作符。操作符的数量是有点吓人，但是很值得你去挨个看一下，这样你可以知道有哪些操作符可以使用。弄懂这些操作符可能会花一些时间，但是一旦弄懂了，你就完全掌握了RxJava的威力。  你甚至可以编写自定义的操作符！这篇blog不打算将自定义操作符，如果你想的话，清自行Google吧。
好吧，你是一个怀疑主义者，并且还很难被说服，那为什么你要关心这些操作符呢？  因为操作符可以让你对数据流做任何操作。  将一系列的操作符链接起来就可以完成复杂的逻辑。代码被分解成一系列可以组合的片段。这就是响应式函数编程的魅力。用的越多，就会越多的改变你的编程思维。

另外，RxJava也使我们处理数据的方式变得更简单。在最后一个例子里，我们调用了两个API，对API返回的数据进行了处理，然后保存到磁盘。但是我们的Subscriber并不知道这些，它只是认为自己在接收一个Observable<String>对象。良好的封装性也带来了编码的便利！

在第三部分中，我将介绍RxJava的另外一些很酷的特性，比如错误处理和并发，这些特性并不会直接用来处理数据。

## 3、响应式编程

在第一篇中，我介绍了RxJava的基础知识。第二篇中，我向你展示了操作符的强大。但是你可能仍然没被说服。这篇里面，我讲向你展示RxJava的其他的一些好处，相信这篇足够让你去使用Rxjava.

3.1、错误处理

  到目前为止，我们都没怎么介绍onComplete()和onError()函数。这两个函数用来通知订阅者，被观察的对象将停止发送数据以及为什么停止（成功的完成或者出错了）。

下面的代码展示了怎么使用这两个函数：

```java
Observable.just("Hello, world!")
    .map(s -> potentialException(s))
    .map(s -> anotherPotentialException(s))
    .subscribe(new Subscriber<String>() {
        @Override
        public void onNext(String s) { System.out.println(s); }

        @Override
        public void onCompleted() { System.out.println("Completed!"); }

        @Override
        public void onError(Throwable e) { System.out.println("Ouch!"); }
    });
```

代码中的potentialException() 和 anotherPotentialException()有可能会抛出异常。

每一个Observerable对象在终结的时候都会调用onCompleted()或者onError()方法，

所以Demo中会打印”Completed!”或者”Ouch!”。
这种模式有以下几个优点：
1.只要有异常发生onError()一定会被调用
这极大的简化了错误处理。只需要在一个地方处理错误即可以。
2.操作符不需要处理异常
将异常处理交给订阅者来做，Observerable的操作符调用链中一旦有一个抛出了异常，就会直接执行onError()方法。
知道什么时候任务结束能够帮助简化代码的流程。（虽然有可能Observable对象永远不会结束）
我觉得这种错误处理方式比传统的错误处理更简单。传统的错误处理中，通常是在每个回调中处理错误。这不仅导致了重复的代码，并且意味着每个回调都必须知道如何处理错误，你的回调代码将和调用者紧耦合在一起。
使用RxJava，Observable对象根本不需要知道如何处理错误！操作符也不需要处理错误状态-一旦发生错误，就会跳过当前和后续的操作符。所有的错误处理都交给订阅者来做。

### 3.2、调度器

假设你编写的Android app需要从网络请求数据（感觉这是必备的了，还有单机么？）。网络请求需要花费较长的时间，因此你打算在另外一个线程中加载数据。那么问题来了！
编写多线程的Android应用程序是很难的，因为你必须确保代码在正确的线程中运行，否则的话可能会导致app崩溃。最常见的就是在非主线程更新UI。
使用RxJava，你可以使用subscribeOn()指定观察者代码运行的线程，使用observerOn()指定订阅者运行的线程：

```java
myObservableServices.retrieveImage(url)
        .subscribeOn(Schedulers.io())
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe(bitmap -> myImageView.setImageBitmap(bitmap));
```

是不是很简单？任何在我的Subscriber前面执行的代码都是在I/O线程中运行。最后，操作view的代码在主线程中运行.
最棒的是我可以把subscribeOn()和observerOn()添加到任何Observable对象上。这两个也是操作符！。我不需要关心Observable对象以及它上面有哪些操作符。仅仅运用这两个操作符就可以实现在不同的线程中调度。
如果使用AsyncTask或者其他类似的，我将不得不仔细设计我的代码，找出需要并发执行的部分。使用RxJava，我可以保持代码不变，仅仅在需要并发的时候调用这两个操作符就可以。

### 3.3、订阅（Subscriptions）

当调用Observable.subscribe()，会返回一个Subscription对象。这个对象代表了被观察者和订阅者之间的联系。

```java
ubscription subscription = Observable.just("Hello, World!")
        .subscribe(s -> System.out.println(s));
```

你可以在后面使用这个Subscription对象来操作被观察者和订阅者之间的联系.

```java
subscription.unsubscribe();
        System.out.println("Unsubscribed=" + subscription.isUnsubscribed());
// Outputs "Unsubscribed=true"
```

RxJava的另外一个好处就是它处理unsubscribing的时候，会停止整个调用链。如果你使用了一串很复杂的操作符，调用unsubscribe将会在他当前执行的地方终止。不需要做任何额外的工作！

记住这个系列仅仅是对RxJava的一个入门介绍。RxJava中有更多的我没介绍的功能等你探索（比如backpressure）。当然我也不是所有的代码都使用响应式的方式–仅仅当代码复杂到我想将它分解成简单的逻辑的时候，我才使用响应式代码。

最初，我的计划是这篇文章作为这个系列的总结，但是我收到许多请求我介绍在Android中使用RxJava，所以你可以继续阅读第四篇了。我希望这个介绍能让你开始使用RxJava。如果你想学到更多，我建议你阅读RxJava的官方wiki。

## 4、RxAndroid

RxAndroid是RxJava的一个针对Android平台的扩展。它包含了一些能够简化Android开发的工具。

首先，AndroidSchedulers提供了针对Android的线程系统的调度器。需要在UI线程中运行某些代码？很简单，只需要使用AndroidSchedulers.mainThread():

```java
retrofitService.getImage(url)
        .subscribeOn(Schedulers.io())
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe(bitmap -> myImageView.setImageBitmap(bitmap));
```

如果你已经创建了自己的Handler，你可以使用HandlerThreadScheduler1将一个调度器链接到你的handler上。
接着要介绍的就是AndroidObservable，它提供了跟多的功能来配合Android的生命周期。

bindActivity()和bindFragment()方法默认使用AndroidSchedulers.mainThread()来执行观察者代码，这两个方法会在Activity或者Fragment结束的时候通知被观察者停止发出新的消息。

```java
AndroidObservable.bindActivity(this, retrofitService.getImage(url))
        .subscribeOn(Schedulers.io())
        .subscribe(bitmap -> myImageView.setImageBitmap(bitmap);
```

我自己也很喜欢AndroidObservable.fromBroadcast()方法，它允许你创建一个类似BroadcastReceiver的Observable对象。下面的例子展示了如何在网络变化的时候被通知到：

```Java
IntentFilter filter = new IntentFilter(ConnectivityManager.CONNECTIVITY_ACTION);
        AndroidObservable.fromBroadcast(context, filter)
        .subscribe(intent -> handleConnectivityChange(intent));
```

最后要介绍的是ViewObservable,使用它可以给View添加了一些绑定。如果你想在每次点击view的时候都收到一个事件，可以使用ViewObservable.clicks()，或者你想监听TextView的内容变化，可以使用ViewObservable.text()。

```java
ViewObservable.clicks(mCardNameEditText, false)
        .subscribe(view -> handleClick(view));
```

大名鼎鼎的Retrofit库内置了对RxJava的支持。通常调用发可以通过使用一个Callback对象来获取异步的结果：

```java
@GET("/user/{id}/photo") 
void getUserPhoto(@Path("id") int id, Callback<Photo> cb);
```

使用RxJava，你可以直接返回一个Observable对象。

```Java
@GET("/user/{id}/photo") 
Observable<Photo> getUserPhoto(@Path("id") int id);
```

现在你可以随意使用Observable对象了。你不仅可以获取数据，还可以进行变换。 

Retrofit对Observable的支持使得它可以很简单的将多个REST请求结合起来。比如我们有一个请求是获取照片的，还有一个请求是获取元数据的，我们就可以将这两个请求并发的发出，并且等待两个结果都返回之后再做处理：

```
Observable.zip(
        service.getUserPhoto(id),
        service.getPhotoMetadata(id),
        (photo, metadata) -> createPhotoWithData(photo, metadata))
        .subscribe(photoWithData -> showPhoto(photoWithData));
```

在第二篇里我展示过一个类似的例子（使用flatMap()）。这里我只是想展示以下使用RxJava+Retrofit可以多么简单地组合多个REST请求。

遗留代码，运行极慢的代码

Retrofit可以返回Observable对象，但是如果你使用的别的库并不支持这样怎么办？或者说一个内部的内码，你想把他们转换成Observable的？有什么简单的办法没？

绝大多数时候Observable.just()和 Observable.from() 能够帮助你从遗留代码中创建 Observable 对象:

````java
private Object oldMethod() { ... }

public Observable<Object> newMethod() {
        return Observable.just(oldMethod());
        }
````

上面的例子中如果oldMethod()足够快是没有什么问题的，但是如果很慢呢？调用oldMethod()将会阻塞住他所在的线程。 

为了解决这个问题，可以参考我一直使用的方法–使用defer()来包装缓慢的代码：

```java
private Object slowBlockingMethod() { ... }

public Observable<Object> newMethod() {
        return Observable.defer(() -> Observable.just(slowBlockingMethod()));
        }
```

**生命周期**
我把最难的不分留在了最后。如何处理Activity的生命周期？

主要就是两个问题： 

 1.在configuration改变（比如转屏）之后继续之前的Subscription。
比如你使用Retrofit发出了一个REST请求，接着想在listview中展示结果。

如果在网络请求的时候用户旋转了屏幕怎么办？你当然想继续刚才的请求，但是怎么搞？
2.Observable持有Context导致的内存泄露
这个问题是因为创建subscription的时候，以某种方式持有了context的引用，尤其是当你和view交互的时候，这太容易发生！如果Observable没有及时结束，内存占用就会越来越大。  不幸的是，没有银弹来解决这两个问题，但是这里有一些指导方案你可以参考。
第一个问题的解决方案就是使用RxJava内置的缓存机制，这样你就可以对同一个Observable对象执行unsubscribe/resubscribe，却不用重复运行得到Observable的代码。cache() (或者 replay())会继续执行网络请求（甚至你调用了unsubscribe也不会停止）。这就是说你可以在Activity重新创建的时候从cache()的返回值中创建一个新的Observable对象。

```java
Observable<Photo> request = service.getUserPhoto(id).cache();
        Subscription sub = request.subscribe(photo -> handleUserPhoto(photo));

// ...When the Activity is being recreated...
        sub.unsubscribe();

// ...Once the Activity is recreated...
        request.subscribe(photo -> handleUserPhoto(photo));
```

注意，两次sub是使用的同一个缓存的请求。当然在哪里去存储请求的结果还是要你自己来做，和所有其他的生命周期相关的解决方案一延虎，必须在生命周期外的某个地方存储。（retained fragment或者单例等等）。

第二个问题的解决方案就是在生命周期的某个时刻取消订阅。一个很常见的模式就是使用CompositeSubscription来持有所有的Subscriptions，然后在onDestroy()或者onDestroyView()里取消所有的订阅。

```java
private CompositeSubscription mCompositeSubscription
        = new CompositeSubscription();

private void doSomething() {
        mCompositeSubscription.add(
        AndroidObservable.bindActivity(this, Observable.just("Hello, World!"))
        .subscribe(s -> System.out.println(s)));
        }

@Override
protected void onDestroy() {
        super.onDestroy();

        mCompositeSubscription.unsubscribe();
        }
```

你可以在Activity/Fragment的基类里创建一个CompositeSubscription对象，在子类中使用它。
注意! 一旦你调用了 CompositeSubscription.unsubscribe()，这个CompositeSubscription对象就不可用了, 如果你还想使用CompositeSubscription，就必须在创建一个新的对象了。
两个问题的解决方案都需要添加额外的代码，如果谁有更好的方案，欢迎告诉我。
总结
RxJava还是一个很新的项目，RxAndroid更是。RxAndroid目前还在活跃开发中，也没有多少好的例子。我打赌一年之后我的一些建议就会被看做过时了。



# 二、可能是东半球最全的RxJava使用场景小结

URL:http://blog.csdn.net/theone10211024/article/details/50435325

## 1、Scheduler线程切换

这种场景经常会在“后台线程取数据，主线程展示”的模式中看见

```Java
Observable.just(1, 2, 3, 4)  
            .subscribeOn(Schedulers.io()) // 指定 subscribe() 发生在 IO 线程  
            .observeOn(AndroidSchedulers.mainThread()) // 指定 Subscriber 的回调发生在主线程  
            .subscribe(new Action1<Integer>() {  
        @Override  
        public void call(Integer number) {  
            Log.d(tag, "number:" + number);  
        }  
    });  
```

## 2、使用debounce做textSearch

用简单的话讲就是当N个结点发生的时间太靠近（即发生的时间差小于设定的值T），debounce就会自动过滤掉前N-1个结点。

比如在做百度地址联想的时候，可以使用debounce减少频繁的网络请求。避免每输入（删除）一个字就做一次联想

```java
RxTextView.textChangeEvents(inputEditText)  
      .debounce(400, TimeUnit.MILLISECONDS)   
      .observeOn(AndroidSchedulers.mainThread())  
      .subscribe(new Observer<TextViewTextChangeEvent>() {  
    @Override  
    public void onCompleted() {  
        log.d("onComplete");  
    }  
  
    @Override  
    public void onError(Throwable e) {  
        log.d("Error");  
    }  
  
    @Override  
    public void onNext(TextViewTextChangeEvent onTextChangeEvent) {  
        log.d(format("Searching for %s", onTextChangeEvent.text().toString()));  
    }  
});  
```

## 3、Retrofit结合RxJava做网络请求框架

这里不作详解，具体的介绍可以看扔物线的[这篇文章](http://gank.io/post/560e15be2dca930e00da1083)，对RxJava的入门者有很大的启发。其中也讲到了RxJava和Retrofit如何结合来实现更简洁的代码

## 4、RxJava代替EventBus进行数据传递：RxBus

注意：RxBus并不是一个库，而是一种模式，是使用了RxJava的思想来达到EventBus的数据传递效果。[这篇文章](http://nerds.weddingpartyapp.com/tech/2014/12/24/implementing-an-event-bus-with-rxjava-rxbus/)把RxBus讲的比较详细。

## 5、使用combineLatest合并最近N个结点

例如：注册的时候所有输入信息（邮箱、密码、电话号码等）合法才点亮注册按钮。

```java
Observable<CharSequence> _emailChangeObservable = RxTextView.textChanges(_email).skip(1);  
Observable<CharSequence> _passwordChangeObservable = RxTextView.textChanges(_password).skip(1);  
Observable<CharSequence>   _numberChangeObservable = RxTextView.textChanges(_number).skip(1);  
  
Observable.combineLatest(_emailChangeObservable,  
              _passwordChangeObservable,  
              _numberChangeObservable,  
              new Func3<CharSequence, CharSequence, CharSequence, Boolean>() {  
                  @Override  
                  public Boolean call(CharSequence newEmail,  
                                      CharSequence newPassword,  
                                      CharSequence newNumber) {  
  
                      Log.d("xiayong",newEmail+" "+newPassword+" "+newNumber);  
                      boolean emailValid = !isEmpty(newEmail) &&  
                                           EMAIL_ADDRESS.matcher(newEmail).matches();  
                      if (!emailValid) {  
                          _email.setError("Invalid Email!");  
                      }  
  
                      boolean passValid = !isEmpty(newPassword) && newPassword.length() > 8;  
                      if (!passValid) {  
                          _password.setError("Invalid Password!");  
                      }  
  
                      boolean numValid = !isEmpty(newNumber);  
                      if (numValid) {  
                          int num = Integer.parseInt(newNumber.toString());  
                          numValid = num > 0 && num <= 100;  
                      }  
                      if (!numValid) {  
                          _number.setError("Invalid Number!");  
                      }  
  
                      return emailValid && passValid && numValid;  
  
                  }  
              })//  
              .subscribe(new Observer<Boolean>() {  
                  @Override  
                  public void onCompleted() {  
                      log.d("completed");  
                  }  
  
                  @Override  
                  public void onError(Throwable e) {  
                     log.d("Error");  
                  }  
  
                  @Override  
                  public void onNext(Boolean formValid) {  
                     _btnValidIndicator.setEnabled(formValid);    
                  }  
              });  
```

## 6、使用merge合并两个数据源。

例如一组数据来自网络，一组数据来自文件，需要合并两组数据一起展示。

```Java
Observable.merge(getDataFromFile(), getDataFromNet())  
              .observeOn(AndroidSchedulers.mainThread())  
              .subscribe(new Subscriber<String>() {  
                  @Override  
                  public void onCompleted() {  
                      log.d("done loading all data");  
                  }  
  
                  @Override  
                  public void onError(Throwable e) {  
                      log.d("error");  
                  }  
  
                  @Override  
                  public void onNext(String data) {  
                      log.d("all merged data will pass here one by one!")  
              });  
```

## 7、使用concat和first做缓存

依次检查memory、disk和network中是否存在数据，任何一步一旦发现数据后面的操作都不执行。

```java
Observable<String> memory = Observable.create(new Observable.OnSubscribe<String>() {  
    @Override  
    public void call(Subscriber<? super String> subscriber) {  
        if (memoryCache != null) {  
            subscriber.onNext(memoryCache);  
        } else {  
            subscriber.onCompleted();  
        }  
    }  
});  
Observable<String> disk = Observable.create(new Observable.OnSubscribe<String>() {  
    @Override  
    public void call(Subscriber<? super String> subscriber) {  
        String cachePref = rxPreferences.getString("cache").get();  
        if (!TextUtils.isEmpty(cachePref)) {  
            subscriber.onNext(cachePref);  
        } else {  
            subscriber.onCompleted();  
        }  
    }  
});  
  
Observable<String> network = Observable.just("network");  
  
//依次检查memory、disk、network  
Observable.concat(memory, disk, network)  
.first()  
.subscribeOn(Schedulers.newThread())  
.subscribe(s -> {  
    memoryCache = "memory";  
    System.out.println("--------------subscribe: " + s);  
}); 
```

## 8、使用timer做定时操作。

当有“x秒后执行y操作”类似的需求的时候，想到使用timer

例如：2秒后输出日志“hello world”，然后结束。

```java
Observable.timer(2, TimeUnit.SECONDS)  
              .subscribe(new Observer<Long>() {  
                  @Override  
                  public void onCompleted() {  
                      log.d ("completed");  
                  }  
  
                  @Override  
                  public void onError(Throwable e) {  
                      log.e("error");  
                  }  
  
                  @Override  
                  public void onNext(Long number) {  
                      log.d ("hello world");  
                  }  
              });  
```

## 9、使用interval做周期性操作。

当有“每隔xx秒后执行yy操作”类似的需求的时候，想到使用interval
例如：每隔2秒输出日志“helloworld”。

```java
Observable.interval(2, TimeUnit.SECONDS)  
         .subscribe(new Observer<Long>() {  
             @Override  
             public void onCompleted() {  
                log.d ("completed");  
             }  
  
             @Override  
             public void onError(Throwable e) {  
                log.e("error");  
             }  
  
             @Override  
             public void onNext(Long number) {  
                log.d ("hello world");  
             }  
         });  
```

## 10、使用throttleFirst防止按钮重复点击

ps：debounce也能达到同样的效果

```java
RxView.clicks(button)  
              .throttleFirst(1, TimeUnit.SECONDS)  
              .subscribe(new Observer<Object>() {  
                  @Override  
                  public void onCompleted() {  
                        log.d ("completed");  
                  }  
  
                  @Override  
                  public void onError(Throwable e) {  
                        log.e("error");  
                  }  
  
                  @Override  
                  public void onNext(Object o) {  
                       log.d("button clicked");  
                  }  
              });  
```

## 11、使用schedulePeriodically做轮询请求

```java
Observable.create(new Observable.OnSubscribe<String>() {  
            @Override  
            public void call(final Subscriber<? super String> observer) {  
  
                Schedulers.newThread().createWorker()  
                      .schedulePeriodically(new Action0() {  
                          @Override  
                          public void call() {  
                              observer.onNext(doNetworkCallAndGetStringResult());  
                          }  
                      }, INITIAL_DELAY, POLLING_INTERVAL, TimeUnit.MILLISECONDS);  
            }  
        }).subscribe(new Action1<String>() {  
            @Override  
            public void call(String s) {  
                log.d("polling….”));  
            }  
        })  

```

## 12、RxJava进行数组、list的遍历

```java
String[] names = {"Tom", "Lily", "Alisa", "Sheldon", "Bill"};  
Observable  
        .from(names)  
        .subscribe(new Action1<String>() {  
            @Override  
            public void call(String name) {  
                log.d(name);  
            }  
        });  
```

## 13、解决嵌套回调（callback hell）问题

```Java
NetworkService.getToken("username", "password")  
    .flatMap(s -> NetworkService.getMessage(s))  
    .subscribe(s -> {  
        System.out.println("message: " + s);  
})
```

## 14、响应式的界面

比如勾选了某个checkbox，自动更新对应的preference

```java
SharedPreferences preferences = PreferenceManager.getDefaultSharedPreferences(this);  
RxSharedPreferences rxPreferences = RxSharedPreferences.create(preferences);  
  
Preference<Boolean> checked = rxPreferences.getBoolean("checked", true);  
  
CheckBox checkBox = (CheckBox) findViewById(R.id.cb_test);  
RxCompoundButton.checkedChanges(checkBox)  
        .subscribe(checked.asAction());  
```

最后，由于个人能力有限，文章难免有疏漏之处，如果您有任何疑议，请让我知道，谢谢！本文所有的例子已经上传到[github上](https://github.com/THEONE10211024/RxJavaSamples) 

致谢：这篇文章的绝大多数例子是从[这里](https://github.com/kaushikgopal/RxJava-Android-Samples)总结的，还有部分例子来自[这里](http://blog.csdn.net/lzyzsd/article/details/50120801)。对作者的无私贡献表示感谢！