---
title: Studio如何进行生成jar或aar
date: 2020-11-04 17:11:25
tags: 技术
categories: Android基础
---

>微信公众号：Lucidastar
如有问题或建议，请公众号留言
最近更新：`2020-11-04`

## 通过Android Studio如何进行生成jar

**==我的Android studio版本是4.1.0==**

### 一.通过Maven仓库中获取对应的jar
  #### **[Maven的搜索地址](https://search.maven.org/)** https://search.maven.org/
>我们以OKHttp为例 

* 输入我们搜索的内容
* 找到相对应的okhttp（我们以4.9.0版本为例子）
  
  >https://search.maven.org/artifact/com.squareup.okhttp3/okhttp/4.9.0/jar
* 在右上角通过download可以下载相对应的jar，还有源码等。
### 二.通过Android Studio来生成jar
1. 创建一个module，phone或者lib都可以
2. 在build.gradle中引入okhttp
 >implementation("com.squareup.okhttp3:okhttp:4.9.0")
3. Sync Project(也就是上面状态栏那个大象按钮)
4. 在Android Studio中，我们就可以在External Libraries中找到相对应的包名，然后找到相对应的jar，右击，打开文件位置，就可以找到相对应的jar了

### 三.通过命令来为我们自己创建的Lib进行生产jar
* **创建一个module，类型是Lib类型的**
* **编写我们的代码**
* **在我们创建的module下的build.gradle中添加如下**
```
task makeJar(type: Copy) {
        //删除存在的
        delete 'build/libs/testLibrary.jar'
        //设置拷贝的文件
        from('build/intermediates/packaged-classes/release/')
        //打进jar包后的文件目录
        into('build/libs/')
        //将classes.jar放入build/libs/目录下
        //include ,exclude参数来设置过滤
        //（我们只关心classes.jar这个文件）
        include('classes.jar')
        //指定打包的class
//        include "com/test/**/*.class"
        //重命名
        rename ('classes.jar', 'testLibrary.jar')
    }
    makeJar.dependsOn(build)
```
* **在Terminal中执行命令,gradlew makeJar就会生产jar**
 >jar 的位置build/intermediates/lib/中 

>aar的位置在build/intermediates/outputs/aar中
>

![Lucidastar](/images/qrcode_for_gh.jpg)