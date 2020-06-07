---
title: 搭建hexo next主题博客
categories: 
  - 前端
tags:
  - 工具
---
<!--more-->
## 添加网页音乐播放器功能
https://asdfv1929.github.io/2018/05/26/next-add-music/
[音乐外链生成网站](https://www.365cent.com/music/)

## 添加评论功能、全局搜索
我用的畅言  密码是12345678lj， 搜索功能安装包，然后配置下

## tagCloud
https://blog.csdn.net/aoman_hao/article/details/89416634 
```
npm i hexo-tag-cloud --save //配置
```

## 添加动漫人物
https://godweiyang.com/2018/04/13/hexo-blog/#toc-heading-22
```
npm i hexo-helper-live2d --save
npm i live2d-widget-model-shizuku --save  //配置
```
## 自定义友链页面
https://blog.maplesugar.space/hexo/next-theme-custom-friendlinks-page/

## hexo博客生成、部署
```
hexo n          # 新建文章，在\source\_posts文件夹里
hexo new page   # 新建页面，比如想在导航栏新增一个“关于我”的页面
hexo clean      # 清除本地的数据库和生成的public文件夹
hexo g          # 生成博客文件
hexo s          # 运行在本地浏览器，可当预览使用
hexo d          # 部署博客到Github等
```

## 其他想尝试的hexo主题
ghost、hexo-theme-yilia


