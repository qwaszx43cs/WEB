## 节点

构成网页的最基本单元。所有节点都是Object,常见节点分为四类:

(1) 文档节点: 整个HTML文档

(2) 元素节点: HTML标签

(3) 属性节点: 元素属性

(4) 文本节点: 标签中的文本内容

## DOM


### 元素节点的获取

```javascript

var myDiv1 = document.getElementById('div1');

var myDiv2 = document.getElementsByTagName('div');

var myDiv3 = document.getElementsByClassName('red');

```

myDiv2和myDiv3是标签数组


### DOM访问关系的获取

节点的访问关系都是**属性**，JS中的父子兄访问关系:


**获取父节点**

> 节点.parentNode;


**获取兄弟节点**

1. 下一个元素节点

> 节点.nextSibling;

2. 前一个兄弟节点

> 节点.previousSibiling;

3. 获取任意一个兄弟节点

> 节点.parentNode.chilren[index];


**获取单个子节点**

1. 第一个子节点

> 节点.firstChild;

2. 最后一个子节点

> 节点.lastChild;

3. 获取所有子节点

childNodes: 标准属性，返回指定元素所有子节点的集合(包括元素节点，文本节点，所有属性节点)

> 节点.childNodes;

**childrens**: 非标准属性，返回指定元素的子节点集合。它只返回HTML节点，甚至不返回文本节点

> 子节点数组 = 指定元素.childrens;



## DOM节点操作

节点的操作都是函数(方法

### 创建节点

```javascript

新的标签（元素）节点名 = document.createElement('标签名');

```

### 插入节点

方式1: 

```javascript
父节点.appendChild(插入节点);

```

代码示例:

```html
<body>
  <ul id="myList">
    <li></li>
    <li></li>
    <li></li>
  </ul>

  <script>
    var myList = document.getElementById('myList');
    var newList = document.createElement('li');
    newList.innerHTML = 'hello world';

    myList.appendChild(newList);
  </script>
</body>
```

方式2:

```javascript
父节点.insertBefore(新节点, 参考节点);
```

如果参考点为null,到父节点的最后插入新节点


### 删除节点

```javascript

父节点.removeChild(子节点);

```

如果要删除自身,也可以这么写

```javascript

node.parentNode.removeChild(node);

```


### 复制节点

```javascript

要复制的节点.clone();
要复制的节点.clone(true); //参数带不带true都可以

```


## 设置节点的属性

```html
<img src="img/image1.jpg" alt="pic" title="图片一张" id="myPic" class="square-pic">
```

1. 获取节点的属性值

方法一: 获取属性值

```javascript
    
    var myPic = document.getElementsByTagName('img')[0];
    console.log(myPic.src);
    console.log(myPic.alt);
    console.log(myPic.title);
    console.log(myPic.id);
    console.log(myPic.className); //注意是className，而不是class

```

方法二: getAttribute()方法

```javascript

    var myPic = document.getElementsByTagName('img')[0];
    console.log(myPic.getAttribute('alt'));
    console.log(myPic.getAttribute('title'));
    console.log(myPic.getAttribute('class')); //注意是class，不是className

```


2. 设置节点的属性值

方法一: 直接设置属性值

```javascript

myPic.alt = 'changed';

```

方法二: setAttribute()方法

myPic.setAttribute('alt', 'changed');


3. 删除节点属性值

```javascript

节点名.removeAttribute(属性名);

myPic.removeAttribute('id');

```

### 节点属性总结

获取节点属性和设置节点属性都有两种方法。

对于**节点的原始属性**，两种方法可以混用（比如普通标签的id,class,styles等属性，img标签的`alt`属性等）。`myPic.title='这是个标题'`来设置title属性，`myPic.getAttribute('title');`来获取属性值。

但如果是非原始属性，方法一和方法二有区别

```javascript

myPic.aaa = 'forExample';

myPic.setAttribute('aaa', 'forExample');

```

区别如下：

  1. 方法一`节点.属性值`或`节点['属性值']`: 绑定的属性值不会出现在标签上

  2. 方法二 get/set/remove操作的属性值会出现在标签上


## DOM补充

### innerHTML和innerText的区别

innerHTML: 双闭合标签里的内容(包括标签)

innerText: 双闭合标签里的内容(不包括标签)

代码示例:

```html

<body>
  <div>
    <p>hello world</p>
  </div>


  <script>
    var myDiv = document.getElementsByTagName('div')[0];
    console.log(myDiv.innerHTML); // <p>hello world</p>
    console.log(myDiv.innerText); // hello world
  </script>
</body>


```


### nodeType属性

- nodeType == 1 表示的是元素节点（标签）

- nodeType == 2 表示的是属性节点

- nodeType == 3 表示的是文本节点

nodeType,nodeName,nodeValue

代码示例:

```html

<body>
  <div id="myDiv">hello world</div>

  <script>
    var myDiv = document.getElementById('myDiv');
    var attribute = myDiv.getAttributeNode('id');
    var txt = myDiv.firstChild;

    // 获取nodeType
    console.log(myDiv.nodeType); //1
    console.log(attribute.nodeType); //2
    console.log(txt.nodeType); //3

    // 获取nodeName
    console.log(myDiv.nodeName); //div
    console.log(attribute.nodeName); //id
    console.log(txt.nodeName); //#text


    // 获取nodevalue
    console.log(myDiv.nodeValue); //null
    console.log(attribute.nodeValue); //myDiv
    console.log(txt.nodeValue); // hello world
  </script>
</body>


```


## 文档的加载