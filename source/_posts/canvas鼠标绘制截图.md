---
title: canvas鼠标绘制截图
date: 2019-06-11 18:13:38
tags:
---

```bash
<canvas id="canvasInAPerfectWorld" width="490" height="220" style="border: 1px solid blue">
    <span>该浏览器不支持canvas内容</span> 
</canvas>
<button onclick="screenshot()">截图</button>
<img id="imgPhotot" alt="">

<script>
    var context = document.getElementById('canvasInAPerfectWorld').getContext("2d");
    var canvas = document.getElementById('canvasInAPerfectWorld')
    var clickX = new Array();
    var clickY = new Array();
    var clickDrag = new Array();
    var paint;

    function screenshot() {
      let showPhoto = document.getElementById("canvasInAPerfectWorld").toDataURL("image/png");
      document.getElementById("imgPhotot").src = showPhoto
    }
 
    // 记录鼠标坐标点
    function addClick(x, y, dragging) {
        clickX.push(x);
        clickY.push(y);
        clickDrag.push(dragging);
    }
 
    // 绘制
    function redraw() {
        canvas.width = canvas.width; // Clears the canvas
 
        context.strokeStyle = "#df4b26";
        context.lineJoin = "round";
        context.lineWidth = 5;
 
        for (var i = 0; i < clickX.length; i++) {
            context.beginPath();
            if (clickDrag[i] && i) { //当是拖动而且i!=0时，从上一个点开始画线。
                context.moveTo(clickX[i - 1], clickY[i - 1]);
            } else {
                context.moveTo(clickX[i] - 1, clickY[i]);
            }
            context.lineTo(clickX[i], clickY[i]);
            context.closePath();
            context.stroke();
        }
    }
 
    // 鼠标按下事件
    $('#canvasInAPerfectWorld').mousedown(function (e) {
        var mouseX = e.pageX - this.offsetLeft;
        var mouseY = e.pageY - this.offsetTop;
 
        paint = true;
        // addClick(e.pageX - this.offsetLeft, e.pageY - this.offsetTop);
        addClick(mouseX, mouseY)
        redraw();
    });
    // 鼠标移动事件
    $('#canvasInAPerfectWorld').mousemove(function (e) {
        if (paint) { //是不是按下了鼠标
            addClick(e.pageX - this.offsetLeft, e.pageY - this.offsetTop, true);
            redraw();
        }
    });
    // 鼠标松开事件
    $('#canvasInAPerfectWorld').mouseup(function (e) {
        paint = false;
    });
    // 鼠标移开事件
    $('#canvasInAPerfectWorld').mouseleave(function (e) {
        paint = false;
    });
</script>
```