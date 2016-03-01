title: Android Studio Buid缓慢问题
date: 2016-01-07
tags:
- Android Studio
---
> Eclipse转到Android Stuido感受最明显的就是buid速度变慢了,对与一些项目庞大一点的的项目稍微改动一两行代码就要buid一分多钟，改动大一点往3分多钟去了。这样太影响开发效率。怎么办？换SSD？别急你可以先试试以下的方法

 # 配置多线程编译
 
 在下面的目录下面创建gradle.properties文件：
 
```
/home/<username>/.gradle/ (Linux)
/Users/<username>/.gradle/ (Mac)
C:\Users\<username>\.gradle (Windows)
```
gradle.properties
 
```
 org.gradle.daemon=true		#开启gradle单独的守护进程
 org.gradle.parallel=true
 org.gradle.jvmargs=-Xmx1024m  #增大gradle运行的java虚拟机的大小
```
 同时上面的这些参数也可以配置到前面的用户目录下的gradle.properties文件里，那样就不是针对一个项目生效，而是针对所有项目生效。


 # 升级Gradle 2.4

Gradle 2.4对执行性能有很大的优化，但Android Studio现在默认使用的是Gradle 2.2,
所以我们需要手动让Android Studio使用Gradle 2.4，在项目根目录下的 build.grade中加入

```
task wrapper(type: Wrapper) {
    gradleVersion = '2.4'
}
```
然后打开终端执行 ./gradlew wrapper，就可以下载Gradle 2.4了，下载完成后，我们需要在
Android Studio让我们的项目使用Gradle 2.4

 # 离线加载
 在不需要加载新的Jencter架包时可以使用离线加载
 ![](http://isming.qiniudn.com/as_gradle_offline.png)
 
 # 打开dex增量编译
这还是一个实验性的功能，但是还是推荐打开试试
在项目主Module下的build.grade中加入
```
dexOptions {
    incremental true
}
```

以上配置完了，是不是明显快了很多。
**感谢参考**
https://www.aswifter.com/2015/06/14/boost-android-studio/
http://my.oschina.net/sammy1990/blog/388846

