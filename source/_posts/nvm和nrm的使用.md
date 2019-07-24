---
title: nvm和nrm的使用
date: 2019-07-24 18:32:42
tags:
---

## nvm

nvm 可以方便的在同一台设备上进行多个 node 版本之间切换

- nvm 下载： https://github.com/coreybutler/nvm-windows/releases 选择 nvm-setup.zip，下载后直接安装。

* nvm -v 验证是否成功

* 下载指定版本的 nodeJS(可安装多个): nvm install 6.9.0

* 使用指定版本的 nodeJS： nvm use 6.9.0

* nvm list 可以列出所有已经安装的版本

## nrm

nrm 是一个 npm 源管理器，允许你快速地在 npm 源间切换

- 安装 nrm: npm install -g nrm，全局安装 nrm

* nrm ls 可以查看所有的源

* nrm add <名字> url 添加定制的源

* nrm del <名字> 删除对应的源

* nrm use <名字> 切换到指定的源

* nrm test npm 可以测试相应源的响应速度
