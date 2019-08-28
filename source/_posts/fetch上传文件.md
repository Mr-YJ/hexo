---
title: fetch上传文件
date: 2019-08-28 17:43:36
tags:
---

fetch 上传文件不需要指定 headers 的 content-type

```bash
let formData = new FormData();
formData.append('upload_file', event.target.files[0]);
formData.append('upload_id', id);
fetch('/upload', {
  method: 'post',
  # headers: {
  #   'Content-Type': 'multipart/form-data',
  # },
  body: formData,
  }).then(response => response.json())
  .then((data) => {
      console.log(data);
  });
}
```

<!--more-->

在 rfc1867 协议中规定，我们在指定 content-type 为 multipart/form-data 时，表明我们需要传递的是多媒体内容，我们需要指定 boundary【分隔符】来分割当上传多个文件/图片。boundary 是随机生成的数字和字母的组合。

如果指定了 headers 就不会生成 boundary
![](/images/fetch-upload.png)

不指定 headers 就会随机生成 boundary
![](/images/fetch-upload2.png)
