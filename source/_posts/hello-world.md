---
title: hexo基本命名
date: 2019-04-14 14:47:08
---

### 创建一个新的文件

``` bash
$ hexo new "My-New-Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### 打包

``` bash
$ hexo g
```

### 发布

``` bash
$ hexo d
```
执行后，会自动将生成的页面提交到github上，需要安装hexo-deployer-git
```bash
npm install hexo-deployer-git --save
```
然后在根目录的_config.yml修改deploy
```bash
deploy:
  type: git   
  repo: <上传git地址> 
  branch: master
```

### 启动

``` bash
$ hexo s
```
