---
title: Android开发之版本统一规范
date: 2017-11-28 14:22:22
tags: Android
categories: Android
thumbnail: http://p2.so.qhimgs1.com/t019f9129d3d99e30d2.jpg
---
项目做久了后来发现又有更新了，贼麻烦，今天有空记录一下统一资源版本管理
看图 ：）

在project的build.gradle文件中添加版本号

![这里写图片描述](http://img.blog.csdn.net/20171128142628609?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmlnX3NlYV9t/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

在使用的时候就非常方便了

![这里写图片描述](http://img.blog.csdn.net/20171128142810747?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYmlnX3NlYV9t/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

注意！在添加依赖时将`''`改成`""`