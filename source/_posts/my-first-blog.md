---
title: 多个git账号之间的切换
date: 2019-04-15 14:47:08
tags:
---
多个git账号，有的是github，有的是gitlab

## 生成sshkey
具体参考各网站生成说明

## 主要修改.ssh/config
``` bash
# gitlab
Host gitlab.com
  HostName gitlab.com
  User user1
  IdentityFile ~/.ssh/id_rsa
  
# github
Host github.com
  HostName github.com
  User user2
  IdentityFile ~/.ssh/id_hub_rsa
```
Host后的名字随意，HostName需要git地址