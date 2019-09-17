---
title: html导出doc
date: 2019-09-17 14:33:46
tags:
---

## html-docx.js

从 github 下载 html-docx.js 的静态文件
https://github.com/evidenceprime/html-docx-js

## save-as

```bash
npm install save-as --save
```

## 导出

```bash
import $ from 'jquery';
import saveAs from 'save-as';
import htmlDocx from './static/html-docx.js';

let content = '<H1 style="text-align: center">文档标题</H1>';

var converted = htmlDocx.asBlob(content);
saveAs(
  converted,
  '文档.docx'
);

## htmlDocx还有其他参数，具体可以参考github网站说明
## orientation: landscape or portrait (default)
## margins: map of margin sizes (expressed in twentieths of point, see WordprocessingML documentation for details):
## top: number (default: 1440, i.e. 2.54 cm)
## right: number (default: 1440)
## bottom: number (default: 1440)
## left: number (default: 1440)
## header: number (default: 720)
## footer: number (default: 720)
## gutter: number (default: 0)

```
