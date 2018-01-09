---
title: android打包到指定目录
date: 2017-11-17 17:18:27
tags: Android
categories: Android
---
将打出来的包用dradle脚本放到指定的目录下并显示渠道名 如 app-dahai-debug-1.0.apk
```
    applicationVariants.all { variant ->
        variant.outputs.each { output ->
            def outputFile = output.outputFile
            if (outputFile != null && outputFile.name.endsWith('.apk')) {
                def dirName = APP_OUTPUT_DIR + "v${defaultConfig.versionName}"
                def fileName = outputFile.name.replace(".apk", "-${defaultConfig.versionName}.apk")
                output.outputFile = new File(dirName, fileName)
            }
        }
    }
```
```
在gradle.properties文件中定义文件的路劲如 APP_OUTPUT_DIR = F\:\\
```