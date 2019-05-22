---
title: js模拟点击下载文件
date: 2019-05-21 17:13:07
tags:
---

下面这种方法能够下载图片、文件、视频，不会直接打开浏览器，其他下载方法window.open(url)、localtion.href=url会导致chrome直接打开浏览器，但是下面这种下载方法是要经过ajax请求的，会存在跨域的问题

```bash
downloadFile(url, filename) {  // 传入文件路径，文件名称
  this.getBlob(url).then(blob => {
    this.saveAs(blob, filename);
  });
},
getBlob(url) {
  return new Promise(resolve => {
    const xhr = new XMLHttpRequest();

    xhr.open("GET", url, true);
    xhr.responseType = "blob";
    xhr.onload = () => {
      if (xhr.status === 200) {
        resolve(xhr.response);
      }
    };
    xhr.send();
  });
},
saveAs(blob, filename) {
  if (window.navigator.msSaveOrOpenBlob) {
    navigator.msSaveBlob(blob, filename);
  } else {
    const link = document.createElement("a");
    const body = document.querySelector("body");

    link.href = window.URL.createObjectURL(blob);
    link.download = filename;

    // fix Firefox
    link.style.display = "none";
    body.appendChild(link);

    link.click();
    body.removeChild(link);

    window.URL.revokeObjectURL(link.href);
  }
},
```