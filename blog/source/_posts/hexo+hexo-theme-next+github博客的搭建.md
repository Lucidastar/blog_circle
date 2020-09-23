---
title: hexo+hexo-theme-next+github博客的搭建
date: 2020-09-23 17:34:21
tags: 
  - 技术
  - 了解
categories: hexo
---

每天查看博客，学习知识，今天我们也来搭建自己的博客。

**开始之前，先给大家看张图，无图无真相。**



![image](https://mmbiz.qpic.cn/mmbiz_png/iccib9G9IAFPiao0MKKVyNuF3YVbbqgBfxDYQms1FDJERTkyudBMYFwC5zib97XoelfRrL3KiadUQycnoyYZnAAlRZg/0?wx_fmt=png)
>这个就是我们的最终目标

### 搭建环境
  - 系统  ==windows==
  - node.js  ==版本v12.18.4==  [下载路径](https://nodejs.org/zh-cn/download/)
  - Git [下载路径](https://git-scm.com/downloads)
    
><span style="color:#A52A2A;font-size:1.3em;">nodejs的安装及git的安装这里就不进行详细介绍了</span>
### hexo 是什么？
>**Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。**


<!--more-->
### 开始
  上面环境准备就绪，打开dos命令框。

  #### 安装hexo
  ```
  $ npm install -g hexo-cli
  ```
   #### 验证hexo是否安装成功
  ```
  $ hexo -v
  ```
### 创建博客
  #### 1、创建一个空的文件夹，执行命令
  ```
  $ hexo init
  ```
  #### 2、执行生成命令
  ```
  $ hexo generate
  ```
  #### 3、开启服务
  ```
  $ hexo server
  ```
  #### 4、打开浏览器，输入如下：
  ```
  localhost:4000
  ```
然后就会出现如下界面，说明博客已经创建成功了。
![image](https://mmbiz.qpic.cn/mmbiz_png/iccib9G9IAFPiao0MKKVyNuF3YVbbqgBfxDdbibQPvW6icvobEk12uazbBOcJCM2sSVj6sw7pBgQSqBD50RoOv2YQ5w/0?wx_fmt=png)

## 更换主题
我使用的主题是hexo-theme-nexT  [主题地址](https://github.com/theme-next/hexo-theme-next)

 ### 1、下载主题
 我们通过git来进行克隆下来，首先进入到创建博客的目录，然后执行
 ```
 $ git clone https://github.com/theme-next/hexo-theme-next themes/next
 ```
### 2、配置主题


- 首先我们叫博客的配置文件叫：根配置文件 **=>`创建博客的文件夹下`**
![image](https://mmbiz.qpic.cn/mmbiz_png/iccib9G9IAFPiao0MKKVyNuF3YVbbqgBfxDnYaTkmSyx1AAhDxKicbND7NGeSuQVWDyH6ptKsicKHwsYzKAicJMMJXEw/0?wx_fmt=png)

- 主题的配置文件叫：主题配置文件 **=>`创建博客的文件夹下/theme/next/`**
![image](https://mmbiz.qpic.cn/mmbiz_png/iccib9G9IAFPiao0MKKVyNuF3YVbbqgBfxD8KjP0icDtMjz3gnkCurxRMa8OfFdXQbmczzhic9A9ia6wgZzwQslYVQuA/0?wx_fmt=png)

  ### 配置
#### 1. 打开根配置文件，找到如下：
>#Extensions<br>
##Plugins: https://hexo.io/plugins/<br>
##Themes: https://hexo.io/themes/<br>
theme: next  **(把这里替换成next)**

然后执行创建博客的`第2步`、`第3步` 。接着在浏览器中查看效果，这时候已经改变了。
```
$ hexo g
$ hexo s
```
#### 2. 主题默认是上下结构，我们改为左右结构
- 打开主题配置文件找到如下
>#Schemes<br>
#scheme: Muse<br>
#scheme: Mist<br>
#scheme: Pisces<br>
scheme: Gemini **(把主题设置成Gemini)**

#### 3. 修改头像部分如下：
![image](https://mmbiz.qpic.cn/mmbiz_png/iccib9G9IAFPiao0MKKVyNuF3YVbbqgBfxDaSTNJ9Mq3icvxlKnOX55NfymWKKEkMCuZjKSfXrwMxrLiadI7pCIbqfw/0?wx_fmt=png)

- 修改头像
打开主题配置文件，找到如下
>avatar:<br>
  #Replace the default image and set the url here.<br>
  url: `头像地址(可以是本地，也可以网上图片，我使用的是)`https://mmbiz.qpic.cn/mmbiz_png/iccib9G9IAFPiaRf9CcSkctpUlmbLRHR72wSdllaqf21JwIueIiadYM1mH0RGme31Ral4NhacRUSakMVdp9nl8iciaVw/0?wx_fmt=png<br>
  #If true, the avatar will be dispalyed in circle.<br>
  rounded: true `(是否设置成圆形)`<br>
  #If true, the avatar will be rotated with the cursor.<br>
  rotated: false `(是否进行旋转)`<br>

---

- 修改名称

打开根配置文件，找到如下:  `这里我就不解释了`
>#Site<br>
title: hyli_teacher<br>
subtitle: ''<br>
description: '不要纠结于现在，努力奔跑就好了'<br>
keywords:<br>
author: hyli_teacher<br>
language: zh-CN<br>
timezone: ''<br>
---

#### 4. 打开这些标签

![image](https://mmbiz.qpic.cn/mmbiz_png/iccib9G9IAFPiao0MKKVyNuF3YVbbqgBfxDc3u9cr33CgaLVgWerVHabG60FZiaqtvASnb1hxpwXH0xNY1qiagv0ic7Q/0?wx_fmt=png)

打开主题配置文件，找到：
>menu:<br>
  home: / || fa fa-home`(首页)`<br>
  about: /about/ || fa fa-user`(关于)`<br>
  tags: /tags/ || fa fa-tags`(标签)`<br>
  categories: /categories/ || fa fa-th`(分类)`<br>
  archives: /archives/ || fa fa-archive`(归档)`<br>
  #schedule: /schedule/ || fa fa-calendar<br>
  #sitemap: /sitemap.xml || fa fa-sitemap<br>
  #commonweal: /404/ || fa fa-heartbeat<br>
  ---
  - **搜索的添加**
  在命令框中执行
```
$ npm install hexo-generator-searchdb --save
```
打开根配置文件，在末尾添加
```
search:
  path: search.xml
  field: post
  content: true
  format: html
```
打开主题根配置文件
```
# Local Search
# Dependencies: https://github.com/theme-next/hexo-generator-searchdb
local_search:
  enable: true  这里设置成true就可以了
  # If auto, trigger search by changing input.
  # If manual, trigger search by pressing enter key or search button.
  trigger: auto
  # Show top n results per article, show all results by setting to -1
  top_n_per_article: 1
  # Unescape html strings to the readable one.
  unescape: false
  # Preload the search data when the page loads.
  preload: false
```

---

#### 5. 添加关于、标签、分类的点击
- **添加关于**
```
$ hexo new page about
```
>这时候就会在根目录/source/about/index.md生成这样的文件然后打开这个文件index.md
如下：
```
title: 关于我
date: 2020-09-22 17:49:35
type: about
```
---
- **添加标签**
```
$ hexo new page tags
```
>这时候就会在根目录/source/tags/index.md生成这样的文件然后打开这个文件index.md
如下：
```
title: 标签
date: 2020-09-22 17:49:35
type: tags
```
---
- **添加分类**
```
$ hexo new page categories
```
>这时候就会在根目录/source/tags/index.md生成这样的文件然后打开这个文件index.md
如下：
```
title: 分类
date: 2020-09-22 17:49:35
type: categories
```

#### 6. 创建一个测试，分类是：hexo，标签是：技术和了解
```
$ hexo new post test
```

然后如下：
```
title: other
date: 2020-09-23 17:34:21
tags: 
  - 技术
  - 了解
categories: hexo
```
接着在下面写正文就可以了。
### 发布到github上
#### 1、在github上创建一个仓库，仓库名字必须是：`你的注册名字+github+io`

像我的这样：lucidastar.github.io

#### 2、安装hexo-deployer-git `https://github.com/hexojs/hexo-deployer-git`
- [地址](https://github.com/hexojs/hexo-deployer-git)
- 执行命令
```
$ npm install hexo-deployer-git --save
```
- 修改根配置文件,如下
```
deploy:
  type: 'git'
  repo: 'git@github.com:Lucidastar/lucidastar.github.io.git'(仓库地址)
  branch: 'master'
```
- 执行上传操作
```
$ hexo d
```
- 打开地址

https://你的仓库名字  
这是我的：`https://lucidastar.github.io`