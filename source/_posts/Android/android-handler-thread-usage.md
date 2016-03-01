title: Android HandlerThread使用总结
date: 2016-01-302
tags:
- Android
---

 # 简介
   首先我们看到HandlerThread很快就会联想到Handler。Android中Handler的使用，一般都在UI主线程中执行，因此在Handler接收消息后，处理消息时，不能做一些很耗时的操作，否则将出现ANR错误。
      Android中专门提供了HandlerThread类，来解决该类问题。HandlerThread类是一个线程专门处理Hanlder的消息，依次从Handler的队列中获取信息，逐个进行处理，保证安全，不会出现混乱引发的异常。HandlerThread继承于Thread，所以它本质就是个Thread。与普通Thread的差别就在于，它有个Looper成员变量。
  在看看官方的对他的讲解
  > Handy class for starting a new thread that has a looper. The looper can  then be used to create handler classes. Note that start() must still be called. 
  
  大致意思就是说HandlerThread可以创建一个带有looper的线程。looper对象可以用于创建Handler类来进行来进行调度。
  接下来看看HandlerThread的源码
  
 ```
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
 
线程run()方法当中先调用Looper.prepare()初始化Looper,最后调用Looper.loop()，这样我们就在该线程当中创建好Looper。(注意：Looper.loop()方法默认是死循环).prepare()呢。

**Handler原理**
要理解Handler的原理，理解如下几个概念即可茅塞顿开。
- Message 意为消息，发送到Handler进行处理的对象，携带描述信息和任意数据。
- MessageQueue 意为消息队列，Message的集合。
- Looper 有着一个很难听的中文名字，消息泵，用来从MessageQueue中抽取Message，发送给Handler进行处理。
- Handler 处理Looper抽取出来的Message。

[ Android 异步消息处理机制 让你深入理解 Looper、Handler、Message三者关系](http://blog.csdn.net/lmj623565791/article/details/38377229)

 # 用法
 
 ```
 HandlerThread thread = newHandlerThread("handler_thread");
 thread.start();//必须要调用start方法
 final Handlerhandler = newHandler(thread.getLooper()){
 ```
 其他api
 
 ```
//用于返回与该线程相关联的Looper对象
thread.getLooper();
//获得该线程的Id
thread.getThreadId();
//结束当前的Looper 循环。
thread.quit();
//用于looper取出的消息处理
thread.run();
 ```
 
 # 实例
 **效果图**
 ![](https://raw.githubusercontent.com/Waylenw/AndroidBase/master/screen/handlerThread.gif)
 
 在以上效果图中可以看到当我点击按钮之后，两个蓝色的方块变成了图片。在按钮点击事件中我添加了两个下载图片的任务(模拟情况下)，并在加载完后替换控件的默认图片。很明显很可以看到是有先后顺序的。在第一张图片加载完后第二张图片才会显示。
 
 
 **MainActivity**
 
 ```
 public class MainActivity extends AppCompatActivity {
    private HandlerThread handlerThread;
    private ImageView imageView,imageView1;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        init();
    }

    private void init() {
        imageView= (ImageView) findViewById(R.id.imageView);
        imageView1= (ImageView) findViewById(R.id.imageView1);

        handlerThread = new HandlerThread("MainActivity");
        handlerThread.start();
        final Handler handler = new Handler(handlerThread.getLooper());

        //点击download开始进行下载
        findViewById(R.id.button).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                handler.post(new MyRunable(1));
                handler.post(new MyRunable(2));
            }
        });
    }


    class MyRunable implements Runnable {
        int pos;

        public MyRunable(int pos) {
            this.pos = pos;
        }


        @Override
        public void run() {
            //模拟耗时
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }


            if (pos == 1) {
                imageView.post(new Runnable() {
                    @Override
                    public void run() {
                        imageView.setBackgroundResource(R.mipmap.ic_launcher);
                    }
                });
            } else {
                imageView.post(new Runnable() {
                    @Override
                    public void run() {
                        imageView1.setBackgroundResource(R.mipmap.ic_launcher);
                    }
                });
            }


        }
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        handlerThread.quit();//停止Looper的循环
    }
}
```

**布局文件**

```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="com.example.handlerthread.MainActivity">


    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">

        <ImageView
            android:id="@+id/imageView"
            android:layout_width="100dp"
            android:layout_height="50dp"
            android:background="@android:color/holo_blue_dark" />

        <ImageView
            android:id="@+id/imageView1"
            android:layout_width="100dp"
            android:layout_height="50dp"
            android:layout_marginLeft="10dp"
            android:background="@android:color/holo_blue_dark" />

    </LinearLayout>


    <Button
        android:id="@+id/button"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="downLoad" />

</LinearLayout>
```
**源码地址**
https://github.com/Waylenw/AndroidBase

 
 
 # 总结
 HandlerThread适合在只需要在一个工作线程(非UI线程)+任务的等待队列的形式,优点是不会有堵塞，减少了对性能的消耗，缺点是不能同时进行多任务的处理,需要等待进行处理。处理效率较低。
 
**感谢参考(更多详细)**
http://blog.csdn.net/lmj623565791/article/details/47079737
http://droidyue.com/blog/2015/11/08/make-use-of-handlerthread/?utm_source=tuicool&utm_medium=referral

 
  
  
  
   

 
 
 


