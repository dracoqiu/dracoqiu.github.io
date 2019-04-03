---
title: CSS滚动条实现步骤及美化小技巧
date: 2019-04-03 20:23:08
tags:
    - css
---
很多朋友在网页设计中要自定义滚动条样式的情景，滚动条的样式我们可以通过css来控制的，比如网易邮箱的滚动条样子很好看，就是利用的CSS来设置实现的。但是css控制的滚动条应该如何实现和隐藏呢？滚动条能不能换颜色或者做的更好看一些呢？下面通通告诉你。

你可以复习一下CSS overflow 属性的内容。
<!-- more -->
1、overflow-y : 设置当对象的内容超过其指定高度时如何管理内容；overflow-x : 设置当对象的内容超过其指定宽度时如何管理内容。

参数:
visible：扩大面积以显示所有内容

auto：仅当内容超出限定值时添加滚动条

hidden：总是隐藏滚动条

scroll：总是显示滚动条

2、height : 设置滚动条的高度（修改其后数值即可）。

3、滚动条颜色参数设置：

scrollbar-3d-light-color 设置或检索滚动条亮边框颜色

scrollbar-highlight-color 设置或检索滚动条3D界面的亮边（ThreedHighlight）颜色

scrollbar-face-color  设置或检索滚动条3D表面（ThreedFace）的颜色

scrollbar-arrow-color  设置或检索滚动条方向箭头的颜色

scrollbar-shadow-color  设置或检索滚动条3D界面的暗边（ThreedShadow）颜色

scrollbar-dark-shadow-color 设置或检索滚动条暗边框（ThreedDarkShadow）颜色

scrollbar-base-color  设置或检索滚动条基准颜色


设置滚动条样式
在原来的html的时候，我们可以这样定义整个页面的滚动条
body{
scrollbar-3dlight-color:#D4D0C8; /*- 最外左 -*/
   scrollbar-highlight-color:#fff; /*- 左二 -*/
   scrollbar-face-color:#E4E4E4; /*- 面子 -*/
   scrollbar-arrow-color:#666; /*- 箭头 -*/
   scrollbar-shadow-color:#808080; /*- 右二 -*/
   scrollbar-darkshadow-color:#D7DCE0; /*- 右一 -*/
   scrollbar-base-color:#D7DCE0; /*- 基色 -*/
   scrollbar-track-color:#;/*- 滑道 -*/
}

但是同样的代码，我们应用在 xhtml下就不起作用了，我相信好多好朋友也遇到过同样的问题
那么怎么才能在xhtml下应用滚动条样式呢？看下列代码
html{
scrollbar-3dlight-color:#D4D0C8; /*- 最外左 -*/
   scrollbar-highlight-color:#fff; /*- 左二 -*/
   scrollbar-face-color:#E4E4E4; /*- 面子 -*/
   scrollbar-arrow-color:#666; /*- 箭头 -*/
   scrollbar-shadow-color:#808080; /*- 右二 -*/
   scrollbar-darkshadow-color:#D7DCE0; /*- 右一 -*/
   scrollbar-base-color:#D7DCE0; /*- 基色 -*/
   scrollbar-track-color:#;/*- 滑道 -*/
}

这段代码和上一段唯一的不同就是在css定义的元素上，一个是body一个是html。我们再测试一下，把html页面的"body"修改成"html"测试一下，发现依然可以实现效果。那到底是为什么呢？

从字面上来看，xhtml比html多一个x,那么这个x其实也就是xml,为什么要加一个xml在里面？其实最根本的原因就是要让html更加结构化标准化（因为html实在是太烂）。我们在html里面定义的是body，因为html不是很标准所以这样可以生效，而在xhtml里面这样就不行了，我看看那个图很明显，body标签本身不是根元素，只有html才是根元素，而页面的滚动条也是属于根元素的，所以这就是我们为什么定义body没有效果的原因，因为我们定义的只是一个子原素。ok，我们知道了原理，来做一个试验如果把定义"body"或"xhtml"换成"*"，
*{
scrollbar-3dlight-color:#D4D0C8;
   scrollbar-highlight-color:#fff;
   scrollbar-face-color:#E4E4E4;
   scrollbar-arrow-color:#666;
   scrollbar-shadow-color:#808080;
   scrollbar-darkshadow-color:#D7DCE0;
   scrollbar-base-color:#D7DCE0;
   scrollbar-track-color:#;
}

在html和xhtml都通过，因为*就是定义页面上的任何标签当然也包括了“html”这个标签。

(ps:其实与其说是html与xhtml的区别到不如说是有无XHTML 1.0 transitional doctype的区别，但是如果你把页面的XHTML 1.0 transitional doctype去掉的话，那么这个页面就没有doctype，默认的显示方式就是html4.01,不过你要把XHTML 1.0 transitional doctype修改成HTML 4.01 doctype同样页面定义body也不会有效果的，虽然这个页面的标准是html 4.01)

css隐藏滚动条(横向,坚向)
网上都说使用overflow-y:hiddencss可以隐藏滚动条，但是只能针对div元素，并不能隐藏浏览器，而一些资料说 <boby>里加入scroll="no"，可隐藏滚动条；在<boby>里加入style="overflow-x:hidden"，可隐藏水平滚动条；加入style="overflow-y:hidden"，可隐藏垂直滚动条。

1、完全隐藏

　　在里加入scroll="no"，可隐藏滚动条;

2、在不需要时隐藏

　　指当浏览器窗口宽度或高度大于页面的宽或高时，不显示滚动条;反之，则显示;

