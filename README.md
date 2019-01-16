# MyHexo
我的博客

#### 使用hexo写博客
首先安装hexo
```
npm install hexo-cli -g
hexo  // 检查是否安装成功
hexo init blog  // 创建一个blog文件夹，并初始化工程
npm install // 安装依赖
hexo new 博客名  // 生成博客的md文件  在source-_posts目录下
hexo server   // 在本地运行  http://localhost:4000访问地址
```
修改配置_config.yml文件   具体参考https://hexo.io/docs/configuration.html
本文只修改如下配置
```
# Site
title: Hexo
subtitle: dahai subtitle
description: start from dahai
author: 大海
language: zh-CN
timezone: Asia/Shanghai

url: https://BigSea001.github.io

deploy:
  type: git
  repo: https://github.com/BigSea001/BigSea001.github.io.git
  branch: master
```
发布到git还需依赖
```
 npm install hexo-deployer-git --save
```

```
hexo generate  // 打包到public目录
hexo deploy // 发布
```
