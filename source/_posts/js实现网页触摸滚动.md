---
title: js实现网页触摸滚动
date: 2019-05-09 18:21:55
tags:
---

```bash
import $ from "jquery";

methods: {
  down: function(evt) {
    let target = evt.srcElement;
    let scrollWidth = 0;
    let scrollHeight = 0;
    while (target.parentNode && scrollHeight === 0 && scrollWidth === 0) {
      target = target.parentNode;
      scrollWidth = target.scrollWidth - target.clientWidth;
      scrollHeight = target.scrollHeight - target.clientHeight;
    }
    
    // 可以判断元素是否存在某个类名来决定是否触发滚动
    if ($(target).attr('class') && $(target).attr('class').indexOf('draging') !== -1) {
      return
    }
    if (scrollHeight) {
      let x = evt.x;
      let y = evt.y;
      this.scrollInfo = { target, x, y };
    } else {
      this.scrollInfo = null;
    }
  },
  up: function() {
    this.scrollInfo = null;
  },
  move: function(evt) {
    if (this.scrollInfo) {
      let deltaX = this.scrollInfo.x - evt.x;
      let deltaY = this.scrollInfo.y - evt.y;
      this.scrollInfo.target.scrollLeft += deltaX * 0.1;
      this.scrollInfo.target.scrollTop += deltaY * 0.1;
    }
  }
},
mounted() {
  document.addEventListener("mousedown", this.down, false);
  document.addEventListener("mouseup", this.up, false);
  document.addEventListener("mousemove", this.move, false);

  //去除监听事件
  // 这边methods里面的方法要写成实名函数，因为removeEventListener只有对实名函数才能生效
  document.addEventListener("mousedown", this.down, false);
  document.addEventListener("mouseup", this.up, false);
  document.addEventListener("mousemove", this.move, false);
}
```