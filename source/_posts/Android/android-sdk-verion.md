title: Android SDK版本与兼容包相关
date: 2016-01-18
tags:
- Android
---
> Android开发的童鞋们都知道，Google的碎片化是很严重的，所以官方提供了兼容包去试配一些低版本的机器。并且兼容包也在更新，除此之外，Android的SDK版本更新也十分的快速，基本上是一年发一个大的版本号。而且Api的变更也十分大，但是很多时候对这些变更感到无从下手。以下链接的这篇文章对SDK版本更新的内容都会有一个汇总。相信你可以很快熟悉各版本之间的差异。

 # SDK版本相关
 > [官方SDK版本Api变动列表:(6.0为例)](https://developer.android.com/intl/zh-cn/sdk/api_diff/23/changes.html)
 [Android API Level所有版本列表](http://developer.android.com/intl/zh-cn/guide/topics/manifest/uses-sdk-element.html#ApiLevels)
 
 ## 4.X
 [Android 4.1 API官方文档详解](http://my.oschina.net/u/1175378/blog/143448?fromerr=px75uTtO)
 
 ## 5.X
 [Android 5.0 Api的变化](http://blog.csdn.net/github_25928675/article/details/46648381)
 [Android 5.0 camera2Api的使用](http://blog.csdn.net/github_25928675/article/details/46344791)
 [Android Palette取色器](http://wuxiaolong.me/2015/08/03/Palette/)
 [VectorDrawable与SVG](http://blog.csdn.net/xu_fu/article/details/44004841)
 
 
 ## 6.X
[Android 6.0 中新的新技术](http://www.race604.com/whats-new-in-android6-0/)
[Android 6.0的变化](http://waylenw.github.io/Android/Android-sdk-6-change/)
[运行时权限开发者需要知道的一切](http://jijiaxin89.com/2015/08/30/Android-s-Runtime-Permission/)

 # 兼容库相关
 > 兼容库和兼容包是什么？兼容库和兼容包是对Android一些更低版本实现高版本的功能所
   提供的兼容架包。
   [官方Support Library版本更新汇总](http://developer.android.com/intl/zh-cn/tools/support-library/index.html)
 
 ## Support-V4
 在项目中引入
 ```
 compile ('com.android.support:support-v4:13.0.0')
 ```
 [NestedScrolling 实战:(5.0加入)](http://mp.weixin.qq.com/s?__biz=MzA3MDMyMjkzNg==&mid=212038361&idx=1&sn=ffcb677d058ecd7ff291a8a42d449566&scene=0#rd)
 
 ## Support-V7
 [Android RecyclerView三部曲之基础篇(一)](http://waylenw.github.io/Android/android-recyclerview-one/)
 
 ##  Android Design Support Library
 [NavigationView的实用讲解](http://blog.csdn.net/lmj623565791/article/details/46405409)
 [TabLayout的使用讲解](http://wuxiaolong.me/2015/08/03/TabLayout/)
 
 ## 百分比库
 [Android 百分比布局库(percent-support-lib) 解析与扩展](http://blog.csdn.net/lmj623565791/article/details/46695347)
 
 


