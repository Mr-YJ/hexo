---
title: js导出pdf
date: 2019-05-22 17:01:37
tags:
---

* html2canvas 将html页面转换为图片
* jsPDF 将图片导出pdf

```bash
npm install jspdf --save
npm html2canvas jspdf --save

import jsPDF from "jspdf";
import html2canvas from "html2canvas";

html2canvas(document.getElementById("export_content")).then(canvas => {
  var contentWidth = canvas.width;
  var contentHeight = canvas.height;

  //一页pdf显示html页面生成的canvas高度;
  var pageHeight = (contentWidth / 592.28) * 841.89;
  //未生成pdf的html页面高度
  var leftHeight = contentHeight;
  //pdf页面偏移
  var position = 0;
  //html页面生成的canvas在pdf中图片的宽高（a4纸的尺寸[595.28,841.89]）
  var imgWidth = 595.28;
  var imgHeight = (592.28 / contentWidth) * contentHeight;

  var pageData = canvas.toDataURL("image/jpeg", 1.0);
  var pdf = new jsPDF("", "pt", "a4");

  //有两个高度需要区分，一个是html页面的实际高度，和生成pdf的页面高度(841.89)
  //当内容未超过pdf一页显示的范围，无需分页
  if (leftHeight < pageHeight) {
    pdf.addImage(pageData, "JPEG", 0, 0, imgWidth, imgHeight);
  } else {
    while (leftHeight > 0) {
      pdf.addImage(pageData, "JPEG", 0, position, imgWidth, imgHeight);
      leftHeight -= pageHeight;
      position -= 841.89;
      //避免添加空白页
      if (leftHeight > 0) {
        pdf.addPage();
      }
    }
  }
  pdf.save("test.pdf");
});
```