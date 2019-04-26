---
layout: post
title:  "元素位置操作"
date:   2017-10-10 08:16:54
categories:
- JavaScript
tags:
- JavaScript
- HTML
---




scroll,offset的使用






# offset

元素的可见大小由其高度、宽度决定,包括所有内边距、滚动条和边框大小(注意,不包括外边距)。通过
下列 5 个属性可以取得元素的偏移量:

1. `offsetHeight`: 获得元素高度　＝　height + border + padding
2. `offsetWidth` : 获得元素宽度　 ＝ widht + border + padding
3. `offsetLeft`: 获得元素左外边框距离包含该元素的左内边框的距离(经过定位)
4. `offsetTop`: 获得元素上外边框距离包含该元素的上内边框的距离(经过定位)
5. `offsetParent`: 获得包含该元素的元素(定位)

![](http://oujvmc3la.bkt.clouddn.com/offset.png)

```
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <title>offset相关的</title>
  <style>
    * {
      margin: 0;
      padding: 0;
    }

    .f {
      width: 600px;
      height: 300px;
      margin: 50px auto;
      border: 10px solid green;
      padding: 10px;
      position: relative;
    }

    #box {
      width: 200px;
      height: 200px;
      padding: 20px;
      border: 10px solid red;
      position: absolute;
      top: 0;
      left: 0;

    }
  </style>
</head>

<body>

  <div class="f">
    <div id="box"></div>
  </div>

  <script src="./jquery.min.js" charset="utf-8"></script>
  <script type="text/javascript">

    console.log("/***********jQuery******************/");
    console.log($(window).width());
    console.log($(document).width());

    //获取 内容 width
    console.log($("#box").width()); //200

    //获取  width + padding
    console.log($("#box").innerWidth()); //240

    //获取  width + padding + border
    console.log($("#box").outerWidth()); //260


    console.log("/**********原生js*************/");
    var box = document.getElementById("box");

    //获取  width + padding + border
    console.log(box.offsetWidth); //260, 只读属性

    //获取  width + padding
    console.log(box.clientWidth); // 240, 只读属性


    //获取  width
    var styles = window.getComputedStyle(box);
    console.log(parseInt(styles.width));
    console.log(styles.border);



    console.log("/*************位置有关系***********************/");
    //一个元素和最近的设置过定位(不是static)的祖先元素之间的距离
    // offsetLeft, offsetTop 只读
    console.log("box.offsetLeft = ", box.offsetLeft);
    console.log("box.offsetTop = ", box.offsetTop);

    box.style.left = "100px";//设置完毕之后， offsetLeft也就变了
    box.style.top = "100px";//设置完毕之后， offsetTop变了


    console.log("box.offsetParent = ", box.offsetParent);

  </script>
</body>

</html>
```

## 使用offset实现动画

元素偏移量　　＝　　当前偏移量　＋　移动步长　

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>offset动画</title>
    <style>
    *{
      padding: 0;
      margin: 0;
    }
      .box{
        width: 100px;
        height: 100px;
        border-radius: 50%;
        background-color: gold;
        margin-top: 200px;
        position: absolute;
        left:0;
        top: 0;
      }
    </style>
  </head>
  <body>
    <div class="box">

    </div>
    <script>
      var box = document.getElementsByClassName('box')[0];
      var timer = setInterval(function(){
        box.style.left = box.offsetLeft + 1 +'px';
        if(box.offsetLeft>1200){
          clearInterval(timer);
        }
      },20);
    </script>

  </body>
</html>
```

# scroll

scroll是滚动用法，即通过拖动条改变页面位置，scroll相关属性：　
　　　　
1. `scrollHeight` :在没有滚动条的情况下,元素内容的总高度。
2. `scrollWidth` :在没有滚动条的情况下,元素内容的总宽度。
3. `scrollLeft` :被隐藏在内容区域左侧的像素数。通过设置这个属性可以改变元素的滚动位置。
4. `scrollTop` :被隐藏在内容区域上方的像素数。通过设置这个属性可以改变元素的滚动位置。

![](http://oujvmc3la.bkt.clouddn.com/scroll.png)

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>window.scrollTo 控制浏览器窗口的滚动条</title>
    <style media="screen">
      * {
        margin: 0;
        padding: 0;
      }

      body {
        width: 3000px;
        height: 3000px;
      }

      .go-top {
        width: 100px;
        height: 100px;
        background: #000;
        color: #fff;
        border-radius:  50%;
        text-align: center;
        line-height: 100px;

        position: fixed;
        right: 100px;
        bottom: 100px;
        cursor: pointer;
        display: none;
      }
    </style>
  </head>
  <body>

    <div id="go-top" class="go-top">顶部</div>

    <script src="./jquery.min.js" charset="utf-8"></script>
    <script src="./scroll.js" charset="utf-8"></script>
    <script type="text/javascript">

    var goTop = document.getElementById("go-top");

    //jQuery中的 scroll
    /* $(window).scroll(function(){
      var temp = getScrollValue();
      console.log("垂直滚动了: ", temp.top);
      // console.log("水平滚动了: ", temp.left);

      if (temp.top >= 666) {
        $(goTop).show();
      } else {
        $(goTop).hide();
      }
    });
    */

    //原生的scroll事件
    window.onscroll = function () {
      var temp = getScrollValue();
      console.log("垂直滚动了: ", temp.top);
      // console.log("水平滚动了: ", temp.left);

      if (temp.top >= 666) {
        $(goTop).show();
      } else {
        $(goTop).hide();
      }
    };


    var timer;
    goTop.onclick = function () {

      clearInterval(timer);
      //让垂直滚动条缓慢滚动到顶部

      timer = setInterval(function () {
        var current = document.body.scrollTop;
        current -= 100;

        if (current <= 10) {
          window.scrollTo(0, 0);
          clearInterval(timer);
        }
        window.scrollTo(0, current);
      }, 20);

    };
    </script>


  </body>
</html>

```

JAVAScript:

```
/*
功能：实现一个比较通用的获得浏览器窗口的垂直，水平的滚动距离
返回值：对象
*/

function getScrollValue(){
  if (window.pageYOffset >= 0) {
    return {
      left: window.pageXOffset,
      top: window.pageYOffset
    }
  } else if (document.documentElement.scrollTop >= 0) {
    return {
      left: document.documentElement.scrollLeft,
      top:  document.documentElement.scrollTop
    }
  } else {
    return {
      left: document.body.scrollLeft,
      top:  document.body.scrollTop
    }
  }
}
```
