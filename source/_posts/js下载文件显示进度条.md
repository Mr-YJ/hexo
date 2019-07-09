---
title: js下载文件显示进度条
date: 2019-07-08 18:26:00
tags:
---

js下载是ajax去下载二进制文件(转成blob)，具体可参考之前发布的js模拟点击下载文件一文,这次主要是显示下载的进度，js只能下载文件，之后还要浏览器将文件存到本地

```bash
getBlob(url, file) {
  let me = this;
  return new Promise(resolve => {
    // ajax去下载
    const xhr = new XMLHttpRequest();
    xhr.open("GET", url, true);
    xhr.responseType = "blob";
    xhr.onload = () => {
      if (xhr.status === 200) {
        resolve(xhr.response);
      }
    };
    xhr.send();

    // 监听下载进度
    xhr.addEventListener(
      "progress",
      function(evt) {
        //evt.loaded是已经下载的进度，evt.total总进度
        var percentComplete = evt.loaded / evt.total;
      },
      false
    );
  });
}
```