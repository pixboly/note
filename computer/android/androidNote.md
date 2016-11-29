> Android笔记

# 一、Android相关知识

## 1、系统分层

![Android系统架构分层](http://note.youdao.com/yws/public/resource/5d0c3b48c9baad78d7560a0a395daa48/xmlnote/7688C14E8BFD431BB6B586B70C3B7E24/23584)

•Framework框架	•library库文件	•Activity活动窗口	•ContentProviders:管理数据库的东西 Content内容	•View System ：控制界面	•Notification Manager ： 通知	•Package Manager：包管理	•Resource Manager :资源管理器	•Location Manager:定位管理器	•XMPP Manager:推送	•NDK: Nature自然的，Natural原生的	•Home：主界面	•Contact：联系人	•Phone:电话	•Browser：浏览器

四层系统架构： Linux Kernel、 Library & Android RunTime、 Framework、 Application



## 2、版本

a

| Platform Version                         | API Level                                | VERSION_CODE             | Notes                                    |
| ---------------------------------------- | ---------------------------------------- | ------------------------ | ---------------------------------------- |
| [Android 4.4](http://developer.android.com/about/versions/android-4.4.html) | [19](http://developer.android.com/sdk/api_diff/19/changes.html) | `KITKAT`                 | [Platform Highlights](http://developer.android.com/about/versions/kitkat.html) |
| [Android 4.3](http://developer.android.com/about/versions/android-4.3.html) | [18](http://developer.android.com/sdk/api_diff/18/changes.html) | `JELLY_BEAN_MR2`         | [Platform Highlights](http://developer.android.com/about/versions/jelly-bean.html) |
| [Android 4.2, 4.2.2](http://developer.android.com/about/versions/android-4.2.html) | [17](http://developer.android.com/sdk/api_diff/17/changes.html) | `JELLY_BEAN_MR1`         | [Platform Highlights](http://developer.android.com/about/versions/jelly-bean.html#android-42) |
| [Android 4.1, 4.1.1](http://developer.android.com/about/versions/android-4.1.html) | [16](http://developer.android.com/sdk/api_diff/16/changes.html) | `JELLY_BEAN`             | [Platform Highlights](http://developer.android.com/about/versions/jelly-bean.html#android-41) |
| [Android 4.0.3, 4.0.4](http://developer.android.com/about/versions/android-4.0.3.html) | [15](http://developer.android.com/sdk/api_diff/15/changes.html) | `ICE_CREAM_SANDWICH_MR1` | [Platform Highlights](http://developer.android.com/about/versions/android-4.0-highlights.html) |
| [Android 4.0, 4.0.1, 4.0.2](http://developer.android.com/about/versions/android-4.0.html) | [14](http://developer.android.com/sdk/api_diff/14/changes.html) | `ICE_CREAM_SANDWICH`     |                                          |
| [Android 3.2](http://developer.android.com/about/versions/android-3.2.html) | [13](http://developer.android.com/sdk/api_diff/13/changes.html) | `HONEYCOMB_MR2`          |                                          |
| [Android 3.1.x](http://developer.android.com/about/versions/android-3.1.html) | [12](http://developer.android.com/sdk/api_diff/12/changes.html) | `HONEYCOMB_MR1`          | [Platform Highlights](http://developer.android.com/about/versions/android-3.1-highlights.html) |
| [Android 3.0.x](http://developer.android.com/about/versions/android-3.0.html) | [11](http://developer.android.com/sdk/api_diff/11/changes.html) | `HONEYCOMB`              | [Platform Highlights](http://developer.android.com/about/versions/android-3.0-highlights.html) |
| [Android 2.3.4Android 2.3.3](http://developer.android.com/about/versions/android-2.3.3.html) | [10](http://developer.android.com/sdk/api_diff/10/changes.html) | `GINGERBREAD_MR1`        | [Platform Highlights](http://developer.android.com/about/versions/android-2.3-highlights.html) |
| [Android 2.3.2Android 2.3.1Android 2.3](http://developer.android.com/about/versions/android-2.3.html) | [9](http://developer.android.com/sdk/api_diff/9/changes.html) | `GINGERBREAD`            |                                          |
| [Android 2.2.x](http://developer.android.com/about/versions/android-2.2.html) | [8](http://developer.android.com/sdk/api_diff/8/changes.html) | `FROYO`                  | [Platform Highlights](http://developer.android.com/about/versions/android-2.2-highlights.html) |
| [Android 2.1.x](http://developer.android.com/about/versions/android-2.1.html) | [7](http://developer.android.com/sdk/api_diff/7/changes.html) | `ECLAIR_MR1`             | [Platform Highlights](http://developer.android.com/about/versions/android-2.0-highlights.html) |
| [Android 2.0.1](http://developer.android.com/about/versions/android-2.0.1.html) | [6](http://developer.android.com/sdk/api_diff/6/changes.html) | `ECLAIR_0_1`             |                                          |
| [Android 2.0](http://developer.android.com/about/versions/android-2.0.html) | [5](http://developer.android.com/sdk/api_diff/5/changes.html) | `ECLAIR`                 |                                          |
| [Android 1.6](http://developer.android.com/about/versions/android-1.6.html) | [4](http://developer.android.com/sdk/api_diff/4/changes.html) | `DONUT`                  | [Platform Highlights](http://developer.android.com/about/versions/android-1.6-highlights.html) |
| [Android 1.5](http://developer.android.com/about/versions/android-1.5.html) | [3](http://developer.android.com/sdk/api_diff/3/changes.html) | `CUPCAKE`                | [Platform Highlights](http://developer.android.com/about/versions/android-1.5-highlights.html) |
| [Android 1.1](http://developer.android.com/about/versions/android-1.1.html) | 2                                        | `BASE_1_1`               |                                          |
| Android 1.0                              | 1                                        | `BASE`                   |                                          |



| Version    | Code name                                | Release date      | API level | DVM/ART                                  | Distribution                             | First devices to run version             |
| ---------- | ---------------------------------------- | ----------------- | --------- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| **7.1**    | [Nougat](https://en.wikipedia.org/wiki/Android_Nougat) | October 4, 2016   | 25        | [ART](https://en.wikipedia.org/wiki/Android_Runtime) | less than 0.1%                           |                                          |
| **7.0**    | [Nougat](https://en.wikipedia.org/wiki/Android_Nougat) | August 22, 2016   | 24        | ART                                      | 0.3%                                     | [Nexus 5X](https://en.wikipedia.org/wiki/Nexus_5X), [Nexus 6P](https://en.wikipedia.org/wiki/Nexus_6P) |
| **6.0**    | [Marshmallow](https://en.wikipedia.org/wiki/Android_Marshmallow) | October 5, 2015   | 23        | ART                                      | 24.0%                                    | [Nexus 5X](https://en.wikipedia.org/wiki/Nexus_5X), [Nexus 6P](https://en.wikipedia.org/wiki/Nexus_6P) |
| **5.1**    | [Lollipop](https://en.wikipedia.org/wiki/Android_Lollipop) | March 9, 2015     | 22        | ART                                      | 22.8%                                    | [Android One](https://en.wikipedia.org/wiki/Android_One) |
| **5.0**    | November 3, 2014                         | 21                | ART 2.1.0 | 11.3%                                    | [Nexus 6](https://en.wikipedia.org/wiki/Nexus_6) |                                          |
| **4.4**    | [KitKat](https://en.wikipedia.org/wiki/Android_KitKat) | October 31, 2013  | 19        | [DVM](https://en.wikipedia.org/wiki/Dalvik_(software)) (and ART 1.6.0) | 25.2%                                    | [Nexus 5](https://en.wikipedia.org/wiki/Nexus_5) |
| **4.3**    | [Jelly Bean](https://en.wikipedia.org/wiki/Android_Jelly_Bean) | July 24, 2013     | 18        | DVM                                      | 2.0%                                     | [Nexus 7 2013](https://en.wikipedia.org/wiki/Nexus_7_(2013)) |
| **4.2**    | November 13, 2012                        | 17                | DVM       | 6.8%                                     | [Nexus 4](https://en.wikipedia.org/wiki/Nexus_4), [Nexus 10](https://en.wikipedia.org/wiki/Nexus_10) |                                          |
| **4.1**    | July 9, 2012                             | 16                | DVM       | 4.9%                                     | [Nexus 7](https://en.wikipedia.org/wiki/Nexus_7_(2012)) |                                          |
| **4.0**    | [Ice Cream Sandwich](https://en.wikipedia.org/wiki/Android_Ice_Cream_Sandwich) | December 16, 2011 | 15        | DVM                                      | 1.3%                                     | [Galaxy Nexus](https://en.wikipedia.org/wiki/Galaxy_Nexus) |
| **2.3.3+** | [Gingerbread](https://en.wikipedia.org/wiki/Android_Gingerbread) | February 9, 2011  | 10        | DVM 1.4.0                                | 1.3%                                     | [Nexus S](https://en.wikipedia.org/wiki/Nexus_S) |
| **2.2**    | [Froyo](https://en.wikipedia.org/wiki/Android_Froyo) | May 20, 2010      | 8         | DVM                                      | 0.1%                                     | [Droid 2](https://en.wikipedia.org/wiki/Droid_2) |

## 3、常见屏幕分辨率

```java
4:3
VGA     640*480 (Video Graphics Array)
QVGA  320*240 (Quarter VGA)
HVGA  480*320 (Half-size VGA)
SVGA  800*600 (Super VGA)

5:3
WVGA  800*480 (Wide VGA)

16:9
FWVGA 854*480 (Full Wide VGA)
HD        1920*1080 High Definition
QHD     960*540
720p    1280*720  标清
1080p  1920*1080 高清

手机:
iphone 4/4s    960*640 (3:2)
iphone5         1136*640
小米1             854*480(FWVGA)
小米2             1280*720
```



## 4、分辨率及对应的DPI

```java
"HVGA    mdpi"
"WVGA   hdpi "
"FWVGA hdpi "
"QHD      hdpi "
"720P     xhdpi"
"1080P   xxhdpi "
```



## 5、开发环境相关

### 5.1、Eclipse

-  设置字符编码

 General——Editors——TextEditors——Spelling——utf-8

General——Workspace——Other——UTF-8二、线程与异步任务

# 二、线程相关

## 1、异步任务AsyncTask

### 1.1、引言

- 开发Android应用时必须遵守`单线程模型`的原则： 

Android UI操作并不是线程安全的,并且这些操作必须在UI线程中执行。

- 单线程模型中始终要记住两条法则：

1). 不要阻塞UI线程 ；
2). 确保只在UI线程中访问Android UI控件。

        当一个程序第一次启动时，Android会同时启动一个对应的主线程(Main Thread)，主线程主要负责处理与UI相关的事件，如：用户的按键事件，用户接触屏幕的事件以及屏幕绘图事件，并把相关的事件分发到对应的组件进行处理。所以主线程通常又被叫做UI线程。

- Android4.0以上版本中，主线程中不允许访问网络。涉及到网络操作的程序一般都是需要开一个新线程完成网络访问。但是在获得页面数据后，又不能将数据返回到UI界面中 。因为子线程（Worker Thread）不能直接访问UI线程中的成员，也就是说没有办法对UI界面上的内容进行操作，如果操作，将抛出异常：CalledFromWrongThreadException。

其实，android提供了几种在其他线程中访问UI线程的方法： 

- Activity.runOnUiThread( Runnable ) 
- View.post( Runnable ) 
- View.postDelayed( Runnable, long ) 
- Handler消息传递机制（后续课程中讲解）

        这些类或方法会使代码很复杂很难理解。为了解决这个问题，Android 1.5提供了一个工具类：AsyncTask，它使创建与用户界面长时间交互运行的任务变得更简单。AsyncTask更轻量级一些，适用于简单的异步处理，不需要借助线程和Handler即可实现。 

### 1.2、AsyncTask的代码实现：

1、AsyncTask是抽象类.AsyncTask定义了三种泛型类型 Params，Progress和Result。 

- Params 启动任务执行的输入参数，比如HTTP请求的URL。 一般用String类型；
- Progress 后台任务执行的百分比。 一般用Integer类型；
- Result 后台执行任务最终返回的结果，一般用byte[]或者String。 

2、AsyncTask的执行分为四个步骤，每一步都对应一个回调方法（由应用程序自动调用的方法），开发者需要做的就是实现这些方法。 

1) 定义AsyncTask的子类； 

2) 实现AsyncTask中定义的方法：（可以全部实现，也可以只实现其中一部分） 

- onPreExecute(), 该方法将在执行实际的后台操作前被UI thread调用。可以在该方法中做一些准备工作，如在界面上显示一个进度条。 
- doInBackground(Params...), 将在onPreExecute 方法执行后马上执行，该方法运行在后台线程中。这里将主要负责执行那些很耗时的后台计算工作。可以调用 publishProgress方法来更新实时的任务进度。该方法是抽象方法，子类必须实现。
- onProgressUpdate(Progress...),在publishProgress方法被调用后，UI thread将调用这个方法从而在界面上展示任务的进展情况，例如通过一个进度条进行展示。 
- onPostExecute(Result), 在doInBackground 执行完成后，onPostExecute 方法将被UI thread调用，后台的计算结果将通过该方法传递到UI thread. 

核心代码：

```java
public class MainActivity extends Activity {
    private final static String TAG = "MainActivity";
    private String urlString = "http://www.baidu.com/";
    private TextView text_main_info;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        text_main_info = (TextView) findViewById(R.id.text_main_info);
        // 调用异步任务，执行网络访问
        new MyTask(this).execute(urlString);
    }
    class MyTask extends AsyncTask<String, Void, byte[]> {
        private ProgressDialog pDialog;
        private Context context = null;
        // 构造方法，初始化进度对话框
        public MyTask(Context context) {
            this.context = context;
            pDialog = new ProgressDialog(context);
            pDialog.setIcon(R.drawable.ic_launcher);
            pDialog.setTitle("提示：");
            pDialog.setMessage("数据加载中。。。");
        }
        // 事先执行方法中显示进度对话框
        @Override
        protected void onPreExecute() {
            pDialog.show();
            super.onPreExecute();
        }
        // 进度条进度改变方法。一般情况下，可以不写该方法
        @Override
        protected void onProgressUpdate(Void... values) {
            // TODO Auto-generated method stub
            super.onProgressUpdate(values);
        }
        // 后台执行方法，这个方法执行worker Thread异步访问网络，加载数据。该方法中不可以执行任何UI操作。
        @Override
        protected byte[] doInBackground(String... params) {
            BufferedInputStream bis = null;
            ByteArrayOutputStream baos = new ByteArrayOutputStream();
            try {
                URL urlObj = new URL(params[0]);
                HttpURLConnection httpConn = (HttpURLConnection) urlObj
                        .openConnection();
                httpConn.setDoInput(true);
                // httpConn.setDoOutput(true);
                httpConn.setRequestMethod("GET");
                httpConn.connect();
                if (httpConn.getResponseCode() == 200) {
                    bis = new BufferedInputStream(httpConn.getInputStream());
                    byte[] buffer = new byte[1024 * 8];
                    int c = 0;
                    while ((c = bis.read(buffer)) != -1) {
                        baos.write(buffer, 0, c);
                        baos.flush();
                    }
                    // Toast.makeText(context, baos.toByteArray().toString(),
                    // Toast.LENGTH_LONG).show();
                    return baos.toByteArray();
                }
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                try {
                    if (bis != null) {
                        bis.close();
                    }
                    if (baos != null) {
                        baos.close();
                    }
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            return null;
        }
        // 事后方法，这个方法主要作用是执行对主线程中UI的操作。可以实现主线程和子线程之间的数据交互
        @Override
        protected void onPostExecute(byte[] result) {
            super.onPostExecute(result);
            if (result == null) {
                text_main_info.setText("网络异常，加载数据失败！");
            } else {
                text_main_info.setText(new String(result));
            }
            pDialog.dismiss();
        }
    }
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.main, menu);
        return true;
    }
}
```

这里可以学到方法命令的方法：

onXXXX():什么时候执行

doXXXX():执行什么操作

4、为了正确的使用AsyncTask类，以下是几条必须遵守的准则： 

　　1) Task的实例必须在UI thread中创建； 

　　2) execute方法必须在UI thread中调用；

　　3) 不要手动的调用onPreExecute(), onPostExecute(Result)，doInBackground(Params...), onProgressUpdate(Progress...)这几个方法 ；

　　4) 该task只能被执行一次，否则多次调用时将会出现异常 ；

      doInBackground方法和onPostExecute的参数必须对应，这两个参数在AsyncTask声明的泛型参数列表中指定，第一个为doInBackground接受的参数，第二个为显示进度的参数，第第三个为doInBackground返回和onPostExecute传入的参数。

  



## 2、Handler、Looper

### 2.1、引言

Handler、Looper进行网络访问和异步任务AsyncTask的区别？		

•AsyncTack，有连接池，适合于多个任务同时加载的情况		

•Handler、Looper适合于加载少量任务的情况	自己使用了Handler、Looper，没有看源码，对此理解就是，我们启动一个线程完成的我们的任务，可以在多线程中根据网络访问的不同情况，通过Handler类的对象将我们的Messager消息进行传递，此处我们需要定义一个Handler的子类来接受消息完成响应。	其实此处也是策略模式的具体体现，Handler类就是具体的策略，我们将策略交给线程类，线程会根据不同的策略完成我们想要的完成的任务。	引入Handler的主要目的就是解决主线程无法访问网络的问题，线程之间的通信问题。

***子线程没有办法对UI界面上的内容进行操作，如果操作，将抛出异常：CalledFromWrongThreadException为了实现子线程中操作UI界面，Android中引入了Handler消息传递机制。***

常用类：（Handler、Looper、Message、MessageQueue）

- Message：消息，其中包含了消息ID，消息处理对象以及处理的数据等，由MessageQueue统一列队，终由Handler处理。
- Handler：处理者，负责Message的发送及处理。使用Handler时，需要实现handleMessage(Message msg)方法来对特定的Message进行处理，例如更新UI等。

  ​	Handler类的主要作用：（有两个主要作用）

  ​	1）、在工作线程中发送消息；

  ​	2）、在主线程中获取、并处理消息。

- MessageQueue：消息队列，用来存放Handler发送过来的消息，并按照FIFO规则执行。当然，存放Message并非实际意义的保存，而是将Message串联起来的，等待Looper的抽取。
- Looper：消息泵，不断地从MessageQueue中抽取Message执行。因此，一个MessageQueue需要一个Looper
- LooperThread：线程，负责调度整个消息循环，即消息循环的执行场所。

### 2.2、概述Handler 、 Looper 、Message 

URL: (http://blog.csdn.net/lmj623565791/article/details/38377229)

很多人面试肯定都被问到过，请问Android中的Looper , Handler , Message有什么关系？本篇博客目的首先为大家从源码角度介绍3者关系，然后给出一个容易记忆的结论。

这三者都与Android异步消息处理线程相关的概念。那么什么叫异步消息处理线程呢？ 异步消息处理线程启动后会进入一个无限的循环体之中，每循环一次，从其内部的消息队列中取出一个消息，然后回调相应的消息处理函数，执行完成一个消息后则继续循环。若消息队列为空，线程则会阻塞等待。

说了这一堆，那么和Handler 、 Looper 、Message有啥关系？其实Looper负责的就是创建一个MessageQueue，然后进入一个无限循环体不断从该MessageQueue中读取消息，而消息的创建者就是一个或多个Handler 。

#### 2.2.1、Looper

对于Looper主要是prepare()和loop()两个方法。 

首先看prepare()方法

```java
public static final void prepare() {
        if (sThreadLocal.get() != null) {
            throw new RuntimeException("Only one Looper may be created per thread");
        }
        sThreadLocal.set(new Looper(true));
}
```

sThreadLocal是一个ThreadLocal对象，可以在一个线程中存储变量。可以看到，在第5行，将一个Looper的实例放入了ThreadLocal，并且2-4行判断了sThreadLocal是否为null，否则抛出异常。这也就说明了Looper.prepare()方法不能被调用两次，同时也保证了一个线程中只有一个Looper实例~相信有些哥们一定遇到这个错误。下面看Looper的构造方法

```java
private Looper(boolean quitAllowed) {  
        mQueue = new MessageQueue(quitAllowed);  
        mRun = true;  
        mThread = Thread.currentThread();  
}
```

在构造方法中，创建了一个MessageQueue（消息队列）。 

然后我们看loop()方法：

```java
public static void loop() {  
        final Looper me = myLooper();  
        if (me == null) {  
            throw new RuntimeException("No Looper; Looper.prepare() wasn't called on this thread.");  
        }  
        final MessageQueue queue = me.mQueue;  
  
        // Make sure the identity of this thread is that of the local process,  
        // and keep track of what that identity token actually is.  
        Binder.clearCallingIdentity();  
        final long ident = Binder.clearCallingIdentity();  
  
        for (;;) {  
            Message msg = queue.next(); // might block  
            if (msg == null) {  
                // No message indicates that the message queue is quitting.  
                return;  
            }  
  
            // This must be in a local variable, in case a UI event sets the logger  
            Printer logging = me.mLogging;  
            if (logging != null) {  
                logging.println(">>>>> Dispatching to " + msg.target + " " +  
                        msg.callback + ": " + msg.what);  
            }  
  
            msg.target.dispatchMessage(msg);  
  
            if (logging != null) {  
                logging.println("<<<<< Finished to " + msg.target + " " + msg.callback);  
            }  
  
            // Make sure that during the course of dispatching the  
            // identity of the thread wasn't corrupted.  
            final long newIdent = Binder.clearCallingIdentity();  
            if (ident != newIdent) {  
                Log.wtf(TAG, "Thread identity changed from 0x"  
                        + Long.toHexString(ident) + " to 0x"  
                        + Long.toHexString(newIdent) + " while dispatching to "  
                        + msg.target.getClass().getName() + " "  
                        + msg.callback + " what=" + msg.what);  
            }  
  
            msg.recycle();  
        }  
}  
```

第2行：
public static Looper myLooper() {
return sThreadLocal.get();
}
方法直接返回了sThreadLocal存储的Looper实例，如果me为null则抛出异常，也就是说looper方法必须在prepare方法之后运行。
第6行：拿到该looper实例中的mQueue（消息队列）
13到45行：就进入了我们所说的无限循环。
14行：取出一条消息，如果没有消息则阻塞。
27行：使用调用 msg.target.dispatchMessage(msg);把消息交给msg的target的dispatchMessage方法去处理。Msg的target是什么呢？其实就是handler对象，下面会进行分析。
44行：释放消息占据的资源。

**Looper主要作用：**

1、	与当前线程绑定，保证一个线程只会有一个Looper实例，同时一个Looper实例也只有一个MessageQueue。
2、	loop()方法，不断从MessageQueue中去取消息，交给消息的target属性的dispatchMessage去处理。
好了，我们的异步消息处理线程已经有了消息队列（MessageQueue），也有了在无限循环体中取出消息的哥们，现在缺的就是发送消息的对象了，于是乎：Handler登场了。



#### 2.2.2、Handler

使用Handler之前，我们都是初始化一个实例，比如用于更新UI线程，我们会在声明的时候直接初始化，或者在onCreate中初始化Handler实例。所以我们首先看Handler的构造方法，看其如何与MessageQueue联系上的，它在子线程中发送的消息（一般发送消息都在非UI线程）怎么发送到MessageQueue中的。

```java
public Handler() {  
        this(null, false);  
}  
public Handler(Callback callback, boolean async) {  
        if (FIND_POTENTIAL_LEAKS) {  
            final Class<? extends Handler> klass = getClass();  
            if ((klass.isAnonymousClass() || klass.isMemberClass() || klass.isLocalClass()) &&  
                    (klass.getModifiers() & Modifier.STATIC) == 0) {  
                Log.w(TAG, "The following Handler class should be static or leaks might occur: " +  
                    klass.getCanonicalName());  
            }  
        }  
  
        mLooper = Looper.myLooper();  
        if (mLooper == null) {  
            throw new RuntimeException(  
                "Can't create handler inside thread that has not called Looper.prepare()");  
        }  
        mQueue = mLooper.mQueue;  
        mCallback = callback;  
        mAsynchronous = async;  
    } 
```

14行：通过Looper.myLooper()获取了当前线程保存的Looper实例，然后在19行又获取了这个Looper实例中保存的MessageQueue（消息队列），这样就保证了handler的实例与我们Looper实例中MessageQueue关联上了。

然后看我们最常用的sendMessage方法

```Java
public final boolean sendMessage(Message msg)  
 {  
     return sendMessageDelayed(msg, 0);  
 }  
```



```java
public final boolean sendEmptyMessageDelayed(int what, long delayMillis) {  
     Message msg = Message.obtain();  
     msg.what = what;  
     return sendMessageDelayed(msg, delayMillis);  
 }  
```



```java
public final boolean sendMessageDelayed(Message msg, long delayMillis)  
   {  
       if (delayMillis < 0) {  
           delayMillis = 0;  
       }  
       return sendMessageAtTime(msg, SystemClock.uptimeMillis() + delayMillis);  
   } 
```



```java
public boolean sendMessageAtTime(Message msg, long uptimeMillis) {  
       MessageQueue queue = mQueue;  
       if (queue == null) {  
           RuntimeException e = new RuntimeException(  
                   this + " sendMessageAtTime() called with no mQueue");  
           Log.w("Looper", e.getMessage(), e);  
           return false;  
       }  
       return enqueueMessage(queue, msg, uptimeMillis);  
   }
```

辗转反则最后调用了sendMessageAtTime，在此方法内部有直接获取MessageQueue然后调用了enqueueMessage方法，我们再来看看此方法：

```java
private boolean enqueueMessage(MessageQueue queue, Message msg, long uptimeMillis) {  
       msg.target = this;  
       if (mAsynchronous) {  
           msg.setAsynchronous(true);  
       }  
       return queue.enqueueMessage(msg, uptimeMillis);  
 } 
```



enqueueMessage中首先为meg.target赋值为this，【如果大家还记得Looper的loop方法会取出每个msg然后交给msg,target.dispatchMessage(msg)去处理消息】，也就是把当前的handler作为msg的target属性。最终会调用queue的enqueueMessage的方法，也就是说handler发出的消息，最终会保存到消息队列中去。

现在已经很清楚了Looper会调用prepare()和loop()方法，在当前执行的线程中保存一个Looper实例，这个实例会保存一个MessageQueue对象，然后当前线程进入一个无限循环中去，不断从MessageQueue中读取Handler发来的消息。然后再回调创建这个消息的handler中的dispathMessage方法，下面我们赶快去看一看这个方法：

```java
public void dispatchMessage(Message msg) {  
        if (msg.callback != null) {  
            handleCallback(msg);  
        } else {  
            if (mCallback != null) {  
                if (mCallback.handleMessage(msg)) {  
                    return;  
                }  
            }  
            handleMessage(msg);  
        }  
 } 
```



可以看到，第10行，调用了handleMessage方法，下面我们去看这个方法：

```java
/** 
   * Subclasses must implement this to receive messages. 
   */  
  public void handleMessage(Message msg) {  
  }    
```

可以看到这是一个空方法，为什么呢，因为消息的最终回调是由我们控制的，我们在创建handler的时候都是复写handleMessage方法，然后根据msg.what进行消息处理。

例如：

```java
private Handler mHandler = new Handler()  
    {  
        public void handleMessage(android.os.Message msg)  
        {  
            switch (msg.what)  
            {  
            case value:  
                  
                break;  
  
            default:  
                break;  
            }  
        };  
    }; 
```

 

到此，这个流程已经解释完毕，让我们首先总结一下

1、首先Looper.prepare()在本线程中保存一个Looper实例，然后该实例中保存一个MessageQueue对象；因为Looper.prepare()在一个线程中只能调用一次，所以MessageQueue在一个线程中只会存在一个。

2、Looper.loop()会让当前线程进入一个无限循环，不端从MessageQueue的实例中读取消息，然后回调msg.target.dispatchMessage(msg)方法。

3、Handler的构造方法，会首先得到当前线程中保存的Looper实例，进而与Looper实例中的MessageQueue想关联。

4、Handler的sendMessage方法，会给msg的target赋值为handler自身，然后加入MessageQueue中。

5、在构造Handler实例时，我们会重写handleMessage方法，也就是msg.target.dispatchMessage(msg)最终调用的方法。

好了，总结完成，大家可能还会问，那么在Activity中，我们并没有显示的调用Looper.prepare()和Looper.loop()方法，为啥Handler可以成功创建呢，这是因为在Activity的启动代码中，已经在当前UI线程调用了Looper.prepare()和Looper.loop()方法。

#### 2.2.3、Handler post

今天有人问我，你说Handler的post方法创建的线程和UI线程有什么关系？

其实这个问题也是出现这篇博客的原因之一；这里需要说明，有时候为了方便，我们会直接写如下代码：

```java
mHandler.post(new Runnable()  
        {  
            @Override  
            public void run()  
            {  
                Log.e("TAG", Thread.currentThread().getName());  
                mTxt.setText("yoxi");  
            }  
}); 
```



然后run方法中可以写更新UI的代码，其实这个Runnable并没有创建什么线程，而是发送了一条消息，下面看源码：

```java
public final boolean post(Runnable r)  
{  
  return  sendMessageDelayed(getPostMessage(r), 0);  
}  
```



```java
private static Message getPostMessage(Runnable r) {  
      Message m = Message.obtain();  
      m.callback = r;  
      return m;  
} 
```



可以看到，在getPostMessage中，得到了一个Message对象，然后将我们创建的Runable对象作为callback属性，赋值给了此message.

注：产生一个Message对象，可以new  ，也可以使用Message.obtain()方法；两者都可以，但是更建议使用obtain方法，因为Message内部维护了一个Message池用于Message的复用，避免使用new 重新分配内存。

```java
public final boolean sendMessageDelayed(Message msg, long delayMillis)  
{  
	if (delayMillis < 0) {  
		delayMillis = 0;  
	}  
	return sendMessageAtTime(msg, SystemClock.uptimeMillis() + delayMillis);  
}  
```

```Java
public boolean sendMessageAtTime(Message msg, long uptimeMillis) {  
  MessageQueue queue = mQueue;  
  if (queue == null) {  
    RuntimeException e = new RuntimeException(  
      this + " sendMessageAtTime() called with no mQueue");  
    Log.w("Looper", e.getMessage(), e);  
    return false;  
  }  
  return enqueueMessage(queue, msg, uptimeMillis);  
} 
```



最终和handler.sendMessage一样，调用了sendMessageAtTime，然后调用了enqueueMessage方法，给msg.target赋值为handler，最终加入MessagQueue.

可以看到，这里msg的callback和target都有值，那么会执行哪个呢？

其实上面已经贴过代码，就是dispatchMessage方法：

```java
public void dispatchMessage(Message msg) {  
       if (msg.callback != null) {  
           handleCallback(msg);  
       } else {  
           if (mCallback != null) {  
               if (mCallback.handleMessage(msg)) {  
                   return;  
               }  
           }  
           handleMessage(msg);  
       }  
   } 
```



第2行，如果不为null，则执行callback回调，也就是我们的Runnable对象。

好了，关于Looper , Handler , Message 这三者关系上面已经叙述的非常清楚了。

最后来张图解：

![img](http://img.blog.csdn.net/20140805002935859?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbG1qNjIzNTY1Nzkx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

希望图片可以更好的帮助大家的记忆~~

#### 2.2.4、后话

其实Handler不仅可以更新UI，你完全可以在一个子线程中去创建一个Handler，然后使用这个handler实例在任何其他线程中发送消息，最终处理消息的代码都会在你创建Handler实例的线程中运行。

```java
new Thread()  
{  
private Handler handler;  
public void run()  
{  

Looper.prepare();  

handler = new Handler()  
{  
  public void handleMessage(android.os.Message msg)  
  {  
      Log.e("TAG",Thread.currentThread().getName());  
  };  
};
```

 Android不仅给我们提供了异步消息处理机制让我们更好的完成UI的更新，其实也为我们提供了异步消息处理机制代码的参考~~不仅能够知道原理，最好还可以将此设计用到其他的非Android项目中去~~

关于后记，有兄弟联系我说，到底可以在哪使用，见博客：[Android Handler 异步消息处理机制的妙用 创建强大的图片加载类](http://blog.csdn.net/lmj623565791/article/details/38476887)



### 2.3、HandlerThread完全解析

URL:[http://blog.csdn.net/lmj623565791/article/details/47079737/]

#### 2.3.1、概述

话说最近股市变动不变，也成了热火朝天的话题。不知道大家有没有考虑做个实时更新股市数据的app呢？假设我们要做一个股市数据实时更新的app，我们可以在网上找个第三方的股市数据接口，然后在我们的app中每隔1分钟（合适的时间）去更新数据，然后更新我们的UI即可。

当然了，本文不是要教大家做这样一个app，只是举个场景。言归正传，回到我们的HandlerThread，大家一定听说过Looper、Handler、Message三者的关系（如果不了解，可以查看[Android 异步消息处理机制 让你深入理解 Looper、Handler、Message三者关系](http://blog.csdn.net/lmj623565791/article/details/38377229)），在我们的UI线程默默的为我们服务。其实我们可以借鉴UI线程Looper的思想，搞个子线程，也通过Handler、Message通信，可以适用于很多场景。

对了，我之前写过一篇博文[Android Handler 异步消息处理机制的妙用 创建强大的图片加载类](http://blog.csdn.net/lmj623565791/article/details/38476887)，这篇博文中就在子线程中创建了Looper，Handler，原理和HandlerThread一模一样，可惜当时我并不知道这个类，不过大家也可以当做HandlerThread应用场景进行学习。

#### 2.3.2、HandlerThread实例

下面看我们模拟大盘走势的代码，其实非常简单，就一个Activity

```java
package com.zhy.blogcodes.intentservice;

import android.os.Bundle;
import android.os.Handler;
import android.os.HandlerThread;
import android.os.Message;
import android.support.v7.app.AppCompatActivity;
import android.text.Html;
import android.widget.TextView;
import com.zhy.blogcodes.R;
public class HandlerThreadActivity extends AppCompatActivity
{
    private TextView mTvServiceInfo;
    private HandlerThread mCheckMsgThread;
    private Handler mCheckMsgHandler;
    private boolean isUpdateInfo;
    private static final int MSG_UPDATE_INFO = 0x110;
    //与UI线程管理的handler
    private Handler mHandler = new Handler();
    @Override
    protected void onCreate(Bundle savedInstanceState)
    {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_thread_handler);

        //创建后台线程
        initBackThread();
        mTvServiceInfo = (TextView) findViewById(R.id.id_textview);
    }

    @Override
    protected void onResume()
    {
        super.onResume();
        //开始查询
        isUpdateInfo = true;
        mCheckMsgHandler.sendEmptyMessage(MSG_UPDATE_INFO);
    }

    @Override
    protected void onPause()
    {
        super.onPause();
        //停止查询
        isUpdateInfo = false;
        mCheckMsgHandler.removeMessages(MSG_UPDATE_INFO);

    }

    private void initBackThread()
    {
        mCheckMsgThread = new HandlerThread("check-message-coming");
        mCheckMsgThread.start();
        mCheckMsgHandler = new Handler(mCheckMsgThread.getLooper())
        {
            @Override
            public void handleMessage(Message msg)
            {
                checkForUpdate();
                if (isUpdateInfo)
                {
                    mCheckMsgHandler.sendEmptyMessageDelayed(MSG_UPDATE_INFO, 1000);
                }
            }
        };
    }

    /**
     * 模拟从服务器解析数据
     */
    private void checkForUpdate()
    {
        try
        {
            //模拟耗时
            Thread.sleep(1000);
            mHandler.post(new Runnable()
            {
                @Override
                public void run()
                {
                    String result = "实时更新中，当前大盘指数：<font color='red'>%d</font>";
                    result = String.format(result, (int) (Math.random() * 3000 + 1000));
                    mTvServiceInfo.setText(Html.fromHtml(result));
                }
            });

        } catch (InterruptedException e)
        {
            e.printStackTrace();
        }
    }

    @Override
    protected void onDestroy()
    {
        super.onDestroy();
        //释放资源
        mCheckMsgThread.quit();
    }
}
```

可以看到我们在onCreate中，去创建和启动了HandlerThread，并且关联了一个mCheckMsgHandler。然后我们分别在onResume和onPause中去开启和暂停我们的查询，最后在onDestory中去释放资源。

这样就实现了我们每隔5s去服务端查询最新的数据，然后更新我们的UI，当然我们这里通过Thread.sleep()模拟耗时，返回了一个随机数，大家可以很轻易的换成真正的数据接口。

布局文件：

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
                xmlns:tools="http://schemas.android.com/tools"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:paddingLeft="@dimen/activity_horizontal_margin"
                android:paddingRight="@dimen/activity_horizontal_margin"
                android:paddingTop="@dimen/activity_vertical_margin"
                android:paddingBottom="@dimen/activity_vertical_margin">

    <TextView
        android:id="@+id/id_textview"
        android:text="正在加载大盘指数..."
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>

</RelativeLayout>
```

**运行效果图**

![](http://img.blog.csdn.net/20150727085713592)

                                                                        

别问我为什么要用红色！！！

ok，当然了，我们的效果很单调，但是你完全可以去扩展，比如ListView显示用户关注的股票数据。或者是其他的需要一直检测更新的数据。

看到这，你是否好奇呢，HandlerThread的内部是如何做的呢？其实非常的简单，如果你看过[Android 异步消息处理机制 让你深入理解 Looper、Handler、Message三者关系](http://blog.csdn.net/lmj623565791/article/details/38377229)估计扫几眼就会了。

***HandlerThread 源码分析***

对于所有Looper，Handler相关细节统一参考上面提到的文章。

我们轻轻的掀开HandlerThread的源码，还记得我们是通过

```java
 mCheckMsgThread = new HandlerThread("check-message-coming");
 mCheckMsgThread.start(); 
```

创建和启动的对象，那么随便扫一眼：

```java
package android.os;


public class HandlerThread extends Thread {
    int mPriority;
    int mTid = -1;
    Looper mLooper;

    public HandlerThread(String name) {
        super(name);
        mPriority = Process.THREAD_PRIORITY_DEFAULT;
    }

    protected void onLooperPrepared() {
    }

    @Override
    public void run() {
        mTid = Process.myTid();
        Looper.prepare();
        synchronized (this) {
            mLooper = Looper.myLooper();
            notifyAll();
        }
        Process.setThreadPriority(mPriority);
        onLooperPrepared();
        Looper.loop();
        mTid = -1;
    }
```



看到了什么，其实我们就是初始化和启动了一个线程；然后我们看run()方法，可以看到该方法中调用了Looper.prepare()，Loop.loop();

prepare()呢，中创建了一个Looper对象，并且把该对象放到了该线程范围内的变量中（sThreadLocal），在Looper对象的构造过程中，初始化了一个MessageQueue，作为该Looper对象成员变量。

loop()就开启了，不断的循环从MessageQueue中取消息处理了，当没有消息的时候会阻塞，有消息的到来的时候会唤醒。如果你不明白我说的，参考上面推荐的文章。

------

接下来，我们创建了一个mCheckMsgHandler，是这么创建的：

```java
mCheckMsgHandler = new Handler(mCheckMsgThread.getLooper());
```

对应源码

```java
public Looper getLooper() {
        if (!isAlive()) {
            return null;
        }

        // If the thread has been started, wait until the looper has been created.
        synchronized (this) {
            while (isAlive() && mLooper == null) {
                try {
                    wait();
                } catch (InterruptedException e) {
                }
            }
        }
        return mLooper;
    }
```

mCheckMsgThread.getLooper()返回的就是我们在run方法中创建的mLooper。

那么Handler的构造呢，其实就是在Handler中持有一个指向该Looper.mQueue对象，当handler调用sendMessage方法时，其实就是往该mQueue中去插入一个message，然后Looper.loop()就会取出执行。

好了，到这我们就分析完了，其实就几行代码；不过有一点我想提一下：

如果你够细心你会发现，run方法里面当mLooper创建完成后有个notifyAll()，getLooper()中有个wait()，这是为什么呢？因为的mLooper在一个线程中执行，而我们的handler是在UI线程初始化的，也就是说，我们必须等到mLooper创建完成，才能正确的返回getLooper();wait(),notify()就是为了解决这两个线程的同步问题。

不过对于这样的线程间的同步问题，我非常喜欢使用Semaphore。

还记得在[Android Handler 异步消息处理机制的妙用 创建强大的图片加载类](http://blog.csdn.net/lmj623565791/article/details/38476887)一文中有类似的说明：

>如果你比较细心，可能会发现里面还有一些信号量的操作的代码，如果你不了解什么是信号量，可以参考：[Java 并发专题 ： Semaphore 实现 互斥 与 连接池](http://blog.csdn.net/lmj623565791/article/details/26810813) 。 简单说一下mSemaphore（信号数为1）的作用，由于mPoolThreadHander实在子线程初始化的，所以我在初始化前调用了mSemaphore.acquire去请求一个信号量，然后在初始化完成后释放了此信号量，我为什么这么做呢？因为在主线程可能会立即使用到mPoolThreadHander，但是mPoolThreadHander是在子线程初始化的，虽然速度很快，但是我也不能百分百的保证，主线程使用时已经初始化结束。

哈，当时也有很多人问，为什么使用这个Semaphore，到这里我想大家应该清楚了。话说假设我当时真的HanderThread这个类，可能之前的代码能简化不少呢~

对了，你可能会问与Timer相比有什么优势呢？

直接参考这篇文章吧：[Handler vs Timer](http://androidtrainningcenter.blogspot.com/2013/12/handler-vs-timer-fixed-period-execution.html)



#### 2.3.3、HandlerThread使用总结

什么时候使用？

希望用Handler往子线程发送消息的时候使用！

1. 创建带名称的HandlerThread
```
HandlerThread handlerThread = new HandlerThread(“threadName”);
```
2. 启动HandlerThread
```
handlerThread.start();
```
3. 创建一个Handler并且将HandlerThread的Looper传入
```
new Handler(handlerThread.getLooper()){
    handleMessage(Message msg) {}
}
```
这样就创建了一个子线程的Thread和一个子线程的Handler



# 三、Android 生命周期

## 1、Activity的生命周期

生命周期的七个方法中，要熟练的掌握在不同情况下，七个方法的执行顺序，七个方法除了onRestart()方法其他六个是成对出现的。

```java
The entire lifetime:onCreate() --- onDestroy()             完整生命周期

The visible lifetime:onStart() --- onStop()                    可见生命周期

The foreground lifetime: onResume() --- onPause()    前沿生命周期	
```

**完整的生命周期**：是从创建到销毁的整个过程，所以是从最先执行的方法onCreate()和最后执行方法onDestroy()方法。		

**可见生命周期**:是从onStart()到onStop(),可见，在执行onStart()方法时，UI界面开始在屏幕上被画出，而执行到onStop()方法时，画面就退出了屏幕。被另一个界面所取代。		

**前沿生命周期**：实在程序运行时执行过程中，如果发生改变，当一个activity开始运行时，运行中的状态就是onResume()方法，如果暂停或突发的意外事故，那么首先被执行的就是onPouse()方法

生命周期总结

启动Activity按后退键退出

```java
|- onCtreate
|- onStart()
|- onResume()
|- onPause()
|- onStop() ;
|- onDestory() ;
```

启动Activity按Home键，返回应用

```java
|- onCreate()
|- onStart()
|- onResume()
//按HOME
|- onPause()
|- onStop()
//返回应用
|- onRestart()
|- onStart()
|- onResume(
```

**生命周期注意点：**

1. 第一点：在切换页面的时候，第一个界面先执行onPause()方法，第二个页面onCreate(),onStart(),onResume()，第一个页面onStop()
2. 第二点：在进行横竖屏切换的时候，Activity的生命周期要销毁重新创建。



# 四、时间调度

Timer、CountDownTimer

```java
public void playMusic() {
		timer = new Timer() ;
		//第一个参数是要执行 的任务
		//第二个，第二个参数是持续的时间，第三个参数是间隔的时间	
		timer.schedule(new TimerTask() {
			public void run() {
				Log.i(TAG, "------>>i ="  + i++) ;
			}
		}, 0, 1000);
	}
```

Timer类就是定时器的类，如果要使用定义是其定时器，就要实例化Timer的对象。	在Timer类中有一个方法是用来启动，并设置该定时器的参数的。	Timer的schedule()方法：		• 第一个参数：传入一个TimerTask的对象，TimerTask是一个抽象类，此类实现了Runnable接口，所以他是一个线程类，相当与是我们的一个代理。将真是的主题交给它去处理。		• 第二个参数：此计时器开始执行后，多少秒后执行我们的主题业务。		• 第三个参数：业务执行相隔的时间。



# 五、Intent及启动模式

## 1、启动模式

一个任务(task)即使在执行某项工作时与用户进行交互的Activity的集合，这些Activity按照被打开的顺序一次被安排在一个堆栈中(回退栈) ；

LaunchMode,是在回退栈中的保存的内容，

    在Activity的配置节点中，可以增加一个属性。

```xml
android:launchMode="standard"
```



默认情况standard下这样的。跳转几次回退栈就有多少个实例。

如果是SingleTop，表示顶端的实例，只能是一个，不能存在重复。

如果是SingleTask，实例只要有相同的，就把他们中间的任务全部消掉，像是纸牌游戏中“上北京”。但不同的是会保留相同的那个对象。

    **SingleInstance:****就相当于单例模式，他是独立于任务的，他会单独启动一个进程。**

**范例：**如果有下面的题目

    如果四个activity页面,跳转顺序：A-B-C-D-A，问按返回键几次可以返回到HOME页面一个任务(task)即使在执行某项工作时与用户进行交互的Activity的集合，这些Activity按照被打开的顺序一次被安排在一个堆栈中(回退栈) ；

LaunchMode,是在回退栈中的保存的内容，

    在Activity的配置节点中，可以增加一个属性。

默认情况standard下这样的。跳转几次回退栈就有多少个实例。

如果是SingleTop，表示顶端的实例，只能是一个，不能存在重复。

如果是SingleTask，实例只要有相同的，就把他们中间的任务全部消掉，像是纸牌游戏中“上北京”。但不同的是会保留相同的那个对象。

     **SingleInstance:****就相当于单例模式，他是独立于任务的，他会单独启动一个进程。**

**范例：**如果有下面的题目

    如果四个activity页面,跳转顺序：A-B-C-D-A，问按返回键几次可以返回到HOME页面	

```
A-D-C-B-HOME
```

答案是4次，因为是单例。



## 2、Intent七大属性

在ActivityManifest.xml文件中Intent节点，可以设置七种属性。

       ·ComponetName、Action、Categorys、Data、Type、Extra、Flags

### 2.1、ComponetName

就是指定从哪个页面跳转到哪个页面。

设置了CompentName属性的，称为显式Intent

```Java
Intent intent = new Intent();
ComponentName cName = new ComponentName(MainActivity.this,NextActivity.class);
intent.setComponent(cName);
startActivity(intent);
```

显式Intent也通常这样写

```java
Intent intent = new Intent(MainActivity.this,NextActivity.class);
```

称为显示Intent

   隐式Intent，设置action属性，就称为隐式Intent，其实现的原理就是利用“反射机制“完成的。他是软编码，可以在文件编译完成后，改变内容。通过xml的配置文件便可以完成了。比如页面的跳转，如果使用隐式Intent，就可以完成跳转的灵活性。

   类似于框架技术。

### 2.2、Action、Category属性与intent-filter属性

Action和Category这两个属性，就是为了程序减少耦合型，其实现的原理就是“反射“。

 使用intent-filter，就代表要使用隐式intent了，所以设置了intent-filter就代表要完成页面的跳转功能。

```java
Intent intent = new Intent(); 
intent.setAction("com.steven.android06lifecycle.NextActivity"); 
startActivity(intent);
```

AndroidManifast.xml

```xml
//在配置文件中注册Activity的时候需要声明：
<activity android:name="com.steven.android06lifecycle.NextActivity">
<intent-filter>
<action android:name="com.steven.android06lifecycle.nextactivity" />
<category android:name="android.intent.category.DEFAULT" /> 
</intent-filter>
</activity>
```



**设置默认第一个启动的Activity**

当某个页面是默认启动页面时，需要定义Action、Category的属性必须为以下字符串：【设置任务入口】

```Xml
<intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
</intent-filter>
```

在跳转的设置action属性，都设置的是包.类

```xml
<action android:name="com.steven.android06lifecycle.nextactivity" />
```



#### 2.2.1、Anction的常用属性

Intent对象不仅可以启动本应用内的程序组件，也可以启动Android系统的其他应用的组件，包括系统内置的程序组件（需要设置权限）。

-   ACTION_MAIN：（android.intent.action.MAIN）Android程序入口。

每个Android应用必须且只能包含一个此类型的Action声明。【如果设置多个，则哪个在前，执行哪个。】

-   ACTION_VIEW： （android.intent.action.VIEW） 显示指定数据。
-   ACTION_EDIT： （android.intent.action.EDIT） 编辑指定数据。
-   ACTION_DIAL： （android.intent.action.DIAL） 显示拨号面板。
-   ACTION_CALL： （android.intent.action.CALL） 直接呼叫Data中所带的号码。
-   ACTION_ANSWER： （android.intent.action.ANSWER） 接听来电。
-   ACTION_SEND： （android.intent.action.SEND） 向其他人发送数据（例如：彩信/email）。
-   ACTION_SENDTO：  （android.intent.action.SENDTO） 向其他人发送短信。
-   ACTION_SEARCH： （android.intent.action.SEARCH） 执行搜索。
-   ACTION_GET_CONTENT： （android.intent.action.GET_CONTENT） 让用户选择数据，并返回所选数据。

#### 2.2.2、category

Category属性为Action增加额外的附加类别信息。CATEGORY_LAUNCHER意味着在加载程序的时候Acticity出现在最上面，而CATEGORY_HOME表示页面跳转到HOME界面。

**范例：**实现页面跳转到HOME界面的代码。

```java
Intent intent = new Intent(); 
intent.setAction(Intent.ACTION_MAIN); 
intent.addCategory(Intent.CATEGOTY_HOME); 
startActivity(intent); 
```

 需要注意的是，这些属性通常可以在程序代码中设置，也可以在配置文件中使用，在配置中使用减少了耦合性，在页面中设置是属于每个Activity的。

##### 2.2.1、常用Category属性常量：

- CATEGORY_DEFAULT： （android.intent.category.DEFAULT） Android系统中默认的执行方式，按照普通Activity的执行方式执行。

- CATEGORY_HOME： （android.intent.category.HOME） 设置该组件为Home Activity。
- CATEGORY_PREFERENCE： （android.intent.category.PREFERENCE） 设置该组件为Preference。
- CATEGORY_LAUNCHER： （android.intent.category.LAUNCHER） 设置该组件为在当前应用程序启动器中优先级最高的Activity，通常与入口ACTION_MAIN配合使用。
- CATEGORY_BROWSABLE： （android.intent.category.BROWSABLE） 设置该组件可以使用浏览器启动。



#### 2.2.3、Data属性

Data属性通常用于向Action属性提供操作的数据，Data属性是一个Uri对象

Uri的格式如下：

scheme://host:port/path

##### 2.2.3.1、系统中内置的几个Data属性常量

- tel://：号码数据格式，后跟电话号码。
- mailto://：邮件数据格式，后跟邮件收件人地址。
- smsto://：短息数据格式，后跟短信接收号码。
- content://：内容数据格式，后跟需要读取的内容。
- file://：文件数据格式，后跟文件路径。
- market://search?q=pname:pkgname：市场数据格式，在Google Market里搜索包名为pkgname的应用。

- geo://latitude, longitude：经纬数据格式，在地图上显示经纬度所指定的位置。




# 六、数据存储

·数据存储的方法

1、 文件存储

2、 数据库存储

3、 contentProvider

4、 网络存储

5、 外部存储



## 1、SharedPreferences

SharedPreferences提供了一些基础的信息的保存功能，所有的数据都是安好key=value的形式进行保存的。
SharedPreferences接口所提供保存信息的只能是一些基本的数据类型。
在ShaedPreferences中有如下的几个常用的方法：

- 进入可编辑状态：public abstract SharedPreferences.Editor edit ()
  edit()的方法返回的是SharedPreferences.Editor接口的类型，在这个接口中提供了如下的几个常用的方法。

**SharedPreferences.Editor接口的常用方法**

| 方法                                   | 描述              |
| ------------------------------------ | --------------- |
| clear()                              | 清除所有的数据         |
| commit()                             | 提交更新的数据         |
| putBoolean(String key,boolean value) | 保存一个boolean类型数据 |
| putFloat(String key,float value)     | 保存一个float类型数据   |
| putInt(String key,float value)       | 保存一个int类型数据     |
| putLong(String key,long value)       | 保存一个long类型数据    |
| putString(String key,String value)   | 保存一个String类型数据  |
| remove(String key)                   | 删除指定key的数据      |






























# 附录

## 1、参考网站

Android开发程序员必备网站：http://www.jianshu.com/p/9ad855577d1c

各种工具类:https://github.com/Blankj/AndroidUtilCode/blob/master/README-CN.md



