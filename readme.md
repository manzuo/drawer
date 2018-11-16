### 问题描述
在页面设计过程中，随着网页的放大，顶部导航栏也随着放大，导致导航栏会挡住页面的主要内容。
### 问题解决思路
首先要在页面中加入两个导航栏菜单，一个是默认的顶部导航栏，一个是抽屉导航栏菜单。默认情况下显示默认的顶部导航栏，当网页放大到一定比例的时候，隐藏默认的顶部导航栏，显示一个抽屉的图标，点击抽屉图标能设置抽屉菜单的隐藏和显示。
### 解决过程
抽屉菜单css部分：
```css
.top2{
    height: auto;
    width: 100%;
    background:rgb(189, 181, 181);
    overflow: hidden;
    white-space:nowrap;
    display: none;
}
.top2 a{
    text-decoration: none;
}
.top2 img{
    width: 20px;
    height: 20px;
    float: right;
}
.drawer{
    height: auto;
    width:100%;
    margin-left: 0;
    background:rgb(189, 181, 181);
    top: 0;/*离顶部的距离为0*/
    display: none;
}
.drawer ul{
    /* 清除ul标签的默认样式 */
    width: 100%;
    list-style-type: none;
    white-space:nowrap;
    overflow: hidden;
    margin-top: 0;         
    padding: 0;
}
.drawer li{
    width: 100%;
    position: relative;
    overflow: hidden;
}
.drawer li a{
   /* 设置链接内容显示的格式*/
    display: block; /* 把链接显示为块元素可使整个链接区域可点击 */
    color:white;
    text-align: center;
    padding: 3px;
    overflow: hidden;
    text-decoration: none; /* 去除下划线 */
    
}
.drawer li a:hover{
    /* 鼠标选中时背景变为黑色 */
    background-color: #111;
}
```


**抽屉菜单的html部分**：默认情况下，这部分是隐藏的。
```html
 <div class="top2">
            <a href="index.html" style="background: url(logo.png) no-repeat 33%,5%;background-size: 100% 100%;float: left;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</a>
            <img id="ico" src="drawer.png" alt="">
        </div>
    <div class="drawer">    
        <ul>
            <li>
                <a href="index.html">最新套图</a>
            </li>
            <li>
                <a href="thelatest.html">最新表情</a>
            </li>
            <li>
                <a href="search.html">表情搜索</a>
            </li>
            <li>
                <a href="http://www.doutula.com/maker">
                    <b>表情制作</b>
            
```

**下面是监测页面缩放大小的js函数**，并通过定时器每秒钟检测一次,当页面缩放到一定的比例时候，隐藏默认的顶部导航栏，显示抽屉菜单的抽屉的图标

```javascript
function detectZoom (){ 
  var ratio = 0,
    screen = window.screen,
    ua = navigator.userAgent.toLowerCase();
  var top1class = document.getElementsByClassName("top");
  var top1 = top1class[0];
  var top2class = document.getElementsByClassName("top2");
  var top2 = top2class[0];
  var drawerclass = document.getElementsByClassName("drawer");
  var drawer = drawerclass[0];
  var oImg = document.getElementById('ico');
   if (window.devicePixelRatio !== undefined) {
      ratio = window.devicePixelRatio;
  }
  else if (~ua.indexOf('msie')) {  
    if (screen.deviceXDPI && screen.logicalXDPI) {
      ratio = screen.deviceXDPI / screen.logicalXDPI;
    }
  }
  else if (window.outerWidth !== undefined && window.innerWidth !== undefined) {
    ratio = window.outerWidth / window.innerWidth;
  }
   
   if (ratio){
    ratio = Math.round(ratio * 100);
  }
   if(ratio>=375){
    top1.style.display='none';
    top2.style.display='block';
    
  }
   if(ratio<375){
    drawer.style.display='none'
    oImg.src = 'drawer.png';
    top2.style.display='none';
    top1.style.display='block';
  }
};
  var t1 = setInterval("detectZoom();",1000);
```

**控制抽屉菜单的显示和隐藏的js函数**

```javascript
window.onload = function () {
    var oImg = document.getElementById('ico');
    var drawerclass = document.getElementsByClassName("drawer");
    var drawer = drawerclass[0];
    var onOff = true;  //此处相当于设置了一个开关，通过设置布尔值，进而通过改变布尔值来完成if的判断！
    oImg.onclick = function () {
        if (onOff) {
            oImg.src = 'cha.png';
            onOff = false;
            drawer.style.display='block';
            
        } else {
            oImg.src = 'drawer.png';
            onOff = true;
            drawer.style.display='none';
        };
    };
};
```

### 效果截图
**默认顶部导航栏**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181116205651170.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21hbl96dW8=,size_16,color_FFFFFF,t_70)

**抽屉菜单**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20181116205735445.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21hbl96dW8=,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181116205910704.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L21hbl96dW8=,size_16,color_FFFFFF,t_70)

### 参考资料、网站

 [javascript检测浏览器的缩放状态实现代码](https://www.jb51.net/article/55753.htm)
 [js的判断以及图片的点击切换效果](https://blog.csdn.net/yingleiming/article/details/79895453)
