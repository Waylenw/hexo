title: Android 图像的拉伸适配解决
date: 2016-01-28
tags:
- Android
---
> 日常开发中，我们避免不了去接触视频图像的展示，但在android屏幕分辨率碎片化如此严重的今天,难免会遇到图片拉伸以及视频拉伸的问题，那该如何解决呢?这就是今天的主题了。

 # 首先看一个例子
 布局文件
 ```
  <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <ImageView
        android:id="@+id/ivImage"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="@drawable/timg"
        />
</RelativeLayout>
 ```
 
很简单就是在一个布局里面添加一个ImgeView的控件，让它充满屏幕。并给它设置一个背景.

效果图是这样
![](https://raw.githubusercontent.com/Waylenw/GithubImg/master/camera/fitimg/fitimg_after.png)
原图是这样的
![](https://raw.githubusercontent.com/Waylenw/GithubImg/master/camera/fitimg/timg.jpeg)
是不是被拉伸了?为什么会出现这样的情况？因为这张图片的宽高是600:375。在宽和高不是等比拉伸的被强制拉伸到屏幕宽高的情况下就出现了这这种现象。如果我们以1080:1920的宽高来对比。宽大概拉伸了1.x呗而高却拉伸了6.x呗。所以肯定会产生拉伸的情况。

我们换一种方法看看，将图片设置为src。看看效果。
![](https://raw.githubusercontent.com/Waylenw/GithubImg/master/camera/fitimg/fitimg_start.png)
你会发现宽被撑满了，而高却没有撑满整个屏幕。这是为什么呢？

这个时候要扯入另一个概念了。在ImagView的onMeasure方法中首先会根据你设置控件的宽高来设置测量方式，由于我们设置的是`match_parent`所以此时的测量方式是宽和高不超过父窗口。也就是子布局的内容可以撑满到父控件的宽高(假设1080：1920)。那么既然测量宽高是屏幕宽高，为什么图片没有拉满整个屏幕。仔细研究你会发现，宽和高都是等比例的放大了，宽度大约放大了1.8倍，而高度遵循这个规则放大了1.8倍。然而放大1.8倍并没有拉伸到屏幕宽度。所以此时imgView的src属性在match_prarent属性下是做等比放大的。

那么问题来了，为什么不已宽度去拉伸了？
因为在View控件的onMeasure中计算好测量高度后都会调用`setMeasuredDimension(heightMeasureSpec,heightMeasureSpec);`
那此时widthMeasureSpec=1080 widthMeasureSpec=1920。如果我以长度为基础去拉伸到1920。然后以此拉伸的倍数去拉伸宽度。结果如下
```
原始宽高	  放大后宽高  放大倍数  
600       3072       5.x倍
375       1920       5.x倍                
```
很明显如果我已长度为基准去拉伸，宽度必须要为3072图片才不是变形的。但是由于View控件在设置的测量宽度小于3072所以由于一边不满足拉伸的条件,所以只做最短拉伸倍数。

 # 改造
 通过上面的一系列研究。基本上明白了这个规则。接下就自己改造一套等比拉伸的ImgView.
 **MainActivity**
 
```
public class MainActivity extends AppCompatActivity {


    private MyFitImageView imageView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        imageView= (MyFitImageView) findViewById(R.id.ivImage);
        Bitmap bitmap=BitmapFactory.decodeResource(getResources(), R.drawable.timg);
        imageView.setImageBitmap(bitmap);
        imageView.setSize(bitmap.getWidth(),bitmap.getHeight());
    }
}
```

**MyFitImageView**
```
public class MyFitImageView extends ImageView {
    private final String TAG="MainActivity";

    private int screenWidth;
    private int screenHeight;
    private int displayWidth;
    private int displayHeight;


    public MyFitImageView(Context context) {
        this(context, null);
    }

    public MyFitImageView(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }

    public MyFitImageView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init();
    }

    private void init() {
        screenWidth = SystemUtils.getDisplayWidth(getContext());
        screenHeight = SystemUtils.getDisplayHeight(getContext());

    }


    public void setSize(int bitmapWidth, int bitmapHeight) {
         displayWidth = screenWidth;
         displayHeight = screenHeight;

        //计算出按比例拉伸后的宽度和高度
        displayWidth = screenHeight * bitmapWidth / bitmapHeight;
        displayHeight = screenWidth * bitmapHeight / bitmapWidth;
        //判断如果以图片高度拉伸到屏幕的高度,按照相应的拉伸比是否能够拉伸超过屏幕宽度或者等于屏幕宽度,否则以另一边为基准

        if(displayWidth>=screenWidth){
            displayHeight=screenHeight;
        }else{
            displayWidth=screenWidth;
        }

        //TODO 拉伸后截取中间的部分
        ViewGroup.MarginLayoutParams lp = (ViewGroup.MarginLayoutParams) getLayoutParams();
        lp.leftMargin = (screenWidth - displayWidth) / 2;
        lp.topMargin = ((screenHeight - displayHeight) / 2);
        setLayoutParams(lp);


        Log.d(TAG, "screenWitdth" + screenWidth);
        Log.d(TAG, "screenHeight:" + screenHeight);
        Log.d(TAG, "fit width:" + displayWidth);
        Log.d(TAG, "fit height:" + displayHeight);
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        setMeasuredDimension(displayWidth,displayHeight);
    }
}
```

**布局文件**

```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <com.example.imgfitdemeo.MyFitImageView
        android:id="@+id/ivImage"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</RelativeLayout>
```

**效果图**
![](https://raw.githubusercontent.com/Waylenw/GithubImg/master/camera/fitimg/fitimg_change.jpeg)
是不是和ImageView的CenterCrop的效果一样呢。此方法还可以适用于视频适配。同样改造surfaceView重写onMeasure方法就可以了。

**源码**
[源码地址](https://github.com/Waylenw/AndroidDemo_Waylen)





  
