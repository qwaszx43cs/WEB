## style属性的获取和修改

在DOM中，若想设置样式：

1) className(针对内嵌样式)

2) style对象(针对行内样式)


**注意**： style对象只能获取行内样式，不能获取外链和内嵌的样式

```html

<body>
  <style>
    .red {
      border: solid 2px red;
    }   
  </style>

  <div class="red" style="background-color: #333; height: 200px; width: 200px"></div>

  <script>
    var myDiv = document.getElementsByClassName('red')[0];
    console.log(myDiv.style.border); // 无结果，style对象不能获取行内样式以外的样式
    console.log(myDiv.style); // 返回style对象
    console.log(myDiv.style.backgroundColor); 
    console.log(myDiv.style.height);
  </script>
</body>

```


### 通过js获取行内样式

方法一： 

```javascript

    节点元素.style.样式名;

```

方法二: 

```javascript

    节点元素.style['样式名(属性)'];

```

### 通过js设置行内元素

```javascript

    节点元素.style.样式名 = 属性值;

```