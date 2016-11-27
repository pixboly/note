> Android 动画笔记
# 1、差值器
[http://blog.csdn.net/qiujuer/article/details/42430269]
序

一个好的动画一定是用心做出来的，何为用心？其中一点我认为定义适当的 Interpolator 就是一种用心的表现；这点在 google material design 中尤为明显。
一个好的动画一定要符合实际，一句老的话就是：石头下落一定要受重力才优雅，不然一颗石头像羽毛一样在风中还飘啊飘的那就不行了。
介绍

Interpolator 是个什么东西？

说到 Interpolator 啊，这个要从3年前说起，话说当年谷歌、诺基亚、Qiujuer 三分天下.... 哈哈，开个玩笑~
Interpolator 这个时间插值类，其主要使用在动画中，其作用主要是控制目标变量的变化值进行对应的变化。
你可以这么理解，现在小明去买酱油，规定时间是1个小时到达，里程是1公里；现在小明心里唯恐无法达到，所以先跑起来了，但因为体力消耗所以逐渐的慢下来了；然后成功到达。这样的一个过程中把小明逐渐慢下来的这个过程抽象出来也就是 Interpolator  的工作；当然 Interpolator 也可以控制小明先慢慢热身然后越跑越快最后达到。
这些都是 Interpolator 能完成的工作，同样 Interpolator 还能控制一个弹球掉在地上弹起来逐渐降低的过程，这些都是可以控制的。
Interpolator 的原理？

```
public interface Interpolator extends TimeInterpolator {  
}
可以看见这个类其是是一个空的类，那么其操作在哪里？
/** 
 * A time interpolator defines the rate of change of an animation. This allows animations 
 * to have non-linear motion, such as acceleration and deceleration. 
 */
public interface TimeInterpolator {  
  
    /** 
     * Maps a value representing the elapsed fraction of an animation to a value that represents 
     * the interpolated fraction. This interpolated value is then multiplied by the change in 
     * value of an animation to derive the animated value at the current elapsed animation time. 
     * 
     * @param input A value between 0 and 1.0 indicating our current point 
     *        in the animation where 0 represents the start and 1.0 represents 
     *        the end 
     * @return The interpolation value. This value can be more than 1.0 for 
     *         interpolators which overshoot their targets, or less than 0 for 
     *         interpolators that undershoot their targets. 
     */  
    float getInterpolation(float input);  
}
```
其操作在所继承的接口中，在所继承的接口中有一个方法 float getInterpolation(float input);
在这个方法中，传入的值是一个0.0~1.0的值，返回值可以小于0.0也可以大于1.0。
你可以这么理解：在Animation中时间是正常的走的，你设置了200ms，现在走到了100ms了，那么按照线性来说现在应该是走了一半的路程也就是0.5；现在就把这0.5传递给Interpolator 让 Interpolator 告诉我走到一半时间的时候此时小明在哪里；这也就是 Interpolator 的原理。
常用类

哎呀我的天天啊，访问不了谷歌就是麻烦，只能从源码截图了：
![](http://note.youdao.com/yws/public/resource/4399b4cc0a8b342c45bd3f8e0c2a3407/05C87680705E4DAFBE201EA33372FC11)

Android 官方提供的就是这么十种，是9种还是10种啊，没有数错吧。分别是：
AccelerateDecelerateInterpolator 在动画开始与结束的地方速率改变比较慢，在中间的时候加速
AccelerateInterpolator  在动画开始的地方速率改变比较慢，然后开始加速   
AnticipateInterpolator 开始的时候向后然后向前甩
AnticipateOvershootInterpolator 开始的时候向后然后向前甩一定值后返回最后的值
BounceInterpolator   动画结束的时候弹起
CycleInterpolator 动画循环播放特定的次数，速率改变沿着正弦曲线
DecelerateInterpolator 在动画开始的地方快然后慢
LinearInterpolator   以常量速率改变
OvershootInterpolator    向前甩一定值后再回到原来位置
PathInterpolator 这个是新增的我说原来怎么记得是9个，这个顾名思义就是可以定义路径坐标，然后可以按照路径坐标来跑动；注意其坐标并不是 XY，而是单方向，也就是我可以从0~1，然后弹回0.5 然后又弹到0.7 有到0.3，直到最后时间结束。（这个后面单独说说）
源码

这里说几个简单的源码
LinearInterpolator

```
@HasNativeInterpolator  
public class LinearInterpolator implements Interpolator, NativeInterpolatorFactory {  
  
    public LinearInterpolator() {  
    }  
      
    public LinearInterpolator(Context context, AttributeSet attrs) {  
    }  
      
    public float getInterpolation(float input) {  
        return input;  
    }  
  
    /** @hide */  
    @Override  
    public long createNativeInterpolator() {  
        return NativeInterpolatorFactoryHelper.createLinearInterpolator();  
    }  
}
```
最简单的一个由于是线性，所以直接返回。
DecelerateInterpolator

```
public class DecelerateInterpolator implements Interpolator, NativeInterpolatorFactory {  
    public DecelerateInterpolator() {  
    }  
  
    /** 
     * Constructor 
     * 
     * @param factor Degree to which the animation should be eased. Setting factor to 1.0f produces 
     *        an upside-down y=x^2 parabola. Increasing factor above 1.0f makes exaggerates the 
     *        ease-out effect (i.e., it starts even faster and ends evens slower) 
     */  
    public DecelerateInterpolator(float factor) {  
        mFactor = factor;  
    }  
  
    public DecelerateInterpolator(Context context, AttributeSet attrs) {  
        this(context.getResources(), context.getTheme(), attrs);  
    }  
  
    /** @hide */  
    public DecelerateInterpolator(Resources res, Theme theme, AttributeSet attrs) {  
        TypedArray a;  
        if (theme != null) {  
            a = theme.obtainStyledAttributes(attrs, R.styleable.DecelerateInterpolator, 0, 0);  
        } else {  
            a = res.obtainAttributes(attrs, R.styleable.DecelerateInterpolator);  
        }  
  
        mFactor = a.getFloat(R.styleable.DecelerateInterpolator_factor, 1.0f);  
  
        a.recycle();  
    }  
  
    public float getInterpolation(float input) {  
        float result;  
        if (mFactor == 1.0f) {  
            result = (float)(1.0f - (1.0f - input) * (1.0f - input));  
        } else {  
            result = (float)(1.0f - Math.pow((1.0f - input), 2 * mFactor));  
        }  
        return result;  
    }  
  
    private float mFactor = 1.0f;  
  
    /** @hide */  
    @Override  
    public long createNativeInterpolator() {  
        return NativeInterpolatorFactoryHelper.createDecelerateInterpolator(mFactor);  
    }  
}
```
从其中可以看出，其并不是一个简单的类，其是是可以通过 XML 进行设置的类，通过 XML 可以设置其中的 mFactor 变量，其值默认是1.0；值越大其变化越快；得到的结果就是，开始的时候更加的快，其结果就是更加的慢，好比一个人开始跑的很快，但是换来的就是后面的路程将会花更多时间慢慢走。

在方法
```
public float getInterpolation(float input) {  
    float result;  
    if (mFactor == 1.0f) {  
        result = (float)(1.0f - (1.0f - input) * (1.0f - input));  
    } else {  
        result = (float)(1.0f - Math.pow((1.0f - input), 2 * mFactor));  
    }  
    return result;  
}
```
其描述的是一个初中学的抛物方程（话说是初中吧），y = x^2 我擦不知道怎么弄上去，就这样吧；意思懂就OK。
由于篇幅就说这么两个；下面说说其他东西。
动画表
![](http://note.youdao.com/yws/public/resource/4399b4cc0a8b342c45bd3f8e0c2a3407/B51C5058ACEA420C970AB6FFE9733134)


这个图片相信前段时间看的不少吧？前段时间 material design 刚刚出来的时候好多人说这个啊，但是好像都是说图，但是没有说说其如何实现吧。
实现

这里送上福利，其是最开始我发现的是 C++ 的版本：
```
float Elastic::easeIn (float t,float b , float c, float d) {  
 if (t==0) return b;  if ((t/=d)==1) return b+c;    
 float p=d*.3f;  
 float a=c;   
 float s=p/4;  
 float postFix =a*pow(2,10*(t-=1)); // this is a fix, again, with post-increment operators  
 return -(postFix * sin((t*d-s)*(2*PI)/p )) + b;  
}  
   
float Elastic::easeOut(float t,float b , float c, float d) {  
 if (t==0) return b;  if ((t/=d)==1) return b+c;    
 float p=d*.3f;  
 float a=c;   
 float s=p/4;  
 return (a*pow(2,-10*t) * sin( (t*d-s)*(2*PI)/p ) + c + b);   
}  
   
float Elastic::easeInOut(float t,float b , float c, float d) {  
 if (t==0) return b;  if ((t/=d/2)==2) return b+c;   
 float p=d*(.3f*1.5f);  
 float a=c;   
 float s=p/4;  
   
 if (t < 1) {  
 float postFix =a*pow(2,10*(t-=1)); // postIncrement is evil  
 return -.5f*(postFix* sin( (t*d-s)*(2*PI)/p )) + b;  
 }   
 float postFix =  a*pow(2,-10*(t-=1)); // postIncrement is evil  
 return postFix * sin( (t*d-s)*(2*PI)/p )*.5f + c + b;  
}
```
参数的意思：
t – 动画中当前的时间
b – 开始值
c – 结束值
d – 动画的总时间
看看 Java 的 第一行前三个的：
```
public class Sine {  
      
    public static float  easeIn(float t,float b , float c, float d) {  
        return -c * (float)Math.cos(t/d * (Math.PI/2)) + c + b;  
    }  
      
    public static float  easeOut(float t,float b , float c, float d) {  
        return c * (float)Math.sin(t/d * (Math.PI/2)) + b;    
    }  
      
    public static float  easeInOut(float t,float b , float c, float d) {  
        return -c/2 * ((float)Math.cos(Math.PI*t/d) - 1) + b;  
    }  
      
}
```
虽然 Java 的也有了，但是话说这个怎么用啊，跟上面的Interpolator如何联系起来啊？
一个简单的方法：首先把 d 总时间设置为固定值 1.0 ，把 b 开始值设置为 0.0 把结束值设置为1.0，然后把 t 当作上面 Interpolator 中的 float getInterpolation(float input);传入值，此时不就能用上了？对不？
举个Case

```
/** 
 * Created by Qiujuer on 2015/1/5. 
 */  
public class InSineInterpolator implements Interpolator{  
    public static float  easeIn(float t,float b , float c, float d) {  
        return -c * (float)Math.cos(t/d * (Math.PI/2)) + c + b;  
    }  
  
    @Override  
    public float getInterpolation(float input) {  
        return easeIn(input, 0, 1, 1);  
    }  
}
```
使用

```
//AnimatorSet  
mAnimatorSet = new AnimatorSet();  
mAnimatorSet.playTogether(aPaintX, aPaintY, aRadius, aBackground);  
mAnimatorSet.setInterpolator(new InSineInterpolator());  
mAnimatorSet.start();
```
可以看出使用与上面 Android 自带的完全一样，当然这个只是一个 Case ，具体使用中你可以随意封装，前提是别改动了主要部分。

好了，完成了，擦又是三个小时过去了，我的 LOL 又没法打了。
最后送上福利，全部的实现类：
![](http://note.youdao.com/yws/public/resource/4399b4cc0a8b342c45bd3f8e0c2a3407/5FACA90862084199A6DFDF4602C60B56)


[下载地址](http://download.csdn.net/detail/qiujuer/8330259)

Animation Interpolator.zip
愿大家都能做出自己满意的动画！

——学之开源，用于开源；初学者的心态，与君共勉！