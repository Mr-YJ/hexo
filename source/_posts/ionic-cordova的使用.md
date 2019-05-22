---
title: ionic+cordova的使用
date: 2019-05-17 16:13:12
tags:
---

## 安装使用
![](/images/ionic.png)

三种布局
* tabs: A tabs based layout
* sidemenu: A sidemenu based layout
* blank: An empty project with a single page

<!--more-->

## 添加/移除平台
```bash
ionic cordova platform
ionic cordova platform add ios
ionic cordova platform add android  

ionic cordova platform rm ios
```

## 添加/移除cordova插件
```bash
ionic cordova plugin
ionic cordova plugin add cordova-plugin-inappbrowser@latest
ionic cordova plugin add phonegap-plugin-push --variable SENDER_ID=XXXXX

ionic cordova plugin rm cordova-plugin-camera
```

## app图标和加载图
需要先在resources文件夹下添加icon.png(至少1024×1024px)和splash.png(至少2732×2732px),之后运行以下命令
```bash
ionic cordova resources
ionic cordova resources ios
ionic cordova resources android
```

## 运行加载app
```bash
// 运行app
ionic cordova run android

// 以证书形式运行，生成发布产品模式,需添加证书
ionic cordova run android --prod --release 

// 热加载，修改文件可以实时变化
ionic cordova run android -l

// 修改文件可以实时变化，并且能够打出日志
ionic cordova run android -l -c
```

## 打包apk
```bash
// 打包app
ionic cordova run android

// 证书形式打包，生成发布产品模式,需添加证书
ionic cordova run android --prod --release 

```