---
title: linux基础命令
date: 2019-04-29 17:17:10
tags:
---

## scp远程拷贝文件  
```bash
scp dist.zip root@1.1.1.1:/var/tmp    // 上传本地文件到远程服务器上：scp [文件] [用户]@[ip]:[目标路径]

scp root@1.1.1.1:/var/tmp/dist.zip /downlod   // 从远程服务复制文件到本地：scp [用户]@[ip]:[目标文件] [本地路径]
```

## ssh远程连接登录  
```bash
ssh root@1.1.1.1    // ssh [用户]@[ip]
```

<!--more-->

## ls命令  
查看文件夹包含的文件

## cd命令  
```bash
cd /var/tmp   // cd [目标路径]
```
切换目录

## mkdir命令  
```bash
mkdir manage-data   // 创建manage-data文件夹
```
创建文件夹

## rm命令
删除一个目录中的一个或多个文件或目录，如果没有使用-r选项，则rm不会删除目录
```bash
rm myfile   // 删除指定的文件 

rm *    // 删除当前目录中的所有文件 

rm –r manage-data   // 删除manage-data文件夹
```

## mv命令 
移动文件或修改文件名
```bash
mv a.js b.js    // 将a.js重命名为b.js

mv a.js .    // 将a.js移动到当前文件夹下

mv * ../    // 移动当前文件夹下的所有文件到上一级目录
```

## cp命令  
将源文件复制至目标文件，或将多个源文件复制至目标目录
```bash
cp a.js b.js    // 将a.js复制成b.js

cp -R dir1 dir2    // 将目录dir1复制成dir2
```

## tab按键  
具有命令补全和档案补齐的功能

## vim  
```bash
vim a.js    // 打开a.js文件

i     // 输入i在当前位置前插入

:wq     // 保存并退出

:q!   // 强制退出并忽略所有更改

:e!   // 放弃所有修改，并打开原来文件。
```

## cat命令
查看文件内容
```bash
cat a.js
```

## chmod改变文件权限  
```bash
chmod +x /a.log   // 增加a.log文件所有用户执行权限
```
