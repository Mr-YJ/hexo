---
title: interactjs的
date: 2019-05-08 17:47:06
tags:
---

interactjs用于现代浏览器的JavaScript拖放、调整大小和多点触摸手势的js库

## 安装使用
```bash
$ npm install interactjs@next --save

import interact from 'interactjs'

// class为draggable的元素添加拖动操作
interact('.draggable')   
  .draggable({
    inertia: true,

    // 将元素保存在其父元素的区域内
    modifiers: [
      interact.modifiers.restrict({
        restriction: "parent",  //也可以用元素写法 restriction: $(".list")[0],
        endOnly: true,
        elementRect: { top: 0, left: 0, bottom: 1, right: 1 }
      }),
    ],

    autoScroll: true,

    // 在每个dragmove事件上调用这个函数
    onmove: dragMoveListener,

    // 在每个dragend事件上调用这个函数
    onend: function (event) {
      // 判断拖动元素是否进入放入区域内，如果没有就重置位置
      if (event.target.getAttribute("class").indexOf("can-drop") == -1) {
        this.reset(event);
      }
    }
  });

// 拖动实现
dragMoveListener(event) {
  var target = event.target,
    x = (parseFloat(target.getAttribute("data-x")) || 0) + event.dx,
    y = (parseFloat(target.getAttribute("data-y")) || 0) + event.dy;

  target.style.zIndex = "9999";

  target.style.webkitTransform = target.style.transform =
    "translate(" + x + "px, " + y + "px)";

  target.setAttribute("data-x", x);
  target.setAttribute("data-y", y);
},
```

<!--more-->

## 拖放
```bash
// 拖放实现，判断拖动元素draggable和draggable-test是否进入container这个元素区域内
interact(".container").dropzone({
  accept: ".draggable, .draggable-test",

  // 判断什么情况下元素算在放入区域内，有三种参数
  * 'pointer' – 鼠标必须位于放入区域之上(默认)
  * 'center' – 拖动元素中心必须位于放入区域之上
  * 0-1之间的一个数字，即(交点面积)/(可拖动面积)。0.5表示当可拖动元素的一半面积超过放入区域时
  overlap: 0.75,

  ondragenter: function(event) {
    // 元素进入时  relatedTarget指的是拖动元素，target指的放入的区域元素
    // 元素可进入，设置类名
    event.relatedTarget.classList.add("can-drop"); 
  },
  ondragleave: function(event) {
    //元素离开自己的区域
    // 元素离开，去掉可放入的类名
    event.relatedTarget.classList.remove("can-drop");
  },
  ondrop: event => {
    // 松开放入时
  },
  ondropactivate: function (event) {
    // 松开放入前
    event.target.classList.add('drop-active')
  },
  ondropdeactivate: function (event) {
    // 松开放入后
    event.target.classList.remove('drop-active')
  }
});

// 重置元素位置
reset(event) {
  var target = event.target,
    x = target.getAttribute("data-x"),
    y = target.getAttribute("data-y");

  target.style.zIndex = "";

  target.style.webkitTransform = target.style.transform =
    "translate(" + 0 + "px, " + 0 + "px)";

  target.setAttribute("data-x", 0);
  target.setAttribute("data-y", 0);
},

```

## snapping
```bash
var element = document.getElementById('grid-snap'),
    x = 0, y = 0;

interact(element)
  .draggable({
    modifiers: [
      interact.modifiers.snap({
        targets: [
          interact.createSnapGrid({ x: 30, y: 30 })
        ],
        range: Infinity,
        relativePoints: [ { x: 0, y: 0 } ]
      }),
      interact.modifiers.restrict({
        restriction: element.parentNode,
        elementRect: { top: 0, left: 0, bottom: 1, right: 1 },
        endOnly: true
      })
    ],
    inertia: true,
  })
  .on('dragmove', function (event) {
    x += event.dx;
    y += event.dy;

    event.target.style.webkitTransform =
    event.target.style.transform =
        'translate(' + x + 'px, ' + y + 'px)';
  });
```

