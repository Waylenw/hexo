title: hexo搭建博客之更换主题以及博客基本配置(二)
tags:
- hexo
---
 # 更换主题
 >在经过上一篇的博客搭建后，很多是否对默认的主题并不是很满意，是否希望可以打造一款属于自己风格的博客主题呢？
 
 **有哪些主题可以更换**
 [github上的hexo的主题大全](https://github.com/hexojs/hexo/wiki/Themes)
 [知乎上的hexo的主题排行版](http://www.zhihu.com/question/24422335/answer/46357100)
 [hexo官方主题](https://hexo.io/themes/)

  **如何进行更换主题**
  [如何使用 Jacman 主题](http://wuchong.me/jacman/2014/11/20/how-to-use-jacman/#more)

 # 主题和博客的相关配置
 **添加导航项目**
 
以我的路径为例:`G:\Hexo\themes\jacman\_config.xml`主题默认的两个导航是主页和`归档: /archives`后是导航的访问路径。前面是导航的名称	根据这个规律自行添加导航和访问路径即可
 
 ```
 ##### Menu  导航项
 menu:
  主页: /      
  归档: /archives
  摄影: /cameras
  About: /abouts

 ```
 **为导航项设置加载页面**
 
 archives导航是默认开启的，archives导航下的文章默认的目录是`source\_posts\`在此文件夹下新建xx.md文件。归档下就会显示相应的文章item
 cameras和abouts导航对应的文件夹是`sourc/cameras和source/abouts`。并在此目录下新建index.md.之后点击对应的menus时就默认加载目录下index.md文件
 
 # 生成rss
 
 生成插件
 
 ```
  npm install hexo-generator-feed --save

 ```
 
 配置`hexo/_config.xml`(详情可以查看第一篇时提供的_config.xml)
 
 
 ```	
plugins: #插件，例如生成 RSS 和站点地图的- hero-generator-feed- hexo-generator-sitemap
 ```
 这里要注意的是该配置必须在themes设置的前面配置,否则不起作用。配置完后clean一下重新生成一下就好。
 最后需要在导航上添加你的RSS链接。具体查看相关的主题配置。
 
 以下是RSS的链接(比如我的),部署到服务器,输入此链接看是否生效
 ```
 waylenw.github.io/atom.xml
 ```
    