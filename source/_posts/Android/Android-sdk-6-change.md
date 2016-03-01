title: Android 6.0的改变
tags:
- Android
---
 **更新ing**
 > 原文链接:http://developer.android.com/intl/zh-cn/about/versions/marshmallow/android-6.0-changes.html

 
 # 运行时权限
 这次的版本引入一个新的权限模型,用户可以在App运行的时候直接管理App的权限。这个模型给了用户改进可见性和控制权限。同时简化了App开发者安装和自动升级的流程。用户可以在已经安装的App进行单独授权或者  撤销权限。

 在你的App设置target6.0(Api 等级23)或者更高。确保在运行时检查和请求权限。调用新的方法[checkSelfPermission()][1]，为已确保你的App已经授予权限。调用[requestPermissions()][2]去请求一个权限。如果你的APP不是target不是Android6.0(Api等级23)。你应该测试你的app在新的权限模型上。更多新的新的权限管理模型的细节在你的应用程序上，[see Working with System Permissionss][4]。如何评估这个提示对你的App的影响，请看[Permissions Best Practices][5]。

  # 通知
 这次版本移除了 Notification.setLatestEventInfo()方法。使用 [Notification.Builder](http://developer.android.com/reference/android/app/Notification.Builder.html) 类而不是用构造参数创建通知。在更新通知的时候重用[Notification.Builder](http://developer.android.com/reference/android/app/Notification.Builder.html) 实例。
 
 调用[build()][6]方法来获得更新 Notification 实例。 adb shell dumpsys notification 该命令不再打印通知的文本。
 使用 adb shell dumpsys notification --noredact命令而不是打印文本通知对象。
 
 # 移除了Apache HTTP Client
 
  Android6.0移除了Apache HTTP Client的支持。如果你的App客户端搭载2.3或者更高的版本。
  你应该使用 HttpURLConnection.这个Api有很多的改变因为它是使用透明压缩和响应缓存。
  并且最大限度的减少你的电力消耗。如果你继续执迷不悟的使用Apache HTTP APIs,你应该在第一时间在编译时依赖的budid.gradle文件中国声明
  ```
  android {
    useLibrary 'org.apache.http.legacy'
   }
  ```
 
 # Runtime运行时(反射有关)
 
  ART运行时现在正确的实现访问[NewInstance][7]方法访问检查的规则。
  这个问题修复了在Dalvik虚拟机在检查访问一些以前不正确的版本的问题。如果你的App在使用 newIntance() 方法以及你想重写访问检查。
  调用 setAccessible() 方法并将传递参数设置为true.如果你的APP使用的v7 appcompat library 或者是 v7 recyclerview library,你必须更新至这些库的最后一个版本。	
 


  [1]: http://developer.android.com/reference/android/content/Context.html#checkSelfPermission(java.lang.String)
  [2]: http://developer.android.com/reference/android/app/Activity.html#requestPermissions(java.lang.String[],int)
  [4]: http://developer.android.com/training/permissions/index.html
  [5]: http://developer.android.com/training/permissions/best-practices.html#testing
  [6]: http://developer.android.com/reference/android/app/Notification.Builder.html#build%28%29
  [7]: http://developer.android.com/intl/zh-cn/reference/java/lang/reflect/Constructor.html#newInstance(java.lang.Object)