## 改变大小
```bash
interact('.resize-drag')
  .draggable({
    onmove: window.dragMoveListener,
    modifiers: [
      interact.modifiers.restrict({
        restriction: 'parent',
        elementRect: { top: 0, left: 0, bottom: 1, right: 1 }
      })
    ]
  })
  .resizable({
    // resize from all edges and corners
    edges: { left: true, right: true, bottom: true, top: true },

    modifiers: [
      // keep the edges inside the parent
      interact.modifiers.restrictEdges({
        outer: 'parent',
        endOnly: true,
      }),

      // minimum size
      interact.modifiers.restrictSize({
        min: { width: 100, height: 50 },
      }),
    ],

    inertia: true
  })
  .on('resizemove', function (event) {
    var target = event.target,
        x = (parseFloat(target.getAttribute('data-x')) || 0),
        y = (parseFloat(target.getAttribute('data-y')) || 0);

    // update the element style
    target.style.width  = event.rect.width + 'px';
    target.style.height = event.rect.height + 'px';

    // translate when resizing from top or left edges
    x += event.deltaRect.left;
    y += event.deltaRect.top;

    target.style.webkitTransform = target.style.transform =
        'translate(' + x + 'px,' + y + 'px)';

    target.setAttribute('data-x', x);
    target.setAttribute('data-y', y);
    target.textContent = Math.round(event.rect.width) + '\u00D7' + Math.round(event.rect.height);
  });
```

## Multi-touch Rotation多点触控旋转(只限触屏)
```bash
var angle = 0;

interact('#rotate-area').gesturable({
  onmove: function (event) {
    var arrow = document.getElementById('arrow');

    angle += event.da;

    arrow.style.webkitTransform =
    arrow.style.transform =
      'rotate(' + angle + 'deg)';

    document.getElementById('angle-info').textContent =
      angle.toFixed(2) + '\u00b0';
  }
});
```

## Pinch-to-zoom捏(触摸屏)
```bash
var angleScale = {
  angle: 0,
  scale: 1,
}
var gestureArea = document.getElementById('gesture-area')
var scaleElement = document.getElementById('scale-element')
var resetTimeout

interact(gestureArea)
  .gesturable({
    onstart: function (event) {
      angleScale.angle -= event.angle

      clearTimeout(resetTimeout)
      scaleElement.classList.remove('reset')
    },
    onmove: function (event) {
      // document.body.appendChild(new Text(event.scale))
      var currentAngle = event.angle + angleScale.angle
      var currentScale = event.scale * angleScale.scale

      scaleElement.style.webkitTransform =
      scaleElement.style.transform =
        'rotate(' + currentAngle + 'deg)' + 'scale(' + currentScale + ')'

      // uses the dragMoveListener from the draggable demo above
      dragMoveListener(event)
    },
    onend: function (event) {
      angleScale.angle = angleScale.angle + event.angle
      angleScale.scale = angleScale.scale * event.scale

      resetTimeout = setTimeout(reset, 1000)
      scaleElement.classList.add('reset')
    }
  })
  .draggable({ onmove: dragMoveListener })

function reset () {
  scaleElement.style.webkitTransform =
    scaleElement.style.transform =
    'scale(1)'

  angleScale.angle = 0
  angleScale.scale = 1
}
```

## Tap, doubletap and hold
```bash
interact('.tap-target')
  .on('tap', function (event) {
    event.currentTarget.classList.toggle('switch-bg');
    event.preventDefault();
  })
  .on('doubletap', function (event) {
    event.currentTarget.classList.toggle('large');
    event.currentTarget.classList.remove('rotate');
    event.preventDefault();
  })
  .on('hold', function (event) {
    event.currentTarget.classList.toggle('rotate');
    event.currentTarget.classList.remove('large');
  });
```