3、样式表方法

　　在里加入style="overflow-x:hidden"，可隐藏水平滚动条;

　　加入style="overflow-y:hidden"，可隐藏垂直滚动条。

body{ overflow-x:hidden; } 在标准 DTD 下是不可以的

html { overflow: scroll; }

强制隐藏滚动条:

html { overflow: hidden; }

隐藏IE的水平滚动条:

html { overflow-x: hidden; }

隐藏IE的垂直滚动条:

html { overflow-y: hidden; }

强制显示IE的水平滚动条:

html { overflow-x: scroll; }

强制显示IE的垂直滚动条:

html { overflow-y: scroll; }

强制显示Mozilla的水平滚动条:

html { overflow:-moz-scrollbars-horizontal; }

注意: 仅仅强制显示水平滚动条. 也就是说, 即使需要显示垂直滚动条时, 垂直滚动条也不会出现.

强制显示Mozilla的垂直滚动条:

html { overflow:-moz-scrollbars-vertical; }

注意: 仅仅强制显示垂直滚动条. 也就是说, 即使需要显示水平滚动条时, 水平滚动条也不会出现.

**最终的解决办法：**
在页面添加:
```cs
<style>
html { overflow-x:hidden; //隐藏水平滚动条overflow-y:hidden;//隐藏垂直滚动条}
</style>
```
CSS怎么美化滚动条
各种浏览器对CSS滚动条的支持情况：

这里说的Webkit浏览器包括谷歌浏览器，苹果公司的Safari浏览器，以及最新的Opera浏览器。这些浏览器加起来占有超过半数的桌面浏览器市场份额。对于移动端浏览器，基本上是谷歌浏览器和Safari浏览器的天下。唯一的遗憾是火狐浏览器，至今没有对CSS滚动条属性做任何的改进。至于IE浏览器，我们期待吧。

鉴于目前浏览器市场的格局，我们很有信心相信CSS滚动条美化功能会有很好的很光明的前景。

很多年前谷歌浏览器就已经开始支持对滚动条的CSS美化。这些Webkit浏览器专属的CSS属性需要使用-webkit-浏览器引擎前缀，我们在这里将只会使用一些基本的CSS滚动条属性，在代码里会增加一些必要的解释说明。
```cs
::-webkit-scrollbar {
    width: 15px;
} /* 这是针对缺省样式 (必须的) */
```
当CSS中出现伪元素样式时，Webkit引擎将会关闭它的缺省滚动条样式输出，只使用CSS里提供的样式信息。

这里是其它一些伪元素样式：
```
::-webkit-scrollbar-track {
      background-color: #b46868;
} /* 滚动条的滑轨背景颜色 */

::-webkit-scrollbar-thumb {
      background-color: rgba(0, 0, 0, 0.2);
} /* 滑块颜色 */

::-webkit-scrollbar-button {
      background-color: #7c2929;
} /* 滑轨两头的监听按钮颜色 */

::-webkit-scrollbar-corner {
      background-color: black;
} /* 横向滚动条和纵向滚动条相交处尖角的颜色 */
```
加上了这些CSS属性，你将会看到下面的效果(再次提醒：你需要使用Webkit浏览器，比如谷歌浏览器才能看到效果)。
![1](/images/201611151617488929.png)
谷歌浏览器的用户是最幸福的。但我们也不能放弃火狐浏览器和IE浏览器用户。对于这些浏览器，有一个非常有效的补救方案，就是使用javascript插件。网上有不少人推荐一个由Kelvin Luck开发的一个jQuery插件：jScrollPane。但也有人评论这个插件是“PITA”，我翻了一下字典，发现“PITA”中文意思是“让人蛋疼”。经过试用，感到它的确是让人蛋疼。不推荐使用它。我发现了另外一个插件malihu-custom-scrollbar-plugin，感觉相当不错，它的用法是：
```
<link rel="stylesheet" href="js/malihu-custom-scrollbar-plugin/jquery.mCustomScrollbar.min.css">
<!-- latest jQuery direct from google's CDN -->
<script type="text/javascript" src="js/jquery-1.11.1.min.js"></script>
<script type="text/javascript" src="js/jquery-migrate-1.2.1.min.js"></script>

<script src="js/malihu-custom-scrollbar-plugin/jquery.mCustomScrollbar.concat.min.js"></script>

<script>
if (!$.browser.webkit) {

    $.mCustomScrollbar.defaults.scrollButtons.enable=true; //enable scrolling buttons by default
    $.mCustomScrollbar.defaults.axis="yx"; //enable 2 axis scrollbars by default

    $(".container").mCustomScrollbar({theme:"dark"});
}
</script>
```
已经有很多网站都使用了这些滚动条的CSS美化技巧，特别是谷歌的一些应用和网站上，比如Gmail和Google+。相信很快火狐浏览器和IE浏览器也会提供自己的解决方案。
用CSS调整滚动条配色
IE浏览器可以通过调整CSS的方式，来给滚动条换色。

代码如下：
```
.uicss-cn
{
height:580px;overflow-y: scroll;
scrollbar-face-color:#EAEAEA;
scrollbar-shadow-color:#EAEAEA;
scrollbar-highlight-color:#EAEAEA;
scrollbar-3dlight-color:#EAEAEA;
scrollbar-darkshadow-color:#697074;
scrollbar-track-color:#F7F7F7;
scrollbar-arrow-color:#666666;
}
```
具体样式对应的滚动条区域如图所示：
![1](/images/201611151625519680.png)