---
title: 如何用CSS3实现瀑布流效果
date: 2019-04-03 20:17:30
tags:
    - css
---
CSS3功能挺强大，觉得几行代码就可以搞定一个效果，这几天学了一个瀑布流效果，还挺喜欢的。
<!-- more -->
效果图
![1](/images/1693725-e3ffa61922c6e632.jpg)
### 一、原理图
![2](/images/1693725-a893a57df112b872.png)
在一个大盒子里，放置多个小盒子，小盒子的大小可以不一致，长短不一样，呈现一种瀑布流的效果。
### 二、body部分代码
```html
<body>
     <div id="con">//建立的大盒子
            //下面是内容区，就放一个盒子，其他的跟它是一样的
           <div class="pic">
                  <img src="images/1.jpg" width="188px" />   //插入图片
                  <h3><a href="#">野蛮生长</a></h3>         //下面的标题
                 <p>人长大的标志：试着听从自己内心的声音，而不去在乎外面的声
                  音，等待和拖延是世界上最容易压垮一个人得东西。犹豫不决是你
                  最大的敌人。能看书就不要发呆，能碎觉就不要拖延，能吃饭就不
                  要饿着，能亲吻就不要说话，能找到自己想做的事情就不容易了，
                  青春得浪费在美好事物上。
                </p>   //文字内容
          </div>     //这个内容盒子可以多复制一些，只要计算好大盒子的宽度和小盒子的数量就好了
    </div>
</body>
```
### 三、CSS3代码
```css
*{padding:0;margin:0}
/*给大盒子添加样式*/
#con{width:980px;margin:60px auto;border-radius:25px;box-shadow:5px 5px 10px #000;padding:20px;

/*下面代码是兼容各个浏览器的，并实现了四列，没两列之间间距为30px，*/
-moz-column-count:4;-moz-column-gap:30px;-moz-column-rule:0px solid #ff0000;   //火狐浏览器
-webkit-column-count:4;-webkit-column-gap:30px;-webkit-column-rule:0px solid #ff0000;   //Google chrome
-o-column-count:4;-o-column-gap:30px;-o-column-rule:0px solid #ff0000;   //Opera浏览器的
}

/*小盒子内容区的样式，display:inline-block：实现 效果*/
#con .pic{width:188px; min-height:100px;box-shadow:2px 2px 6px #b5b5b5;padding:20px 15px;margin:10px;display:inline-block}

#con h3{border-bottom:1px solid #ddd;line-height:30px;text-align:center;padding:5px 5px;}
#con h3 a{text-decoration:none;color:#999;}
#con  p{font-size:12px;color:#666;line-height:20px;white-space:nowrap;overflow:hidden;
/*很多文字，一行显示不下，用省略号显示的代码*/
text-overflow:ellipsis;-o-text-overflow:ellipsis;
-moz-text-overflow: ellipsis; -webkit-text-overflow: ellipsis;  }
```
话说，CSS3真心强大，以前看到瀑布流样式的图片，从来没想过会这么简单，几行代码就能轻松搞定。