---
title: CSS3滚动条美化CSS3滚动条皮肤
date: 2019-04-03 20:30:37
tags:
    - css
---
CSS3 -webkit-scrollbar滚动条皮肤美化实现，利用-webkit-scrollbar，-webkit-scrollbar-track，-webkit-scrollbar-thumb这2个属性设置不同样式的滚动条。
<!-- more -->
下面是5个滚动条样式。
css代码
```css
.test1::-webkit-scrollbar {
 width: 8px;
}
 .test1::-webkit-scrollbar-track {
 background-color:#808080;
 -webkit-border-radius: 2em;
 -moz-border-radius: 2em;
 border-radius:2em;
}
 .test1::-webkit-scrollbar-thumb {
 background-color:#ff4400;
 -webkit-border-radius: 2em;
 -moz-border-radius: 2em;
 border-radius:2em;
}
.test2::-webkit-scrollbar {
 width: 8px;
}
 .test2::-webkit-scrollbar-track {
 background-color:#fff;
 -webkit-border-radius: 2em;
 -moz-border-radius: 2em;
 border-radius:2em;
 border:1px solid #ccc;
}
 .test2::-webkit-scrollbar-thumb {
 background-color: #F90;
 background-image: -webkit-linear-gradient(45deg,  rgba(255, 255, 255, .4) 25%,  transparent 25%,  transparent 50%,  rgba(255, 255, 255, .4) 50%,  rgba(255, 255, 255, .4) 75%,  transparent 75%,  transparent);
 -webkit-border-radius: 2em;
 -moz-border-radius: 2em;
 border-radius:2em;
}
.test3::-webkit-scrollbar {
 width: 12px;
}
 .test3::-webkit-scrollbar-track {
 background-color:#f5f5f5;
 -webkit-border-radius: 2em;
 -moz-border-radius: 2em;
 border-radius:2em;
}
 .test3::-webkit-scrollbar-thumb {
 border-radius: 10px;
 background-color: #FFF;
 background-image: -webkit-gradient(linear,  40% 0%,  75% 84%,  from(#4D9C41),  to(#19911D),  color-stop(.6, #54DE5D));
 -webkit-border-radius: 2em;
 -moz-border-radius: 2em;
 border-radius:2em;
}
.test4{
    width:500px;
    overflow:scroll !important;
    width:600px;
}
.test4>div{
    width:1000px;
}
.test4::-webkit-scrollbar {
 width: 12px;
 height:12px;
}
 .test4::-webkit-scrollbar-track {
 background-color:#f5f5f5;
 -webkit-border-radius: 2em;
 -moz-border-radius: 2em;
 border-radius:2em;
}
 .test4::-webkit-scrollbar-thumb {
 border-radius: 10px;
 background-color: #F90;
  background-image: -webkit-linear-gradient(90deg,  rgba(255, 255, 255, .4) 25%,  transparent 25%,  transparent 50%,  rgba(255, 255, 255, .4) 50%,  rgba(255, 255, 255, .4) 75%,  transparent 75%,  transparent);
 -webkit-border-radius: 2em;
 -moz-border-radius: 2em;
 border-radius:2em;
}
.test5::-webkit-scrollbar {
 width: 12px;
 height:12px;
}
 .test5::-webkit-scrollbar-track {
 background-color:#f5f5f5;

}
 .test5::-webkit-scrollbar-thumb {
 background-color: #d52828;
}
```
转自：https://code.pengyaou.com/legendsz/front/codecss/MTA1.html

