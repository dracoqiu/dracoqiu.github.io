---
title: CSS3弹性布局
date: 2019-04-03 19:59:20
tags:
    - css
---
### CSS3弹性布局align-items和align-self垂直轴方向行对齐属性详解及实例
弹性项目可以按弹性容器当前行的`cross axis`来对齐，和`justify-content`类似，但在垂直方向上。其中`align-items`属性用于弹性容器，而`align-self`用于弹性项目。 `align-items` 为弹性容器中所有项目设置缺省对齐属性，包括匿名弹性项目。`align-self` 可以为单独的弹性项目设置对齐来覆盖缺省值。
**（对于匿名弹性项目，align-self总是匹配对应弹性容器的align-items的值。）**
<!-- more -->
如果弹性项的cross-axis外边距是auto，align-self属性没有效果
align-items和align-self的语法如下
> align-items: flex-start | flex-end | center | baseline | stretch
> align-self: auto | flex-start | flex-end | center| baseline |stretch

align-self属性取值auto时，将计算为其父元素的align-items值，或者为stretch如果没有父元素的话。对齐属性的取值定义如下：
弹性项的cross-start margin的边紧挨着该行的cross-start的边（一般来说，就是挨着弹性容器顶部）。
弹性项的cross-end margin的边紧挨着该行的cross-end的边（一般来说，就是挨着弹性容器底部）。
弹性项的margin box在该行的cross axis上居中（垂直居中对齐）。（如果该弹性行的cross size小于弹性项，将在两个方向上同时溢出。）
baseline
如果弹性项的行内轴（inline axis）和垂直轴（cross axis）一致，该值等同于flex-start。否则，它参与基线对齐（baseline alignment）：所有参与的弹性项按基线对齐，而基线和cross-start margin的边距离最大的项目被放在该行cross-start的边上（一般来说，就是基线和容器顶部距离最大的项目被定位在最上边）。
stretch
如果弹性项的垂直尺寸（cross size）属性（一般来说，就是高度）是auto，并且没有垂直外边距（cross-axis margin）是auto的，该弹性项被拉伸以适应容器高度， 其所使用的值是使项目的外边框（margin box）的垂直尺寸尽可能接近和行相同的尺寸，并依然遵循min-height/min-width/max-height/max-width属性的限制。 
注意：如果弹性容器的高度被限制，该值可能会导致该项的内容溢出。
弹性项的顶部挨着容器的顶部。
![1](/images/flex-align.svg)
上图包含5个例子，每个例子的弹性容器中包含4个不同颜色的弹性项目，最下面的弹性项目带有文本基线。