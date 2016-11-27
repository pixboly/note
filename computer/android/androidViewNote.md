> Android View 笔记

# 概述

## 1、API Guides

           Appcomponents 组件

           UserInterface

           Appresources 资源

           Animationand Graphics 动画图像

           Mediaand Camera 多媒体

           Locationand Sensors定位感应器

           Connectivity连接

           Textand Input 输入法

           DataStorage 数据存储

           WebApps ：

# 一、ViewGroup相关

## 1、布局Layout

### 1.1布局的分类

RelativeLayout相对布局：(重点)

           LinearLayout线性布局：（重点）

           GridLayout网格布局（重点）

           AbsoluteLayout绝对布局

           FrameLayout:帧动画布局

           TableLayout表格布局

# 二、View相关

## 1、View相关

### 1.1、View的常用属性

1、android:id 组件的id（唯一）

2、android:background

 3、android:onClick 为该空间的单击事件绑定监听器

 每一控件都可以单击监听。onClick调用监听器

4、android:padding：设置控件四周的填充区域

填充的时候需要设置填充的方向，默认是四周都填充

5、android:margin：外边距

6、android:visibility:该空间是否控件

7、android:alpha        该组件的透明度

8、android:layout_height  子组件的布局高度

9、android:layout_width: 子组件的布局宽度

## 2、TextView

###  2.1、文字加粗

英文设置加粗可以在xml里面设置: 
复制代码代码如下:

```
<SPAN style="FONT-SIZE: 18px">android:textStyle="bold"</SPAN> 
```

英文还可以直接在String文件里面直接这样填写: 
复制代码代码如下:

```
<string name="styled_text">Plain, <b>bold</b>, <i>italic</i>, <b><i>bold-italic</i></b></string> 
```

b代码加粗,i代表倾斜 
中文设置加粗就需要在代码中获取到当前TextView在进行设置: 
复制代码代码如下:

```java
TextView tv = (TextView)findViewById(R.id.tv); 
TextPaint tp = tv.getPaint(); 
tp.setFakeBoldText(true); 
```

### 2.2、字数限制

为了解决textview中内容过长的话自动换行，但是调用measureText函数时发现返回值很不准确，单位也不确定，是pixel还是dip，都不准。后来想起textview中有个内容过长加省略号的属性，即ellipsize，可以解决这个问题，用法如下：
在xml中

```java
android:ellipsize = "end"　　  省略号在结尾
android:ellipsize = "start" 　　省略号在开头
android:ellipsize = "middle"     省略号在中间
android:ellipsize = "marquee"  跑马灯
最好加一个约束android:singleline = "true"
```

当然也可以用代码语句

```java
tv.setEllipsize(TextUtils.TruncateAt.valueOf("END"));
tv.setEllipsize(TextUtils.TruncateAt.valueOf("START"));
tv.setEllipsize(TextUtils.TruncateAt.valueOf("MIDDLE"));
tv.setEllipsize(TextUtils.TruncateAt.valueOf("MARQUEE"));
```

最好再加一个约束tv.setSingleLine(true);
不仅对于textview有此属性，对于editext也有，不过它不支持marquee

这里要说明一下：
 1、如果按字数限制
那么布局文件因使用如下属性：
范例：

```
<TextView
   android:id="@+id/tv_sweepstake_second_nickname"
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"
   android:maxEms="6"
   特别要注意这里使用的是maxEms而非mamLength
   android:textColor="#FFF"
   android:singleLine="true"
   android:ellipsize="end"
   android:textSize="9sp"
   android:text="本色小宝得得得地对地导弹"
    />
```

2、如果按布局限制，则固定其宽度即可

### 2.3、HTML样式显示

支持的Html标签

```
<a href="...">
<b>
<big>
<blockquote>
<br>
<cite>
<dfn>
<div align="...">
<em>
<font size="..." color="..." face="...">
<h1>
<h2>
<h3>
<h4>
<h5>
<h6>
<i>
<img src="...">
<p>
<small>
<strike>
<strong>
<sub>
<sup>
<tt>
<u>
```

### 2.4、TextView图片标签

