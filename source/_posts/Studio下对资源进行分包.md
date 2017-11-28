---
layout: android
title: Studio下对资源进行分包
date: 2017-11-28 13:44:06
tags: Android
categories: Android
thumbnail: http://p2.so.qhimgs1.com/t019f9129d3d99e30d2.jpg
---
在项目做大后资源文件不好管理，怎么办呢？今天记录一下资源文件分包 : )
如下

![这里写图片描述](http://img.blog.csdn.net/20171128134654604?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmlnX3NlYV9t/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

是不是发现activity目录有标记了
再来看看具体配置吧
```
android {
    compileSdkVersion 25
    buildToolsVersion '25.0.0'
    aaptOptions.cruncherEnabled = false
    aaptOptions.useNewCruncher = false
   
	...
	
	 sourceSets {
        main {
            res.srcDirs = [
                    'src/main/res',
                    'src/main/res/layouts',
                    'src/main/res/layouts/activity',
                    'src/main/res/layouts/adapteritem',
                    'src/main/res/layouts/base',
                    'src/main/res/layouts/fragment',
                    'src/main/res/layouts/view'
            ]
        }
}
```

注意，将目录创建好后同步一下，工程要开到project模式才可见 ：)