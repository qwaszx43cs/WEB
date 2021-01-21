## 前言

JS动画内容主要包括如下:

1. 三大系列和事件对象

三大系列: offset/scroll/client

事件对象: event(事件被触动时，键盘和鼠标的状态)


2. 动画(闪现\匀速\缓动)

3. 冒泡/兼容/封装


## offset

offset能方便的获取元素尺寸, 其包括:

- offsetWidth

- offsetHeight

- offsetLeft

- offsetTop

- offsetParent


### 1. offsetWidth&offsetHeight

这两个属性能获取元素的**宽高+padding+border**，不包含margin：

offsetWidth = width + padding + border

offsetHeight = height + padding + border

代码示例:

```html

<body>
  <style>
    .red {
      border: solid 2px red;
      padding: 10px;
    }   
  </style>

  <div class="red" style="background-color: #333; height: 200px; width: 100px"></div>

  <script>
    var myDiv = document.getElementsByClassName('red')[0];
    console.log(myDiv.offsetHeight); //200 + 10 * 2 + 2 * 2
    console.log(myDiv.offsetWidth); // 100 + 10 * 2 + 2 * 2
  </script>
</body>


```


### 2. offsetParent

`offsetParent`: 获取当前元素的定位父元素

    - 若当前元素的父元素有css定位，如position: fixed,absolute,relative,那么offsetParent为最近的父元素

    - 若当前元素的父元素没有css定位，则offsetParent为body元素

代码示例: 

```javascript

<body>
  <style>
    .nav {
      position: relative;
    }

    .nav-content {
      position: absolute;
    }
  </style>

  <div class="nav">
    <div class="nav-content"></div>
  </div>

  <div class="red"></div>

  <script>
    var myContent = document.querySelector('.nav-content');
    var myRed = document.querySelector('.red');

    console.log(typeof myContent.offsetParent); // 父元素nav
    console.log(myRed.offsetParent); //body
  </script>
</body>

```

### offsetTop&offsetLeft

offsetTop: 当前元素对于其定位父元素的垂直偏移量。

offsetLeft: 当前元素对于其定位父元素的水平偏移量。

注意: 从父元素的padding开始算距离，border不算

**offsetLeft和style.left区别**

1. offsetLeft可以返回无定位父元素的偏移量。如果父元素中都没有定位，则以body为准。

style.left只能获取行内样式，如果父元素没有设置定位，则返回""（空字符串）

```html

<body>
  <style>
    .box1 {
      height: 300px;
      width: 300px;
      background-color: aqua;
      position:absolute;
    }
  </style>

  <div class="box1"></div>

  <script>
    var myBox = document.querySelector('.box1');

    console.log(myBox.offsetLeft); 
    console.log(typeof myBox.offsetLeft); //number
    console.log(myBox.style.left); //空字符串
    console.log(typeof myBox.style.left); //string
  </script>
</body>

```

2. offsetLeft返回的是数值, style.left/top返回的是字符串，而且带有单位px。

3. offsetLeft和offsetTop是只读，而style.left和style.top是可读写的。

**总结：**我们一般用offsetLeft取值，用style.left来赋值


## 动画的种类

- 闪现（基本不用）

- 匀速 

- 缓动

### 匀速动画的封装: 每间隔30ms，移动盒子10px