[http://blog.csdn.net/u010418593/article/details/9324101 正在之前Html类支撑的HTML标签文章中懂得到当剖析到img标签时便会回调getDrawable()方式，并须要返回一个Drawable工具；当前我们须要界说类并真]

在之前Html类支持的HTML标签文章中了解到当解析到<img>标签时就会回调getDrawable()方法，并需要返回一个Drawable对象；当前我们需要定义类并实现ImageGetter接口以及在getDrawable方法中做相应的处理，下面我们则来看看具体该如何处理；
具体代码：

```
/** 
 * ImageGetter接口的使用 
 * @author Susie 
 */  
public class ImgLabelActivity extends Activity {  
  
    private static final String TAG = "ImgLabelActivity";  
    /**本地图片*/  
    private TextView mTvOne;  
    /**项目资源图片*/  
    private TextView mTvTwo;  
    /**网络图片*/  
    private TextView mTvThree;  
    /**网络图片name*/  
    private String picName = "networkPic.jpg";  
    /**网络图片Getter*/  
    private NetworkImageGetter mImageGetter;  
    /**网络图片路径*/  
    private String htmlThree = "网络图片测试：" + "<img src='http://img.my.csdn.net/uploads/201307/14/1373780364_7576.jpg'>";  
      
    @Override  
    public void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_img_label);  
          
        mTvOne = (TextView) this.findViewById(R.id.tv_img_label_one);  
        String htmlOne = "本地图片测试：" + "<img src='/mnt/sdcard/imgLabel.jpg'>";  
        mTvOne.setText(Html.fromHtml(htmlOne, new LocalImageGetter(), null));  
  
        mTvTwo = (TextView) this.findViewById(R.id.tv_img_label_two);  
        String htmlTwo = "项目图片测试：" + "<img src=""+R.drawable.imagepro+"">";  
        mTvTwo.setText(Html.fromHtml(htmlTwo, new ProImageGetter(), null));  
       
          
        mTvThree = (TextView) this.findViewById(R.id.tv_img_label_three);  
        mImageGetter = new NetworkImageGetter();  
        mTvThree.setText(Html.fromHtml(htmlThree, mImageGetter, null));  
    }  
    /** 
     * 本地图片 
     * @author Susie 
     */  
    private final class LocalImageGetter implements Html.ImageGetter{  
          
        @Override  
        public Drawable getDrawable(String source) {  
            // 获取本地图片  
            Drawable drawable = Drawable.createFromPath(source);  
            // 必须设为图片的边际,不然TextView显示不出图片  
            drawable.setBounds(0, 0, drawable.getIntrinsicWidth(), drawable.getIntrinsicHeight());  
            // 将其返回  
            return drawable;  
        }  
    }  
    /** 
     * 项目资源图片 
     * @author Susie 
     */  
    private final class ProImageGetter implements Html.ImageGetter{  
  
        @Override  
        public Drawable getDrawable(String source) {  
            // 获取到资源id  
            int id = Integer.parseInt(source);  
            Drawable drawable = getResources().getDrawable(id);  
            drawable.setBounds(0, 0, drawable.getIntrinsicWidth(), drawable.getIntrinsicHeight());  
            return drawable;  
        }  
    }  
    /** 
     * 网络图片 
     * @author Susie 
     */  
    private final class NetworkImageGetter implements Html.ImageGetter{  
  
        @Override  
        public Drawable getDrawable(String source) {  
              
            Drawable drawable = null;  
            // 封装路径  
            File file = new File(Environment.getExternalStorageDirectory(), picName);  
            // 判断是否以http开头  
            if(source.startsWith("http")) {  
                // 判断路径是否存在  
                if(file.exists()) {  
                    // 存在即获取drawable  
                    drawable = Drawable.createFromPath(file.getAbsolutePath());  
                    drawable.setBounds(0, 0, drawable.getIntrinsicWidth(), drawable.getIntrinsicHeight());  
                } else {  
                    // 不存在即开启异步任务加载网络图片  
                    AsyncLoadNetworkPic networkPic = new AsyncLoadNetworkPic();  
                    networkPic.execute(source);  
                }  
            }  
            return drawable;  
        }  
    }  
    /** 
     * 加载网络图片异步类 
     * @author Susie 
     */  
    private final class AsyncLoadNetworkPic extends AsyncTask<String, Integer, Void>{  
  
        @Override  
        protected Void doInBackground(String... params) {  
            // 加载网络图片  
            loadNetPic(params);  
            return null;  
        }  
          
        @Override  
        protected void onPostExecute(Void result) {  
            super.onPostExecute(result);  
            // 当执行完成后再次为其设置一次  
            mTvThree.setText(Html.fromHtml(htmlThree, mImageGetter, null));  
        }  
        /**加载网络图片*/  
        private void loadNetPic(String... params) {  
            String path = params[0];  
              
            File file = new File(Environment.getExternalStorageDirectory(), picName);  
              
            InputStream in = null;  
              
            FileOutputStream out = null;  
              
            try {  
                URL url = new URL(path);  
                  
                HttpURLConnection connUrl = (HttpURLConnection) url.openConnection();  
                  
                connUrl.setConnectTimeout(5000);  
                  
                connUrl.setRequestMethod("GET");  
                  
                if(connUrl.getResponseCode() == 200) {  
                      
                    in = connUrl.getInputStream();  
                      
                    out = new FileOutputStream(file);  
                      
                    byte[] buffer = new byte[1024];  
                      
                    int len;  
                      
                    while((len = in.read(buffer))!= -1){  
                        out.write(buffer, 0, len);  
                    }  
                } else {  
                    Log.i(TAG, connUrl.getResponseCode() + "");  
                }  
            } catch (Exception e) {  
                e.printStackTrace();  
            } finally {  
                  
                if(in != null) {  
                    try {  
                        in.close();  
                    } catch (IOException e) {  
                        e.printStackTrace();  
                    }  
                }  
                if(out != null) {  
                    try {  
                        out.close();  
                    } catch (IOException e) {  
                        e.printStackTrace();  
                    }  
                }  
            }  
        }  
    }  
} 
```

需要注意的是：
在获取到drawable时需要为其设置边界，如没有设置的话TextView就不能显示该drawable了；
在加载网络图片时需要开启子线程去访问网络并将图片存储到本地，之后再次为其设置一次；

### 2.5、TextView的Img标签

方法一：重写TextView的onDraw方法，也挺直观就是不太好控制显示完图片后再显示字体所占空间的位置关系。一般如果字体是在图片上重叠的推荐这样写。时间关系，这个不付源码了。

方法二：利用TextView支持部分Html的特性，直接用api赋图片。代码如下:
第一种方法在TextView中显示图片  

```
    String html = "<img src='" + R.drawable.circle + "'/>";  
    ImageGetter imgGetter = new ImageGetter() {  
          
        @Override  
        public Drawable getDrawable(String source) {  
            // TODO Auto-generated method stub  
            int id = Integer.parseInt(source);  
            Drawable d = getResources().getDrawable(id);  
            d.setBounds(0, 0, d.getIntrinsicWidth(), d.getIntrinsicHeight());  
            return d;  
        }  
    };  
    CharSequence charSequence = Html.fromHtml(html, imgGetter, null);  
    textView1.setText(charSequence);  
    textView1.append("您好 ");
```

注意下面这句话：String html = "<img src='" + R.drawable.circle + "'/>"; img src = 后面除了"之外还有个'号。如果去掉这两个'号就变成了String html = "<img src=" + R.drawable.circle +"/>"; 是会报错的，因为src直接等于了一个数字，是解析不了的。

方法三: 用ImageSpan和SpannableString，代码如下：
第2种方法在TextView中显示图片

```
    Bitmap b = BitmapFactory.decodeResource(getResources(), R.drawable.hanguo);  
    ImageSpan imgSpan = new ImageSpan(this, b);  
    SpannableString spanString = new SpannableString("icon");  
    spanString.setSpan(imgSpan, 0, 4, Spannable.SPAN_EXCLUSIVE_EXCLUSIVE);  
    textView2.setText(spanString);  
    textView2.append("中新网4月27日电 据央视报道，韩国国务总理郑烘原于当地时间27日上午召开发布会，称自己应对韩国“岁月号”沉船事件负责，宣布辞职，并希望家属能原谅及理解他的决定。");</span>  
```

这种方法是最直观的，通过Bitmap或Drawable对象得到ImageSpan对象，再新建SpannableString对象，设置span的内容就ok了。其实SpannableString很强大，如在EditText里将部分文本高亮、下划线、斜体、插入表情都可以用它，详见链接:http://gundumw100.iteye.com/blog/904107  
http://blog.csdn.net/rockcoding/article/details/7231756
下为效果图，上下两幅图分别对应第二种和第三种方法：
![](http://note.youdao.com/yws/public/resource/4399b4cc0a8b342c45bd3f8e0c2a3407/EAF860FA2CD24A80B4E6AF78E7A6BDF7)

### 2.6、文字阴影效果

[http://blog.csdn.net/hewence1/article/details/39993415]
废话不多说直接说关键的：

字体阴影需要四个相关参数：

1. android:shadowColor：阴影的颜色
2. android:shadowDx：水平方向上的偏移量
3. android:shadowDy：垂直方向上的偏移量
4. Android:shadowRadius：是阴影的的半径大少

最好这4个值都一起设计
shadowColor这个属性就不多说了，android:shadowDx跟android:shadowDy
为了更清楚的演示就做个试验，分三组xml布局如下：

```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:background="#ff895544" >

     <TextView 
		    android:id="@+id/test_shadow"
		    android:layout_width="wrap_content"
		    android:layout_height="wrap_content"
		    android:textSize="60sp"
		    android:textColor="#ffffffff"
		    android:shadowColor="#ff000000"
		    android:text="Test Shadow"
		    android:layout_gravity="center"
		    android:shadowRadius="1"
		    android:shadowDx="0"
		    android:shadowDy="0"
		    />
       
		<TextView 
		    android:id="@+id/test_shadow2"
		    android:layout_width="wrap_content"
		    android:layout_height="wrap_content"
		    android:textSize="60sp"
		    android:textColor="#ffffffff"
		    android:layout_gravity="center"
		    android:text="Test Shadow"
		    android:shadowColor="#ff000000"
		    android:shadowRadius="1"
		    android:shadowDx="10"
		    android:shadowDy="10"
		    />
		
		<TextView 
		    android:id="@+id/test_shadow3"
		    android:layout_width="wrap_content"
		    android:layout_height="wrap_content"
		    android:textSize="60sp"
		    android:textColor="#ffffffff"
		    android:layout_gravity="center"
		    android:text="Test Shadow"
		    android:shadowColor="#ff000000"
		    android:shadowRadius="1"
		    android:shadowDx="30"
		    android:shadowDy="30"
		    />
		
</LinearLayout>
```

dx  dy 分别为   （0 ， 0） ， （10 ， 10 ） ， （30 ， 30）
结果如下：
![](http://note.youdao.com/yws/public/resource/4399b4cc0a8b342c45bd3f8e0c2a3407/210AEA9DEDA8403299AFEFA6DBB5DB79)
现在更加清楚了吧！

下一个属性是android:shadowRadius  是阴影的的半径大少

对于此属性进行6组试验：

dx  dy 都是 30  shadowRadius  分别为： 0 ， 0.01 ， 1 ， 2 ， 5 ， 10

结果如下：
![](http://note.youdao.com/yws/public/resource/4399b4cc0a8b342c45bd3f8e0c2a3407/995376471FC748EEB5FD0AEB1BE67332)
从结果分析：

1 这个值为0的话是不会显示的

2 值越大，阴影就越大，而且越模糊

到现在应该都清楚这4个值会影响什么效果了，经验丰富的从属性名字就大概知道是什么意思了。



现在回到正常阴影的效果：
1.可以把shadowRadius  变大来实现阴影模糊，使得看起来更加的自然：
代码：

```
 <TextView 
   android:id="@+id/test_shadow"
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"
   android:textSize="60sp"
   android:textColor="#ffffffff"
   android:shadowColor="#ff000000"
   android:text="Test Shadow"
   android:layout_gravity="center"
   android:shadowRadius="1"
   android:shadowDx="5"
   android:shadowDy="5"
   />
       
<TextView 
   android:id="@+id/test_shadow2"
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"
   android:textSize="60sp"
   android:textColor="#ffffffff"
   android:layout_gravity="center"
   android:text="Test Shadow"
   android:shadowColor="#ff000000"
   android:shadowRadius="5"
   android:shadowDx="5"
   android:shadowDy="5"
   />

<TextView 
   android:id="@+id/test_shadow2"
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"
   android:textSize="60sp"
   android:textColor="#ffffffff"
   android:layout_gravity="center"
   android:text="Test Shadow"
   android:shadowColor="#ff000000"
   android:shadowRadius="10"
   android:shadowDx="5"
   android:shadowDy="5"
   />
```

效果：
![](http://note.youdao.com/yws/public/resource/4399b4cc0a8b342c45bd3f8e0c2a3407/BF566070B8184156B35C4AB138EE057C)
调节shadowRadius来确定最适合自己的阴影

2.调试dx  跟 dy来改变光源，使阴影偏向不同的方向 跟  距离

```
 如果光源是在左边，那么dx 是为正的， 
```

```
 光源在最右边，那么dx就是负
```

```
 光源在上  那么dy 为 正
```

```
 光源在下， 那么dy 为 负  
```

那么左上 ， 右下 就是。。。。。。

dx  跟 dy 的正负调节方向， 其值的大少影响距离 ，dx 跟 dy 的 比值 就影响光源的角度

```
<TextView   
        android:id="@+id/test_shadow"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:textSize="60sp"  
        android:textColor="#ffffffff"  
        android:shadowColor="#ff000000"  
        android:text="Test Shadow"  
        android:layout_gravity="center"  
        android:shadowRadius="1"  
        android:shadowDx="5"  
        android:shadowDy="0"  
        />  
        
    <TextView   
        android:id="@+id/test_shadow2"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:textSize="60sp"  
        android:textColor="#ffffffff"  
        android:layout_gravity="center"  
        android:text="Test Shadow"  
        android:shadowColor="#ff000000"  
        android:shadowRadius="1"  
        android:shadowDx="-5"  
        android:shadowDy="0"  
        />  
      
    <TextView   
        android:id="@+id/test_shadow2"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:textSize="60sp"  
        android:textColor="#ffffffff"  
        android:layout_gravity="center"  
        android:text="Test Shadow"  
        android:shadowColor="#ff000000"  
        android:shadowRadius="1"  
        android:shadowDx="0"  
        android:shadowDy="5"  
        />  
      
    <TextView   
        android:id="@+id/test_shadow3"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:textSize="60sp"  
        android:textColor="#ffffffff"  
        android:layout_gravity="center"  
        android:text="Test Shadow"  
        android:shadowColor="#ff000000"  
        android:shadowRadius="1"  
        android:shadowDx="0"  
        android:shadowDy="-5"  
        />  
```

![](http://img.blog.csdn.net/20141011145513484?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGV3ZW5jZTE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
带一点浮雕效果的，把dx  dy都设置较小的值

现在三组 设置为 （0.2 ， 0.2） ， （1 ， 1） ， （2 ， 2）
![](http://img.blog.csdn.net/20141011150421656?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGV3ZW5jZTE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
光圈效果：

```
 把dx  dy设置为0 ， Raduis位置较大就行了，字体颜色跟阴影颜色要协调（建议使用相同，相近，相差太大就难看比如黑色跟白色）
```

 试验代码：

```
<TextView   
            android:id="@+id/test_shadow"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:textSize="60sp"  
            android:textColor="#ffffffff"  
            android:layout_gravity="center"  
            android:text="Test Shadow"  
            android:shadowColor="#ff00ff00"  
            android:shadowRadius="1"  
            android:shadowDx="0"  
            android:shadowDy="0"  
            />  
       
     <TextView   
            android:id="@+id/test_shadow2"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:textSize="60sp"  
            android:textColor="#ffffffff"  
            android:layout_gravity="center"  
            android:text="Test Shadow"  
            android:shadowColor="#ffee00ff"  
            android:shadowRadius="2"  
            android:shadowDx="0"  
            android:shadowDy="0"  
            />  
         
        <TextView   
            android:id="@+id/test_shadow2"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:textSize="60sp"  
            android:textColor="#ffffffff"  
            android:layout_gravity="center"  
            android:text="Test Shadow"  
            android:shadowColor="#ffeedd00"  
            android:shadowRadius="5"  
            android:shadowDx="0"  
            android:shadowDy="0"  
            />  
        <TextView   
            android:id="@+id/test_shadow2"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:textSize="60sp"  
            android:textColor="#ffffffff"  
            android:layout_gravity="center"  
            android:text="Test Shadow"  
            android:shadowColor="#ff335824"  
            android:shadowRadius="10"  
            android:shadowDx="0"  
            android:shadowDy="0"  
            />
```

![](http://img.blog.csdn.net/20141011151631581?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGV3ZW5jZTE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

荧光灯的效果： 把把dx  dy设置为0 ， Raduis位置较大就行了，最重要的事字体颜色 跟背景颜色一样（或者非常相近）
![](http://img.blog.csdn.net/20141011152357414?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGV3ZW5jZTE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
![](http://img.blog.csdn.net/20141011152420112?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGV3ZW5jZTE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
如果再把dx  跟  dy再设置一下的话 就会变成这样的dx dy 分别为 (1 ,1) , (2 , 2) , (5 , 5) ,(10 , 10)
![](http://img.blog.csdn.net/20141011152536209?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGV3ZW5jZTE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

上面使用的背景色跟字体都是为（#ff895544） 那么我们把字体设置为相近（#ff784433）的那么结果为：

代码：

```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    xmlns:tools="http://schemas.android.com/tools"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    android:orientation="vertical"  
    android:background="#ff895544" >  
  
     <TextView   
            android:id="@+id/test_shadow"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:textSize="60sp"  
            android:textColor="#ff784433"  
            android:layout_gravity="center"  
            android:text="Test Shadow"  
            android:shadowColor="#ff00ff00"  
            android:shadowRadius="1"  
            android:shadowDx="1"  
            android:shadowDy="1"  
            />  
       
     <TextView   
            android:id="@+id/test_shadow2"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:textSize="60sp"  
            android:textColor="#ff784433"  
            android:layout_gravity="center"  
            android:text="Test Shadow"  
            android:shadowColor="#ffee00ff"  
            android:shadowRadius="2"  
            android:shadowDx="2"  
            android:shadowDy="2"  
            />  
         
        <TextView   
            android:id="@+id/test_shadow2"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:textSize="60sp"  
            android:textColor="#ff784433"  
            android:layout_gravity="center"  
            android:text="Test Shadow"  
            android:shadowColor="#ffeedd00"  
            android:shadowRadius="5"  
            android:shadowDx="5"  
            android:shadowDy="5"  
            />  
          
          
          
        <TextView   
            android:id="@+id/test_shadow2"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:textSize="60sp"  
            android:textColor="#ff784433"  
            android:layout_gravity="center"  
            android:text="Test Shadow"  
            android:shadowColor="#ff335824"  
            android:shadowRadius="10"  
            android:shadowDx="10"  
            android:shadowDy="10"  
            />  
</LinearLayout>  
```

![](http://img.blog.csdn.net/20141011152801296?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGV3ZW5jZTE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
这个更明显一点
再把dx  dy 都设置为0
![](http://img.blog.csdn.net/20141011153240405?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGV3ZW5jZTE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

颜色混合：主要是修改字体颜色的alpha值
code:

```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    xmlns:tools="http://schemas.android.com/tools"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    android:orientation="vertical"  
    android:background="#ff000000" >  
  
     <TextView   
            android:id="@+id/test_shadow"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:textSize="60sp"  
            android:textColor="#ffffffff"  
            android:layout_gravity="center"  
            android:text="Test Shadow"  
            android:shadowColor="#ffffff00"  
            android:shadowRadius="5"  
            android:shadowDx="0"  
            android:shadowDy="0"  
            />  
     <TextView   
            android:id="@+id/test_shadow"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:textSize="60sp"  
            android:textColor="#afffffff"  
            android:layout_gravity="center"  
            android:text="Test Shadow"  
            android:shadowColor="#ffffff00"  
            android:shadowRadius="5"  
            android:shadowDx="0"  
            android:shadowDy="0"  
            />  
     <TextView   
            android:id="@+id/test_shadow"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:textSize="60sp"  
            android:textColor="#9fffffff"  
            android:layout_gravity="center"  
            android:text="Test Shadow"  
            android:shadowColor="#ffffff00"  
            android:shadowRadius="5"  
            android:shadowDx="0"  
            android:shadowDy="0"  
            />  
     <TextView   
            android:id="@+id/test_shadow"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:textSize="60sp"  
            android:textColor="#6fffffff"  
            android:layout_gravity="center"  
            android:text="Test Shadow"  
            android:shadowColor="#ffffff00"  
            android:shadowRadius="5"  
            android:shadowDx="0"  
            android:shadowDy="0"  
            />  
     <TextView   
            android:id="@+id/test_shadow"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:textSize="60sp"  
            android:textColor="#3fffffff"  
            android:layout_gravity="center"  
            android:text="Test Shadow"  
            android:shadowColor="#ffffff00"  
            android:shadowRadius="5"  
            android:shadowDx="0"  
            android:shadowDy="0"  
            />  
          
</LinearLayout>  
```

！[](http://img.blog.csdn.net/20141011154817093?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGV3ZW5jZTE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

可以让阴影颜色现在显示在字体颜色中
code:

```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    xmlns:tools="http://schemas.android.com/tools"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    android:orientation="vertical"  
    android:background="#ff000000" >  
  
    <TextView   
            android:id="@+id/test_shadow"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:textSize="60sp"  
            android:textColor="#cc000000"  
            android:text="Test Shadow"  
            android:layout_gravity="center"  
            android:shadowColor="#aa22ff22"  
            android:shadowRadius="10"  
            android:shadowDx="0"  
            android:shadowDy="0"  
            />  
         
     <TextView   
            android:id="@+id/test_shadow"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:textSize="60sp"  
            android:textColor="#aa000000"  
            android:text="Test Shadow"  
            android:layout_gravity="center"  
            android:shadowColor="#aa22ff22"  
            android:shadowRadius="10"  
            android:shadowDx="0"  
            android:shadowDy="0"  
            />  
      <TextView   
            android:id="@+id/test_shadow"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:textSize="60sp"  
            android:textColor="#77000000"  
            android:text="Test Shadow"  
            android:layout_gravity="center"  
            android:shadowColor="#aa22ff22"  
            android:shadowRadius="10"  
            android:shadowDx="0"  
            android:shadowDy="0"  
            />  
       <TextView   
            android:id="@+id/test_shadow"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:textSize="60sp"  
            android:textColor="#33000000"  
            android:text="Test Shadow"  
            android:layout_gravity="center"  
            android:shadowColor="#aa22ff22"  
            android:shadowRadius="10"  
            android:shadowDx="0"  
            android:shadowDy="0"  
            />  
         
        <TextView   
            android:id="@+id/test_shadow2"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:textSize="60sp"  
            android:textColor="#33000000"  
            android:text="Test Shadow"  
            android:layout_gravity="center"  
            android:shadowColor="#aaffffff"  
            android:shadowRadius="10"  
            android:shadowDx="0"  
            android:shadowDy="0"  
            />  
          
        <TextView   
            android:id="@+id/test_shadow2"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:textSize="60sp"  
            android:textColor="#55000000"  
            android:text="Test Shadow"  
            android:layout_gravity="center"  
            android:shadowColor="#ffffffff"  
            android:shadowRadius="10"  
            android:shadowDx="0"  
            android:shadowDy="0"  
            />  
        <TextView   
            android:id="@+id/test_shadow2"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:textSize="60sp"  
            android:textColor="#77000000"  
            android:text="Test Shadow"  
            android:layout_gravity="center"  
            android:shadowColor="#ffffffff"  
            android:shadowRadius="10"  
            android:shadowDx="0"  
            android:shadowDy="0"  
            />  
        <TextView   
            android:id="@+id/test_shadow2"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:textSize="60sp"  
            android:textColor="#99000000"  
            android:text="Test Shadow"  
            android:layout_gravity="center"  
            android:shadowColor="#ffffffff"  
            android:shadowRadius="10"  
            android:shadowDx="0"  
            android:shadowDy="0"  
            />  
        <TextView   
            android:id="@+id/test_shadow2"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:textSize="60sp"  
            android:textColor="#aa000000"  
            android:text="Test Shadow"  
            android:layout_gravity="center"  
            android:shadowColor="#ffffffff"  
            android:shadowRadius="10"  
            android:shadowDx="0"  
            android:shadowDy="0"  
            />  
          
</LinearLayout> 
```

![](http://img.blog.csdn.net/20141011155441171?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvaGV3ZW5jZTE=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
通过改变   字体背景色， 字体颜色  字体阴影色 阴影半径 dx  dy alpha  就可以实现这么多种效果（当然还有更多的效果，主要是颜色搭配）

原来textView也可以这么美，完全不需要使用图片



## 3、ImageView

## 4、RadioButton



关于RadioButton的使用，是单选按钮，在进行布局设计的时候

     ·拖取垂直线性布局

     ·在垂直线性布局中加入，RadioGroup，会自动生成三个RadioButton，如果想要完成合适的RadioButton个数，只需进行增删操作即可。

      此时在线性布局中没有直接使用RadioButton，而使用的是RadioGroup。是因为这样更方便的管理，稍后在MainActivity.java中使用时进行说明。

 ·在布局中加入一个Button按钮。

```Xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical" >
    <RadioGroup
        android:id="@+id/radioGroup_main_sex"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" >

        <RadioButton
            android:id="@+id/radioButton_main_female"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="女" />

        <RadioButton
            android:id="@+id/radioButton_main_male"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:checked="true"
            android:text="男" />
    </RadioGroup>
	 <Button
        android:id="@+id/button_main_submit"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:onClick="clickButton"
        android:text="提交" />
</LinearLayout>
```

## 5、CheckBox

范例：

```Xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical" >

    <CheckBox
        android:id="@+id/checkBox_main_hobby1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="游泳" />

    <CheckBox
        android:id="@+id/checkBox_main_hobby2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="上网" />

    <CheckBox
        android:id="@+id/checkBox_main_hobby3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="音乐" />

    <CheckBox
        android:id="@+id/checkBox_main_hobby4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="睡觉" />

    <Button
        android:id="@+id/button_main_submit"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="提交" />

</LinearLayout>
```

然后编写核心代码：

在MainActivity.java中完成的步骤：

           ·声明四个复选框的对象和一个按钮的对象

           ·在onCreate()方法中实例化他们

           ·定义个取得选择结果的字符串的方法getResult()

                     这个方法中的实现，可以调用CheckBox对象的isChecked()方法，可以得知复选框是否被选中，如果被选中，那么就获取复选框的文本，将它的文本加入到StringBuilder对象中。并返回

           ·在onCreate()方法中为提交按钮加入单击的事件监听器，调用getResult()方法将所有内容显示。

           ·可以创建一个OnCheckedChangeListener接口的对象用来在复选框被选之后处理的时间的监听器对象。

           ·为四个复选框加入在被选择时触发的监听器监听器。

```java
package com.steven.android03.checkbox2;

import android.app.Activity;
import android.os.Bundle;
import android.view.Menu;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.CompoundButton;
import android.widget.CompoundButton.OnCheckedChangeListener;
import android.widget.Toast;

public class MainActivity extends Activity {
	private CheckBox checkBox_main_hobby1;
	private CheckBox checkBox_main_hobby2;
	private CheckBox checkBox_main_hobby3;
	private CheckBox checkBox_main_hobby4;
	private Button button_main_submit;

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		checkBox_main_hobby1 = (CheckBox) findViewById(R.id.checkBox_main_hobby1);
		checkBox_main_hobby2 = (CheckBox) findViewById(R.id.checkBox_main_hobby2);
		checkBox_main_hobby3 = (CheckBox) findViewById(R.id.checkBox_main_hobby3);
		checkBox_main_hobby4 = (CheckBox) findViewById(R.id.checkBox_main_hobby4);
		button_main_submit = (Button) findViewById(R.id.button_main_submit);

		button_main_submit.setOnClickListener(new OnClickListener() {
			@Override
			public void onClick(View v) {
				Toast.makeText(MainActivity.this, "您选择了：" + getResult(),
						Toast.LENGTH_SHORT).show();
			}
		});
		// 定义一个有名字的监听器类。之所以不用匿名内部类形式，是因为有多个控件都要使用这同一个监听器
		OnCheckedChangeListener listener = new OnCheckedChangeListener() {
			@Override
			public void onCheckedChanged(CompoundButton buttonView,
					boolean isChecked) {
				Toast.makeText(MainActivity.this, "您选择了：" + getResult(),
						Toast.LENGTH_SHORT).show();
			}
		};
		checkBox_main_hobby1.setOnCheckedChangeListener(listener);
		checkBox_main_hobby2.setOnCheckedChangeListener(listener);
		checkBox_main_hobby3.setOnCheckedChangeListener(listener);
		checkBox_main_hobby4.setOnCheckedChangeListener(listener);
	}

	// 获取多选项中被勾选的结果。利用isChecked()方法来判断哪个选项被勾选
	private String getResult() {
		StringBuilder sb = new StringBuilder();
		if (checkBox_main_hobby1.isChecked()) {
			sb.append(checkBox_main_hobby1.getText());
		}
		if (checkBox_main_hobby2.isChecked()) {
			sb.append(checkBox_main_hobby2.getText());
		}
		if (checkBox_main_hobby3.isChecked()) {
			sb.append(checkBox_main_hobby3.getText());
		}
		if (checkBox_main_hobby4.isChecked()) {
			sb.append(checkBox_main_hobby4.getText());
		}
		return sb.toString();
	}

	@Override
	public boolean onCreateOptionsMenu(Menu menu) {
		// Inflate the menu; this adds items to the action bar if it is present.
		getMenuInflater().inflate(R.menu.main, menu);
		return true;
	}
}
```

### 5.1、全选与反选

xml布局文件

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical" >

    <CheckBox
        android:id="@+id/checkBox_main_hobby1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="游泳" />

    <CheckBox
        android:id="@+id/checkBox_main_hobby2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="上网" />

    <CheckBox
        android:id="@+id/checkBox_main_hobby3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="音乐" />

    <CheckBox
        android:id="@+id/checkBox_main_hobby4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="睡觉" />
    
    <CheckBox
        android:id="@+id/checkBox_main_selectall"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="全选" />

    <Button
        android:id="@+id/button_main_submit"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="提交" />

</LinearLayout>
```

java文件

```java
package com.steven.android04.checkboxall;

import android.app.Activity;
import android.os.Bundle;
import android.view.Menu;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.CheckBox;
import android.widget.CompoundButton;
import android.widget.CompoundButton.OnCheckedChangeListener;

public class MainActivity extends Activity {
	private CheckBox checkBox_main_hobby1;
	private CheckBox checkBox_main_hobby2;
	private CheckBox checkBox_main_hobby3;
	private CheckBox checkBox_main_hobby4;
	private CheckBox checkBox_main_selectall;
	private Button button_main_submit;

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);

		checkBox_main_hobby1 = (CheckBox) findViewById(R.id.checkBox_main_hobby1);
		checkBox_main_hobby2 = (CheckBox) findViewById(R.id.checkBox_main_hobby2);
		checkBox_main_hobby3 = (CheckBox) findViewById(R.id.checkBox_main_hobby3);
		checkBox_main_hobby4 = (CheckBox) findViewById(R.id.checkBox_main_hobby4);
		checkBox_main_selectall = (CheckBox) findViewById(R.id.checkBox_main_selectall);
		button_main_submit = (Button) findViewById(R.id.button_main_submit);

		button_main_submit.setOnClickListener(new OnClickListener() {

			@Override
			public void onClick(View v) {
				String result = getResult();
				setTitle(result);
			}
		});

		OnCheckedChangeListener listener = new OnCheckedChangeListener() {
			@Override
			public void onCheckedChanged(CompoundButton buttonView,
					boolean isChecked) {
               //全选反选的核心代码
				if (checkBox_main_hobby1.isChecked()
						&& checkBox_main_hobby2.isChecked()
						&& checkBox_main_hobby3.isChecked()
						&& checkBox_main_hobby4.isChecked()) {
					checkBox_main_selectall.setChecked(true);
				} else {
					checkBox_main_selectall.setChecked(false);
				}
				String result = getResult();
				setTitle(result);
			}
		};
		checkBox_main_hobby1.setOnCheckedChangeListener(listener);
		checkBox_main_hobby2.setOnCheckedChangeListener(listener);
		checkBox_main_hobby3.setOnCheckedChangeListener(listener);
		checkBox_main_hobby4.setOnCheckedChangeListener(listener);

		checkBox_main_selectall.setOnClickListener(new OnClickListener() {
			@Override
			public void onClick(View v) {
				boolean flag = checkBox_main_selectall.isChecked();
				checkBox_main_hobby1.setChecked(flag);
				checkBox_main_hobby2.setChecked(flag);
				checkBox_main_hobby3.setChecked(flag);
				checkBox_main_hobby4.setChecked(flag);
			}
		});
	}

	private String getResult() {
		StringBuilder sb = new StringBuilder();
		if (checkBox_main_hobby1.isChecked()) {
			sb.append(checkBox_main_hobby1.getText());
		}
		if (checkBox_main_hobby2.isChecked()) {
			sb.append(checkBox_main_hobby2.getText());
		}
		if (checkBox_main_hobby3.isChecked()) {
			sb.append(checkBox_main_hobby3.getText());
		}
		if (checkBox_main_hobby4.isChecked()) {
			sb.append(checkBox_main_hobby4.getText());
		}
		return sb.toString();
	}

	@Override
	public boolean onCreateOptionsMenu(Menu menu) {
		// Inflate the menu; this adds items to the action bar if it is present.
		getMenuInflater().inflate(R.menu.main, menu);
		return true;
	}

}
```



## 6、Toast

Toast这个类就是用于在桌面上显示提示的信息的一个类。

           在官方的文档如下说明：

           Toast是一个view用于显示一个快捷的小的消息。Toast这个类帮助你创建显示他们、

在Toast中有一个常用的方法：

           ·第一个参数：传入要显示的上下文的对象，如传入Activity.this传入Activity的对象

           ·第二个参数传入要显示的内容的字符串

           ·第三个参数传入要显示的时间，在Toast这个类中定义了两个常量

                     ·public static final int LENGTH_LONG显示的时间略长

                     ·public static final int LENGTH_SHORT：显示的时间略短

**·Toast****的好处：**

                     Toast可以不用在布局中定义控件便可以进行在用户的界面上随时提醒的一个信息条，如果只是一个简单的提示，它可以写在非线程代码中的任何地方。



## 7、Spinner

### 7.1、ArrayAdapter适配器

```java
// 构建适配器。Spinner控件常用ArrayAdapter适配器，只显示文本。
// ArrayAdapter是数组适配器。第一个参数是上下文对象或者说是环境对象，第二个参数是显示数据的布局id，
// 布局id可以自定义布局，也可以使用系统自带的布局。如果使用系统的布局，则使用android.R.layout.的形式来调用。
// 第三个参数是需要加载的数据源数组。至于是哪种类型的数组，取决于ArrayAdapter的泛型类型。
```

### 7.2、OnItemSelectedListener

这个监听器是用来监听Spinner中的列表项是否被选择的监听器。在这个监听的覆写的方法中，要想获得选择的item的文本有一下几个方式：

- 方式一：利用AdapterView的getItemAtPosition(position)获取item的内容

  因为我们已经将数据通过适配器设置到了UI控件上，那么我们就可以通过适配器获

得文本，在适配器的方法的参数中

​	·AdapterView<?> parent 他是次监听器父类的接口,这个监听器中定义了getItemAtPosition方法，可以取得指定位置的字符。

​	·Viewview view对象

​	·**int** position 点击的项目的位置

​	·**long** id ：

- **方式二：**利用AdapterView的getSelectedItem()获取item的内容 
- **方式三：**通过适配器对象的getItem(选择的位置)获取内容
- **方式四：**通过数据源的数组或是集合的位置获得arr[ptsition]，list.get(position)

```java
// 第一种写法，获取数组或数组中的数据，通过position获得
// String data = arr[position] ;
// 第二种写法，
// String data = (String)
// parent.getItemAtPosition(position) ;
// 第三种写法：
// String data = (String) parent.getSelectedItem() ;
// 第四种写法：
// String data = adapter.getItem(position);
// 用switch判断选择的是第几个

```

### 7.3、Spinner使用范例

xml

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:orientation="vertical" >

    <Spinner
        android:id="@+id/spinner_main_edu"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"/>

</LinearLayout>

```

java

```java
package com.steven.android04.spinner2;

import java.util.ArrayList;
import java.util.List;

import android.app.Activity;
import android.os.Bundle;
import android.view.Menu;
import android.view.View;
import android.widget.AdapterView;
import android.widget.AdapterView.OnItemSelectedListener;
import android.widget.ArrayAdapter;
import android.widget.Spinner;

public class MainActivity extends Activity {
	private List<String> list = null;
	private Spinner spinner_main_edu;
	private ArrayAdapter<String> adapter = null;

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		spinner_main_edu = (Spinner) findViewById(R.id.spinner_main_edu);

		list = new ArrayList<String>();
		list.add("初中");
		list.add("高中");
		list.add("大专");
		list.add("本科");
		list.add("硕士研究生");
		adapter = new ArrayAdapter<String>(MainActivity.this,
				android.R.layout.simple_spinner_dropdown_item, list);

		ArrayAdapter<String> adapter2 = new ArrayAdapter<String>(this,
				android.R.layout.simple_list_item_single_choice);
		adapter2.addAll(list);

		spinner_main_edu.setAdapter(adapter2);

		spinner_main_edu
				.setOnItemSelectedListener(new OnItemSelectedListener() {

					@Override
					public void onItemSelected(AdapterView<?> parent,
							View view, int position, long id) {
						// TODO Auto-generated method stub
						// 第1种写法：通过position获取数组或者集合的数据
						String data1 = list.get(position);
						// 第2种写法：通过parent对象获得
						String data2 = (String) parent
								.getItemAtPosition(position);
						// 第3种写法：
						String data3 = parent.getSelectedItem().toString();
						// 第4种写法：
						String data4 = adapter.getItem(position);

						setTitle(data1 + "-" + data2 + "-" + data3 + "-"
								+ data4);
					}

					@Override
					public void onNothingSelected(AdapterView<?> parent) {
						// TODO Auto-generated method stub

					}
				});

	}

	@Override
	public boolean onCreateOptionsMenu(Menu menu) {
		// Inflate the menu; this adds items to the action bar if it is present.
		getMenuInflater().inflate(R.menu.main, menu);
		return true;
	}

}
```



## 8、ListView

### 8.1、SimpleAdapter的使用

SimpleAdapter适配器的使用，可以加入自定义样式

五个参数

- 上下文对象
- 数据源
- 布局样式
- Map的键的字符串数组
- Map键对应的自定义布局中空间的id组成的整形数组

范例：

````java
/*
		 * 常用的SimpleAdapter的构造方法有五个参数：
		 * 
		 * @param context ：表示上下文对象或者环境对象。
		 * 
		 * @param data ：表示数据源。往往采用List<Map<String, Object>>集合对象。
		 * 
		 * @param resource ：自定义的ListView中每个item的布局文件。用R.layout.文件名的形式来调用。
		 * 
		 * @param from ：其实是数据源中Map的key组成的一个String数组。
		 * 
		 * @param to ：表示数据源中Map的value要放置在item中的哪个控件位置上。其实就是自定义的item布局文件中每个控件的id。
		 * 通过R.id.id名字的形式来调用。
*/
SimpleAdapter mSimpleAdapter = new SimpleAdapter(this,mDataList,R.layout.item_testlistview_simpleadapter
                                                 ,new String[]{"username", "password"},new int[] {R.id.text_item_listview_username,R.id.text_item_listview_pwd });
````

### 8.2、监听Item点击

用于ListView的Item点击的监听器是OnItemClickLinstener

通常Item点击的获取数据有四种方式：

```java
//第一种写法，获取数组或数组中的数据，通过position获得
// String data = arr[position] ;
// 第二种写法，
// String data = (String)
// parent.getItemAtPosition(position) ;
// 第三种写法：
// String data = (String) parent.getSelectedItem() ;
// 第四种写法：
// String data = adapter.getItem(position);
// 用switch判断选择的是第几个
```

范例：

```java
 @Override
    public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
        //第一种写法，获取数组或数组中的数据，通过position获得
        // HashMap<String,String> data = mDataList.get(position);

        // 第二种写法，
        //HashMap<String,String> data = (HashMap<String, String>) parent.getItemAtPosition(position);

        // 第三种写法：
        //HashMap<String,String> data = (HashMap<String, String>) parent.getSelectedItem();

        // 第四种写法：
        // String data = adapter.getItem(position);
        HashMap<String,String> data = (HashMap<String, String>) mSimpleAdapter.getItem(position);

        Toast.makeText(this,"postion:" + position + ",dat:" + data.toString(),Toast.LENGTH_LONG).show();
    }
```







## N、SeekBar拖动条控件





# 二、自定义View相关

## 1、基础知识

### 1.1、Matrix

参考链接地址：

http://www.cnblogs.com/qiengo/archive/2012/06/30/2570874.html#code

自定义View系列文章：http://www.gcssloop.com/timeline



#### 1.1.1、矩阵的基本概念

Android中的Matrix是一个3 x 3的矩阵，其内容如下：

![](http://hi.csdn.net/attachment/201111/19/0_13217092330d9Q.gif)

Matrix的对图像的处理可分为四类基本变换：

Translate           平移变换

Rotate                旋转变换

Scale                  缩放变换

Skew                  错切变换

从字面上理解，矩阵中的

MSCALE用于处理缩放变换，

MSKEW用于处理错切变换，

MTRANS用于处理平移变换，

MPERSP用于处理透视变换。

实际中当然不能完全按照字面上的说法去理解Matrix。同时，在Android的文档中，未见到用Matrix进行透视变换的相关说明，所以本文也不讨论这方面的问题。

针对每种变换，Android提供了pre、set和post三种操作方式。其中

set用于设置Matrix中的值。

pre是先乘，因为矩阵的乘法不满足交换律，因此先乘、后乘必须要严格区分。先乘相当于矩阵运算中的右乘。

post是后乘，因为矩阵的乘法不满足交换律，因此先乘、后乘必须要严格区分。后乘相当于矩阵运算中的左乘。



除平移变换(Translate)外，旋转变换(Rotate)、缩放变换(Scale)和错切变换(Skew)都可以围绕一个中心点来进行，如果不指定，在默认情况下是围绕(0, 0)来进行相应的变换的。



#### 1.1.2、平移变换

下面我们来看看四种变换的具体情形。由于所有的图形都是有点组成，因此我们只需要考察一个点相关变换即可。

假定有一个点的坐标是![img](http://hi.csdn.net/attachment/201111/19/0_132170933730wW.gif) ，将其移动到![img](http://hi.csdn.net/attachment/201111/19/0_1321709345NG9E.gif) ，再假定在*x*轴和*y*轴方向移动的大小分别为：

![img](http://hi.csdn.net/attachment/201111/19/0_1321709352RQ75.gif)

如下图所示：

![img](http://hi.csdn.net/attachment/201111/19/0_1321709520MmsS.gif)

不难知道：

![img](http://hi.csdn.net/attachment/201111/19/0_1321709527kmK6.gif)

如果用矩阵来表示的话，就可以写成：

![img](http://hi.csdn.net/attachment/201111/19/0_1321709536Otg4.gif) 



#### 1.1.3、旋转变换

**围绕坐标原点旋转：**

假定有一个点![img](http://hi.csdn.net/attachment/201111/19/0_132170933730wW.gif) ，相对坐标原点顺时针旋转![img](http://hi.csdn.net/attachment/201111/19/0_1321709702OsPP.gif)后的情形，同时假定*P*点离坐标原点的距离为*r*，如下图：

![img](http://hi.csdn.net/attachment/201111/19/0_132170975189NC.gif)

那么，

![img](http://hi.csdn.net/attachment/201111/19/0_1321709797SBJW.gif)

如果用矩阵，就可以表示为：

![img](http://hi.csdn.net/attachment/201111/19/0_1321709849ZLVc.gif)



**围绕某个点旋转**

如果是围绕某个点![img](http://hi.csdn.net/attachment/201111/19/0_13217099588gza.gif)顺时针旋转![img](http://hi.csdn.net/attachment/201111/19/0_1321709702OsPP.gif)，那么可以用矩阵表示为：

![img](http://hi.csdn.net/attachment/201111/19/0_13217100380220.gif)

可以化为：

![img](http://hi.csdn.net/attachment/201111/19/0_13217100952Vqv.gif)

很显然，

1.   ​

  ![img](http://hi.csdn.net/attachment/201111/19/0_1321710153kurQ.gif)是将坐标原点移动到点![img](http://hi.csdn.net/attachment/201111/19/0_13217099588gza.gif)后，![img](http://hi.csdn.net/attachment/201111/19/0_132170933730wW.gif) 的新坐标。

2.     ​

![img](http://hi.csdn.net/attachment/201111/19/0_1321710301T9nf.gif)

是将上一步变换后的![img](http://hi.csdn.net/attachment/201111/19/0_132170933730wW.gif)，围绕新的坐标原点顺时针旋转![img](http://hi.csdn.net/attachment/201111/19/0_1321709702OsPP.gif) 。

3.     ​

![img](http://hi.csdn.net/attachment/201111/19/0_1321710398Z3Je.gif)

经过上一步旋转变换后，再将坐标原点移回到原来的坐标原点。

 

所以，围绕某一点进行旋转变换，可以分成3个步骤，即首先将坐标原点移至该点，然后围绕新的坐标原点进行旋转变换，再然后将坐标原点移回到原先的坐标原点。



#### 1.1.4、缩放变换

理论上而言，一个点是不存在什么缩放变换的，但考虑到所有图像都是由点组成，因此，如果图像在*x*轴和*y*轴方向分别放大*k1*和*k2*倍的话，那么图像中的所有点的*x*坐标和*y*坐标均会分别放大*k1*和*k2*倍，即

![img](http://hi.csdn.net/attachment/201111/19/0_1321710511G7yd.gif)

用矩阵表示就是：

![img](http://hi.csdn.net/attachment/201111/19/0_1321710517pb9W.gif)

缩放变换比较好理解，就不多说了。



#### 1.1.5、错切变换

错切变换(skew)在数学上又称为Shear mapping(可译为“剪切变换”)或者Transvection(缩并)，它是一种比较特殊的线性变换。错切变换的效果就是让所有点的*x*坐标(或者*y*坐标)保持不变，而对应的*y*坐标(或者*x*坐标)则按比例发生平移，且平移的大小和该点到*x*轴(或y轴)的垂直距离成正比。错切变换，属于等面积变换，即一个形状在错切变换的前后，其面积是相等的。

比如下图，各点的*y*坐标保持不变，但其*x*坐标则按比例发生了平移。这种情况将水平错切。

![img](http://hi.csdn.net/attachment/201111/19/0_1321710615riwr.gif)

下图各点的*x*坐标保持不变，但其*y*坐标则按比例发生了平移。这种情况叫垂直错切。

![img](http://hi.csdn.net/attachment/201111/19/0_1321710625smm5.gif) 

假定一个点![img](http://hi.csdn.net/attachment/201111/19/0_132170933730wW.gif)经过错切变换后得到![img](http://hi.csdn.net/attachment/201111/19/0_1321709345NG9E.gif)，对于水平错切而言，应该有如下关系：

![img](http://hi.csdn.net/attachment/201111/19/0_1321710790633H.gif)

用矩阵表示就是：

![img](http://hi.csdn.net/attachment/201111/19/0_1321710798y5L6.gif)

扩展到3 x 3的矩阵就是下面这样的形式：

![img](http://hi.csdn.net/attachment/201111/19/0_13217108084B3T.gif) 

同理，对于垂直错切，可以有：

![img](http://hi.csdn.net/attachment/201111/19/0_13217108954sms.gif)

在数学上严格的错切变换就是上面这样的。在Android中除了有上面说到的情况外，还可以同时进行水平、垂直错切，那么形式上就是：

![img](http://hi.csdn.net/attachment/201111/19/0_13217109074Nv2.gif)

#### 1.1.6、对称变换

除了上面讲到的4中基本变换外，事实上，我们还可以利用Matrix，进行对称变换。所谓对称变换，就是经过变化后的图像和原图像是关于某个对称轴是对称的。比如，某点![img](http://hi.csdn.net/attachment/201111/19/0_132170933730wW.gif) 经过对称变换后得到![img](http://hi.csdn.net/attachment/201111/19/0_1321709345NG9E.gif)，

如果对称轴是x轴，难么，

![img](http://hi.csdn.net/attachment/201111/19/0_1321711018S31a.gif)

用矩阵表示就是：

![img](http://hi.csdn.net/attachment/201111/19/0_1321711026LZ03.gif)

如果对称轴是y轴，那么，

![img](http://hi.csdn.net/attachment/201111/19/0_1321711090fhGd.gif)

用矩阵表示就是：

![img](http://hi.csdn.net/attachment/201111/19/0_1321711099Xhak.gif)

如果对称轴是*y = x*，如图：

![img](http://hi.csdn.net/attachment/201111/19/0_1321711217oHNz.gif)

那么，

![img](http://hi.csdn.net/attachment/201111/19/0_1321711240gEeT.gif)

很容易可以解得：

![img](http://hi.csdn.net/attachment/201111/19/0_1321711261E6xG.gif)

用矩阵表示就是：

![img](http://hi.csdn.net/attachment/201111/19/0_132171128473YK.gif)

同样的道理，如果对称轴是*y = -x*，那么用矩阵表示就是：

![img](http://hi.csdn.net/attachment/201111/19/0_1321711292jO01.gif) 

特殊地，如果对称轴是*y = kx*，如下图：

![img](http://hi.csdn.net/attachment/201111/19/0_13217113506Hb8.gif)

那么，

![img](http://hi.csdn.net/attachment/201111/19/0_1321711502QQ7A.gif)

很容易可解得：

![img](http://hi.csdn.net/attachment/201111/19/0_1321711521GZlt.gif)

用矩阵表示就是：

![img](http://hi.csdn.net/attachment/201111/19/0_1321711541FJA1.gif)

当*k = 0*时，即*y = 0*，也就是对称轴为*x*轴的情况；当*k*趋于无穷大时，即*x = 0*，也就是对称轴为*y*轴的情况；当*k =*1时，即*y = x*，也就是对称轴为*y = x*的情况；当*k = -*1时，即*y = -x*，也就是对称轴为*y = -x*的情况。不难验证，这和我们前面说到的4中具体情况是相吻合的。

 

如果对称轴是*y = kx + b*这样的情况，只需要在上面的基础上增加两次平移变换即可，即先将坐标原点移动到(*0, b*)，然后做上面的关于*y = kx*的对称变换，再然后将坐标原点移回到原来的坐标原点即可。用矩阵表示大致是这样的：

![img](http://hi.csdn.net/attachment/201111/19/0_1321711616I9SJ.gif)

需要特别注意：在实际编程中，我们知道屏幕的*y*坐标的正向和数学中*y*坐标的正向刚好是相反的，所以在数学上*y = x*和屏幕上的*y = -x*才是真正的同一个东西，反之亦然。也就是说，如果要使图片在屏幕上看起来像按照数学意义上*y = x*对称，那么需使用这种转换：

![img](http://hi.csdn.net/attachment/201111/19/0_1321711292jO01.gif)

要使图片在屏幕上看起来像按照数学意义上*y = -x*对称，那么需使用这种转换：
![img](http://hi.csdn.net/attachment/201111/19/0_132171128473YK.gif) 

关于对称轴为*y = kx *或*y = kx + b*的情况，同样需要考虑这方面的问题。







## 2、自定义属性

​	当我们自定义一个View的时候为了使用方便，我们可以自定义一些属性。

### 2.1、属性的定义

​	为一个View提供自定义的属性非常的简单，只需要在res资源目录values目录下创建一个attrs.xml的属性定义文件即可。

范例：TopBar的自定义属性

res/values/attrs.xml

```Xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <declare-styleable name="TopBar">
        <attr name="title" format="string"></attr>
        <attr name="titleTextSize" format="dimension"></attr>
        <attr name="titleTextColor" format="color"></attr>
        <attr name="leftTextColor" format="color"></attr>
        <attr name="leftBackground" format="reference|color"></attr>
        <attr name="leftText" format="string"></attr>
        <attr name="rightTextColor" format="color"></attr>
        <attr name="rightBackground" format="reference|color"></attr>
        <attr name="rightText" format="string"></attr>
    </declare-styleable>
</resources>
```

如上范例：

<resources>标签下的子标签<declare-styleable>标签声明一个自定义属性集合，

name属性用于哪个View

通过<attr>标签来定义一个属性，其中name为属性名称，formart为属性的格式。

formart的属性格式：

| 属性格式      | 说明   |
| --------- | ---- |
| string    | 字符串  |
| color     | 颜色值  |
| reference | 引用   |
| boolean   | 布尔变量 |
| dimension | 尺寸值  |
| enum      | 枚举   |
| flag      | 标志   |
| float     | 浮点数  |
| integer   | 整数   |
| fraction  |      |

有的可以是多种属性的用`|`来分割，如：`color|reference`



### 2.2、属性的取得

在xml中定义完属性后，我们就可以在我们自定义的View中使用这个属性了，一般情况都是在自定义View的构造方法中取得自定义属性。

范例：

```java
//通过这个方法，将你在attrs.xml中定义的declare-styleable的所有属性存储到TypedArray中
TypedArray typedArray = context.obtainStyledAttributes(attrs, R.styleable.TopBar);
```

首先将我们定义的属性存储到TypedArray中！

然后个通过一些列的getXXX方法来取得属性。

范例：

```java
public TopBar(Context context, AttributeSet attrs) {
        super(context, attrs);
        //通过这个方法，将你在attrs.xml中定义的declare-styleable的所有属性存储到TypedArray中
        TypedArray typedArray = context.obtainStyledAttributes(attrs, R.styleable.TopBar);
        //从TypedArray中取出对应的值来为要设置的属性赋值
        mLeftTextColor = typedArray.getColor(R.styleable.TopBar_leftTextColor,0);
        mLeftBackgroud = typedArray.getDrawable(R.styleable.TopBar_leftBackground);
        mLeftTextString = typedArray.getString(R.styleable.TopBar_leftText);

        mRightTextColor = typedArray.getColor(R.styleable.TopBar_rightTextColor,0);
        mRightBackground = typedArray.getDrawable(R.styleable.TopBar_rightBackground);
        mRightTextString = typedArray.getString(R.styleable.TopBar_rightText);

        mTitleTextColor = typedArray.getColor(R.styleable.TopBar_titleTextColor,0);
        mTitleString = typedArray.getString(R.styleable.TopBar_title);
        mTitleTextSize = typedArray.getDimension(R.styleable.TopBar_titleTextSize,10);
  		//释放
        typedArray.recycle();
    }
```

这里特别要注意的是，一定要记着释放TypedArray。



## 3、代码中设置View的属性

### 3.1、设置Gravity属性

设置布局的gravity属性：

在xml中通常有：android:gravity=""属性和android:layout_gravity=""属性

那么在代码中要如何设置：

设置android:gravity属性

```java
button.setGravity(Gravity.CENTER);
```

设置android:layout_gravity属性

```java
LinearLayout.LayoutParams lp = new LinearLayout.LayoutParams(LayoutParams.WRAP_CONTENT,LayoutParams.WRAP_CONTENT);  
//此处相当于布局文件中的Android:layout_gravity属性  
lp.gravity = Gravity.RIGHT;  
button.setLayoutParams(lp);  
```



### 3.2、LayoutParams类

其实这个LayoutParams类是用于child view（子视图） 向 parent view（父视图）传达自己的意愿的一个东西（孩子想变成什么样向其父亲说明）其实子视图父视图可以简单理解成
一个LinearLayout 和 这个LinearLayout里边一个 TextView 的关系 TextView 就算LinearLayout的子视图 child view 。需要注意的是LayoutParams只是ViewGroup的一个内部类这里边这个也就是ViewGroup里边这个LayoutParams类是 base class 基类实际上每个不同的ViewGroup都有自己的LayoutParams子类
比如LinearLayout 也有自己的 LayoutParams 大家打开源码看几眼就知道了



### 3.3、在java代码中向RelativeLayout添加控件

使用Android.view.ViewGroup.LayoutParams 的内嵌类 LayoutParams
RelativeLayout，顾名思义，就是以“相对”位置/对齐为基础的布局方式。android.widget.RelativeLayout 有个 继承自android.view.ViewGroup.LayoutParams 的内嵌类 LayoutParams，使用这个类的实例调用 RelativeLayout.addView 就可以实现“相对布局”。 

首先我们需要定义一个 RelativeLayout的布局参数relLayoutParams，如下：
RelativeLayout.LayoutParams params = new RelativeLayout.LayoutParam(LayoutParams.FILL_PARENT,LayoutParams.WRAP_CONTENT)
     其中LayoutParams中两个参数分别为：
子控件的宽(width)，子控件的高(height),除了可以为LayoutParams.FILL_PARENT(android.view.ViewGroup.LayoutParams)等外还可以是数值;
     下面这里就是重点了：
通过LayoutParams的 addRule 方法来额外的添加别的规则了，android.widget.RelativeLayout.LayoutParams.addRule(int verb, int anchor)，
其中 anchor 参数指定可以是 View 的 id(“相对于谁”)、RelativeLayout.TRUE（启用某种对齐方式）或者 是-1（应用于某些不需要 anchor 的 verb)[因为
RelativeLayout.TRUE的值为 -1 ，所以-1或者RelativeLayout.TRUE都是可以的]、是  0 （不启用这个规则）
     其中 verb 参数指定相对的“动作”；
     如果是相对于父控件的相对布局的话 anchor 参数可以不用或者设置为-1或者RelativeLayout.TRUE ，如果是相对于级别和自己同一级的控件的话参数设置应该是 view 的id ,如果参数设置为 0 的话，则表示这个规则不会运用到该控件的布局中，当是相对于本身的父控件的时候这个参数可以省略。

params.addRule(RelativeLayout.ABOVE,imageViewId.getId())    子控件相对于控件：imageViewId在其的上面
params.addRule(RelativeLayout.BELOW ,imageViewId.getId())  子控件相对于控件：imageViewId在其的下面
 
params.addRule(RelativeLayout.ALIGN_PARENT_RIGHT ,-1) 与
params.addRule(RelativeLayout.ALIGN_PARENT_RIGHT ,RelativeLayout.TRUE) 与
params.addRule(RelativeLayout.ALIGN_PARENT_RIGHT )  表示的是一样的表示子控件在父控件的右边
 
params.setMargins(arg0, arg1, arg2, arg3); 或者 params.topMargin=5 等等离某元素的左、上、右、下 的距离 单位

3.各参数含义

params.alignWithParent=true   如果对应的兄弟元素找不到的话就以父元素做参照物
RelativeLayout.CENTER_HORIZONTAL   在父控件中水平居中
RelativeLayout.CENTER_VERTICAL   在父控件中垂直居中
RelativeLayout.CENTER_IN_PARENT  相对于父控件完全居中
RelativeLayout.ALIGN_PARENT_BOTTOM  紧贴父控件的下边缘
RelativeLayout.ALIGN_PARENT_TOP  紧贴父控件的上边缘
RelativeLayout.ALIGN_PARENT_LEFT 紧贴父控件的左边边缘
RelativeLayout.ALIGN_PARENT_RIGHT  紧贴父控件的右边缘
RelativeLayout.ABOVE  在某元素的上方  需要第二个参数为某元素的ID
RelativeLayout.BELOW 在某元素的下方  需要第二个参数为 某元素的ID
RelativeLayout.LEFT_OF  在某元素的左边  需要第二个参数为某元素的ID
RelativeLayout.RIGHT_OF  在某元素的右边  需要第二个参数为 某元素的ID
RelativeLayout.ALIGN_TOP 本元素的上边缘和某元素的的上边缘对齐 需要第二个参数为 某元素的ID
RelativeLayout.ALIGN_BOTTOM  本元素的上边缘和某元素的的下边缘对齐 需要第二个参数为 某元素的ID
RelativeLayout.ALIGN_LEFT  本元素的上边缘和某元素的的左边缘对齐 需要第二个参数为 某元素的ID
RelativeLayout.ALIGN_RIGHT  本元素的上边缘和某元素的的右边缘对齐 需要第二个参数为 某元素的ID
RelativeLayout.ALIGN_BASELINE    本元素的基线和某元素的的基线对齐 需要第二个参数为 某元素的ID



## 4、View的相关属性和方法





