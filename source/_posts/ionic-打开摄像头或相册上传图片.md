---
title: ionic+cordova打开摄像头或相册上传图片
date: 2019-07-09 18:35:12
tags:
---

```bash
ionic cordova plugin add cordova-plugin-camera
npm install @ionic-native/camera

this.cameraOptions = {
  quality: 100, //  图片质量0-100
  destinationType: this.camera.DestinationType.DATA_URL, 
  // DATA_URL: 返回base64编码的字符串。DATA_URL可能会占用大量内存，导致应用程序崩溃或内存不足。如果可能的话，使用FILE_URI或NATIVE_URI,这边选用DATA_URL为了能够在界面上预览选择的图片
  // FILE_URI: Return file uri (content://media/external/images/media/2 for Android)
  // NATIVE_URI: Return native uri (eg. asset-library://... for iOS)
  encodingType: this.camera.EncodingType.JPEG,  // 图片格式
  mediaType: this.camera.MediaType.PICTURE,     // 图片还是视频
  saveToPhotoAlbum: true // 是否捕获后将图像保存到设备上的相册中
};
```

<!--more-->

```bash
// 弹出选择框
uploadImage(index: number) {
  let actionSheet = this.actionSheetCtrl.create({
    buttons: [
      {
        text: '摄像头',
        handler: () => {
          this.cameraOptions.sourceType = this.camera.PictureSourceType.CAMERA;
          this.takePicture(index)
        }
      },
      {
        text: '图片库',
        handler: () => {
          this.cameraOptions.sourceType = this.camera.PictureSourceType.SAVEDPHOTOALBUM;
          this.takePicture(index)
        }
      }
    ]
  });

  return actionSheet.present();
}

// 获取图片
takePicture(index: number) {
  this.camera.getPicture(this.cameraOptions)
    .then(path => {
      this.path = 'data:image/jpeg;base64,' + path;
    })
}

```

```bash
// 上传图片
ionic cordova plugin add cordova-plugin-file-transfer
npm install @ionic-native/file-transfer

import { FileTransfer, FileUploadOptions, FileTransferObject } from '@ionic-native/file-transfer/ngx';

constructor(private fileTransfer: FileTransferObject) { }

let options: FileUploadOptions = {
  fileKey: 'upload_file',
  httpMethod: 'POST',
  params: {
    'upload_id': id
  }
};
// filetransfer上传不会带上session的，如果后台检查了会返回code: 1, http-status:403
return this.fileTransfer.upload(this.path,"http:localhost:8080/api/v1/upload/image", options)
```
