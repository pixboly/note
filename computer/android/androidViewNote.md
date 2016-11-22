> Android View 笔记

# 一、系统控件笔记

## 1、ViewGroup相关

##  2、View相关

### SeekBar拖动条控件





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

1.   

  ![img](http://hi.csdn.net/attachment/201111/19/0_1321710153kurQ.gif)是将坐标原点移动到点![img](http://hi.csdn.net/attachment/201111/19/0_13217099588gza.gif)后，![img](http://hi.csdn.net/attachment/201111/19/0_132170933730wW.gif) 的新坐标。

2.     

![img](http://hi.csdn.net/attachment/201111/19/0_1321710301T9nf.gif)

是将上一步变换后的![img](http://hi.csdn.net/attachment/201111/19/0_132170933730wW.gif)，围绕新的坐标原点顺时针旋转![img](http://hi.csdn.net/attachment/201111/19/0_1321709702OsPP.gif) 。

3.     

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

​            



