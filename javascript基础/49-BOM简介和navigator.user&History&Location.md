## 概念

js基础分为三部分: (1)ECMAScript (2)DOM (3)BOM


### 常见的BOM对象

BOM可以让我们用js来操控浏览器窗体

常见的BOM对象有:

(1) Window对象：代表整个浏览器的窗口，同时window也是网页的全局对象

(2) Navigator对象：代表当前浏览器的信息，通过该对象可以识别不同的浏览器

(3) Location对象: 代表当前浏览器的地址栏信息，通过Location可以获取地址栏信息，或者操控浏览器跳转页面

(4) History: 代表浏览器的历史记录，通过该对象可以操作浏览器的历史记录。但是由于隐私，不能获取具体的历史记录网址，

只能向前或向后翻页，而且操作只对当次访问有效

(5) Screen: 代表用户的屏幕信息，通过该对象可以获取用户的显示器的相关信息



## Navigator和`navigator.userAgent`

我们一般使用`navigator.userAgent`来获取浏览器信息。

userAgent是一个字符串，简称为UA，不同的浏览器有不同的UA

```javascript

 console.log(window.navigator.userAgent); //Mozilla/5.0 (Windows NT 10.0; WOW64; rv:68.0) Gecko/20100101 Firefox/68.0
 
```


## History对象

History对象可以操控浏览器向前或向后翻页

### History对象属性

```javascript

history.length

```

`length`属性来获取历史列表中url的数量。注意，当浏览器关闭后，length值会重置为1

### History对象的方法

(1) history.back(); 返回上一个页面

(2) history.forward(); 跳转到下一个页面

(3) history.go(int num);

```javascript

history.go(0); // 刷新当前页面

history.go(1); //向前跳转一个页面

history.go(2); // 向前跳转两个页面

history.go(-1); //返回一个页面，相当于history.back();

history.go(-2); //返回两个页面

```


## Location对象

Loaction对象: 封装了浏览器地址栏的信息

### Location对象的属性

`location.href`获取当前页面的url路径

修改location.href，页面会自动跳转到该路径，并生成相应的历史记录

### Location对象的方法

1. location.assign()

```javascript

location.assign(str);
location.assign('https://www.baidu.com');

```

地址跳转，和修改location.href相同

2. location.reload()

```javascript

location.reload(); // 重新加载页面
location.reload(true); // 传入参数true,则会强制清空缓存页面

```

3. location.replace()

使用一个新页面来替换当前页面，但不会生成历史记录，不能使用[后退按钮]来后退

