---
title: canvas图片旋转
date: 2019-06-05 14:56:31
tags:
---

想要对canvas里的图片进行操作，只能对canvas对象操作，旋转图片就是要去旋转canvas，其中旋转点是canvas的原点（0,0）点,旋转后对应的x、y轴也会相应的变化，比如旋转90度

![](/images/canvsrotate.png)

<!--more-->

```bash
<canvas class="pic-canvas" width="600" height="600" id="canvas"></canvas>

this.context = document.getElementById("canvas").getContext("2d");
this.context.restore();  // 重置画布状态到第一次save的时候
this.context.clearRect(0, 0, 600, 600); // 清除画布内容

this.context.save()  // 在旋转前先保存当前画布状态
this.context.translate(500, 0); // 为了保证旋转后保证图片还在正中间显示，需要计算偏移量
this.context.rotate((90 * Math.PI) / 180);  // 旋转90°
this.context.drawImage(this.video, 0, 0, 600, 400);  // 从画布0，0点绘制一个x轴为600，y轴为400的图片
```

![](/images/canvasrotate2.png)
![](/images/canvasrotate3.png)
