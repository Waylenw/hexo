title: hexo搭建博客之环境搭建和配置(一)
date: 2015-09-09
tags:
- hexo
---
 # 简介
   Hexo是由一个台湾的大神弄出来的东东。可以使用此工具来生成个人博客。很神奇高大上的东东。
   大神的博客:http://zespia.tw/
   hexo官网:https://hexo.io/

 # 1. 安装Git
   如何安装git详情查看
 # 2. 安装Node.js
   
  下载：http://nodejs.org/download/

  可以下载 node-v0.10.33-x64.msi
 
  安装时直接保持默认配置即可。
 # 3.环境配置 

 ## 3.1安装Hexo 
 windows鼠标右键==>git bash
 
  ```
  $ npm install -g hexo
  ```
  
   等待安装完毕就可以了
   
   **这里注意:**mac安装的或有不同，详情请参照hexo官网https://hexo.io/zh-cn/docs/index.html

 ## 3.2初始化hexo的文件
 mac初始化hexo文件
  ```
  hexo init hexo-lcx
  ```
  在电脑中建立一个名字叫「Hexo」的文件夹，然后在此文件夹中右键打开Git Bash。执行下面的命令<br>
   ```
   $ hexo init
   ```

 ## 3.3安装依赖包
 先执行下面的代码，一般需要等待一段时间。成功后可以看到xxx文件夹
  ```
  npm install 
  ```
  以下命令都是要执行的
  ```
  npm install hexo-deployer-git
  ```

 ## 3.4启动服务

     hexo server
用浏览器打开http://localhost:4000/或者http://127.0.0.1:4000/就能看到网页了
推荐使用现代化浏览器(Chrome)获得最佳效果
按Ctrl+C停止本地预览服务

 ## 3.5添加一篇自己的文章
 hexo是支持markdown，创建完成后可以进入提示的路径修改文本*.md的内容
  ```$ hexo new "My New Post"
[info] File created at d:\Hexo\source\_posts\My-New-Post.md ```

 执行完命令重复3.4的命令

 # 4.hexo的基本讲解

 ## 4.1 目录结构
```
# Hexo Configuration
## Docs: http://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site #站点信息
title: Waylenwang #标题
subtitle: 专注互联网 #副标题
description: waylenwang #站点描述，给搜索引擎看的
author: waylenwang #作者
email:  #电子邮箱
language: zh-CN #语言

# URL #链接格式
url: http://waylenwang.github.io #网址
root: / #根目录
permalink: :year/:month/:day/:title/ #文章的链接格式
tag_dir: tags #标签目录
archive_dir: archives #存档目录
category_dir: categories #分类目录
code_dir: downloads/code
permalink_defaults:

# Directory #目录
source_dir: source #源文件目录
public_dir: public #生成的网页文件目录

# Writing #写作
new_post_name: :title.md #新文章标题
default_layout: post #默认的模板，包括 post、page、photo、draft（文章、页面、照片、草稿）
titlecase: false #标题转换成大写
external_link: true #在新选项卡中打开连接
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
highlight: #语法高亮
  enable: true #是否启用
  line_number: true #显示行号
  tab_replace:

# Category & Tag #分类和标签
default_category: uncategorized #默认分类
category_map:
tag_map:

# Archives
2: 开启分页
1: 禁用分页
0: 全部禁用
archive: 2
category: 2
tag: 2

# Server #本地服务器
port: 4000 #端口号
server_ip: localhost #IP 地址
logger: false
logger_format: dev

# Date / Time format #日期时间格式
date_format: YYYY-MM-DD #参考http://momentjs.com/docs/#/displaying/format/
time_format: H:mm:ss

# Pagination #分页
per_page: 10 #每页文章数，设置成 0 禁用分页
pagination_dir: page

# Disqus #Disqus评论，替换为多说
disqus_shortname:

# Extensions #拓展插件
theme: landscape-plus #主题
exclude_generator:

plugins: #插件，例如生成 RSS 和站点地图的
- hexo-generator-feed
- hexo-generator-sitemap

# Deployment #部署
deploy:
  type: git
  repo: git@github.com:Waylenwang/Waylenwang.github.io.git   #或者是https://github.com/Waylenwang/Waylenwang.github.io.git 
```
    
## 4.2常用命令

```
hexo help #查看帮助
hexo init #初始化一个目录
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成网页，可以在 public 目录查看整个网站的文件
hexo server #本地预览，'Ctrl+C'关闭
hexo deploy #部署.deploy目录
hexo clean #清除缓存，**强烈建议每次执行命令前先清理缓存，每次部署前先删除 .deploy 文件夹**

```
命令简写
```
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy

```
 # 5.发布
 
 # 5.1创建github仓库
 ```
   名字必须是:   your_github_name.github.io
   例如我的:     Waylenwang.github.io
 ```

 ## 5.2修改_config.yml
编辑全局配置文件 _config.yml 中的 deploy 部分
```
deploy:
  type: git
  repo: git@github.com:Waylenwang/Waylenwang.github.io.git
  branch: master
```
 ## 5.3部署
```
   hexo clean//清理数据缓存，建议每次都清楚
   hexo d //部署

```
部署成功的信息
    [info] Deploy done: github
 ## 5.4成功
   在浏览器输入[http://waylenwang.github.io/](http://waylenw.github.io/)就能看到网页了。
  
  
  
**[hexo搭建博客之更换主题以及博客基本配置(二)](http://waylenw.github.io/hexo/hexo-two/)**


