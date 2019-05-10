---
title: flex布局
date: 2019-05-10 10:48:41
tags:
---

采用 Flex布局的元素，display: flex; 称为Flex容器（flex container），它的所有子元素自动成为容器成员  
容器默认存在两根轴：水平的主轴和垂直的交叉轴。

## flex-direction
* row（默认值）：主轴为水平方向，起点在左端。
![](/images/flexrow.png)
* row-reverse：主轴为水平方向，起点在右端。
![](/images/flexrowreverse.png)
* column：主轴为垂直方向，起点在上沿。
![](/images/flexcolumn.png)
* column-reverse：主轴为垂直方向，起点在下沿
![](/images/flexcolumnreverse.png)

<!--more-->

## flex-wrap
* nowrap(默认)：不换行
* wrap：换行，往下换行
* wrap-reverse：换行，往上换行

## justify-content
项目在主轴上的对齐方式
* flex-start（默认）：左对齐
* flex-end：右对齐
* center： 居中
* space-between：两端对齐，项目之间的间隔相等
* space-around：每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍类似于margin: 0px 20px;
![](/images/spacearound.png)

## align-items
* flex-start：交叉轴的起点对齐。
* flex-end：交叉轴的终点对齐。
* center：交叉轴的中点对齐。
* baseline: 项目的第一行文字的基线对齐。
* stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。
![](/images/alignitem.png)

## align-content
定义多根轴线的对齐方式,如果项目只有一根轴线，该属性不起作用。
* flex-start：与交叉轴的起点对齐。
* flex-end：与交叉轴的终点对齐。
* center：与交叉轴的中点对齐。
* space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。
* space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。
* stretch（默认值）：轴线占满整个交叉轴。
![](/images/aligncontent.png)