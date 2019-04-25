---
title: js-xlsx的使用
date: 2019-04-24 15:41:40
tags:
---

## 安装使用
```bash
npm install xlsx --save

import XLSX from "xlsx";
```
<!--more--> 

## 导出
```bash
let jsono = [
  {
    col1: "表头1",
    col2: "表头2",
    col3: "表头3"
  },
  {
    col1: "数据1",
    col2: "数据2",
    col3: "数据3"
  }，
];
downloadExl(jsono) {
  let json = jsono;
  let tmpDown; //导出的二进制对象
  let keyMap = []; //获取键
  for (let k in json[0]) {
    keyMap.push(k);
  }
  let tmpdata = []; //用来保存转换好的json
  json
    .map((v, i) =>
      keyMap.map((k, j) =>
        Object.assign(
          {},
          {
            //运用ES6内容
            v: v[k],
            position:
              (j > 25 ? this.getCharCol(j) : String.fromCharCode(65 + j)) +
              (i + 1)
          }
        )
      )
    )
    .reduce((prev, next) => prev.concat(next))
    .forEach(
      (v, i) =>
        (tmpdata[v.position] = {
          v: v.v
        })
    );
  let outputPos = Object.keys(tmpdata); //设置区域,比如表格从A1到D10
  let tmpWB = {
    SheetNames: ["sheet1"], //保存的表标题
    Sheets: {
      sheet1: Object.assign(
        {},
        tmpdata, //内容
        {
          "!ref": outputPos[0] + ":" + outputPos[outputPos.length - 1] //设置填充区域
        }
      )
    }
  };
  tmpDown = new Blob(
    [
      this.s2ab(
        XLSX.write(
          tmpWB,
          {
            bookType: "xlsx",
            bookSST: false,
            type: "binary"
          } //这里的数据是用来定义导出的格式类型
        )
      )
    ],
    {
      type: ""
    }
  ); //创建二进制对象写入转换好的字节流
  this.saveAs(
    tmpDown,
    "sheet1.xlsx"
  );
},
saveAs(obj, fileName) {
  //当然可以自定义简单的下载文件实现方式
  let tmpa = document.createElement("a");
  tmpa.download = fileName || "下载";
  tmpa.href = URL.createObjectURL(obj); //绑定a标签
  tmpa.click(); //模拟点击实现下载
  setTimeout(function() {
    //延时释放
    URL.revokeObjectURL(obj); //用URL.revokeObjectURL()来释放这个object URL
  }, 100);
},
s2ab(s) {
  //字符串转字符流
  let buf = new ArrayBuffer(s.length);
  let view = new Uint8Array(buf);
  for (let i = 0; i != s.length; ++i) {
    view[i] = s.charCodeAt(i) & 0xff;
  }
  return buf;
},
getCharCol(n) {
  let temCol = "",
    s = "",
    m = 0;
  while (n > 0) {
    m = (n % 26) + 1;
    s = String.fromCharCode(m + 64) + s;
    n = (n - m) / 26;
  }
  return s;
}
```
![](/images/jsxlsx1.png)

### 合并单元格
单元地址对象分别存储在{c:C, r:R}其中C并且R是0索引的列和行号。例如，单元格地址B5由对象表示{c:1, r:4}。

小区范围对象存储为{s:S, e:E}其中S是所述第一小区和 E在该范围内的最后一个单元。范围包括在内。例如，范围A3:B7由对象表示{s:{c:0, r:2}, e:{c:1, r:6}}。
```bash
let tmpWB = {
  SheetNames: ["sheet1"], //保存的表标题
  Sheets: {
    sheet1: Object.assign(
      {},
      tmpdata, //内容
      {
        //设置填充区域
        "!ref": outputPos[0] + ":" + outputPos[outputPos.length - 1], 
        //合并第一行，A1、B1、C1三个单元格
        "!merges": [{
            s: {
              c: 0,
              r: 0
            },
            e: {
              c: 2,
              r: 0
            }
          }
        ],
      }
    )
  }
};
```
![](/images/jsxlsx2.png)

### 表格样式
js-xlsx不支持直接修改样式，需要安装xlsx-style插件
```bash
npm install xlsx-style --save
```

```bash
let tmpWB = {
  SheetNames: ["sheet1"], //保存的表标题
  Sheets: {
    sheet1: Object.assign(
      {},
      tmpdata, // 内容
      {
        // 设置填充区域
        "!ref": outputPos[0] + ":" + outputPos[outputPos.length - 1], 
        // 合并第一行，A1、B1、C1三个单元格
        "!merges": [{
            s: {
              c: 0,
              r: 0
            },
            e: {
              c: 2,
              r: 0
            }
          }
        ],
        // 设置A1单元格的样式
        A1: {
          v: '表头1',
          s: {
            font: {
                sz: 13,
                bold: true,
                color: {
                    rgb: "FFFFAA00"
                }
            },
            alignment: {
                horizontal: "center",
                vertical: "center"
            }
          }
        },
      }
    )
  }
};
```
![](/images/jsxlsx3.png)

## 导入
```bash
<input type="file"onchange="importf(this)" />
<div id="demo"></div>
<script>
  /*
  FileReader共有4种读取方法：
  1.readAsArrayBuffer(file)：将文件读取为ArrayBuffer。
  2.readAsBinaryString(file)：将文件读取为二进制字符串
  3.readAsDataURL(file)：将文件读取为Data URL
  4.readAsText(file, [encoding])：将文件读取为文本，encoding缺省值为'UTF-8'
  */
  var wb;//读取完成的数据
  var rABS = false; //是否将文件读取为二进制字符串

  function importf(obj) {//导入
      if(!obj.files) {
          return;
      }
      var f = obj.files[0];
      var reader = new FileReader();
      reader.onload = function(e) {
          var data = e.target.result;
          if(rABS) {
              wb = XLSX.read(btoa(fixdata(data)), {//手动转化
                  type: 'base64'
              });
          } else {
              wb = XLSX.read(data, {
                  type: 'binary'
              });
          }
          //wb.SheetNames[0]是获取Sheets中第一个Sheet的名字
          //wb.Sheets[Sheet名]获取第一个Sheet的数据
          document.getElementById("demo").innerHTML= JSON.stringify( XLSX.utils.sheet_to_json(wb.Sheets[wb.SheetNames[0]]) );
      };
      if(rABS) {
          reader.readAsArrayBuffer(f);
      } else {
          reader.readAsBinaryString(f);
      }
  }

  function fixdata(data) { //文件流转BinaryString
      var o = "",
          l = 0,
          w = 10240;
      for(; l < data.byteLength / w; ++l) o += String.fromCharCode.apply(null, new Uint8Array(data.slice(l * w, l * w + w)));
      o += String.fromCharCode.apply(null, new Uint8Array(data.slice(l * w)));
      return o;
  }
</script>
```