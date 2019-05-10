---
title: vue-virtual-keyboard vue虚拟键盘
date: 2019-05-11 11:26:54
tags:
---

## 安装使用
```bash
npm install --save vue-virtual-keyboard

import $ from "jquery";
import Keyboard from "vue-virtual-keyboard/src/Keyboard";

// 作为组件使用
<Keyboard :options="keyboardOption" class="user-name" v-model="userName"></Keyboard>

components: {
  Keyboard
},
data() {
  return {
    userName: "",
    // 详细参数可参考https://github.com/Mottie/Keyboard/wiki/Options
     keyboardOption: {
        usePreview: false,
        stickyShift: false,
        autoAccept: true,
        language: "zh",
        display: {
          // 覆盖显示文字
          accept: "确认",
          bksp: "删除",
          cancel: "取消",
          enter: "回车"
        }
      },
  }
}
// 获取数据的时候可以直接通过userName获取输入值
// 清楚数据的时候除了设置userName=""外还需手动清楚界面上的输入框value值
// $(".user-name input").val("");

```

<!--more-->

![](/images/vuekeyboard.jpg)
