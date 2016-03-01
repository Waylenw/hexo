title: Android SurfaceView的基本介绍(一)
date: 2015-12-26
tags:
- Android
---
 # 概述
- **官方APi的描述**
 SurfaceView是视图(View)的继承类，这个视图里内嵌了一个专门用于绘制的Surface。你可以控制这个Surface的格式和尺寸。Surfaceview控制这个Surface的绘制位置。surface是纵深排序(Z-ordered)的，这表明它总在自己所在窗口的后面。surfaceview提供了一个可见区域，只有在这个可见区域内 的surface部分内容才可见，可见区域外的部分不可见。surface的排版显示受到视图层级关系的影响，它的兄弟视图结点会在顶端显示。这意味者 surface的内容会被它的兄弟视图遮挡，这一特性可以用来放置遮盖物(overlays)(例如，文本和按钮等控件)。注意，如果surface上面 有透明控件，那么它的每次变化都会引起框架重新计算它和顶层控件的透明效果，这会影响性能。
- **定义**
 SurfaceView是View类的子类，可以直接从内存或者DMA等硬件接口取得图像数据，是个非常重要的绘图视图。它的特性是：可以在主线程之外的线程中向屏幕绘图上。这样可以避免画图任务繁重的时候造成主线程阻塞，从而提高了程序的反应速度。在游戏开发中多用到SurfaceView，游戏中的背景、人物、动画等等尽量在画布canvas中画出。
- **绘制机制**
 更详细的讲解:http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2012/1201/656.html
 # 实现
 
 ```
 
 public class MainActivity extends Activity {
 	private SurfaceView surfaceView
 
　　　 @Override
      public void onCreate(Bundle savedInstanceState) {
         super.onCreate(savedInstanceState);
         surfaceView=new SurfaceView(this);
         setContentView(surfaceView);
         surfaceView.getHolder().addCallback(callback);
      }
 
     
    /**
     * surface的回调
     */
    private SurfaceHolder.Callback callback = new SurfaceHolder.Callback() {
        @Override
        public void surfaceDestroyed(SurfaceHolder holder) {
        //在surface销毁的时候调用

        }

        @Override
        public void surfaceCreated(SurfaceHolder holder) {
            //在创建时激发，一般在这里调用画图的线程。
                 

        }

        @Override
        public void surfaceChanged(SurfaceHolder holder, int format,
                                   int width, int height) {
         //销毁时激发，一般在这里将画图的线程停止、释放。

        }

     };　　
    }      　　
   }
 ```

 
 