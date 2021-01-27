## ES6的变量声明

在ES5中，使用`var`定义全局变量

ES6中，新增了`let`和`const`来定义变量: 

`let`定义局部变量

`const`定义常量（常量定义后，不可修改）


### 1. var: 定义全局变量

```javascript

    {
      var a = 1;
    }

    console.log(a); //输出1

```

即使在区块中使用var定义变量，这个变量还是全局变量，在全局起作用。

**使用var定义变量不具备块级作用域特性**

```javascript

    var a = 1; 
    {
      var a = 2;
    }

    console.log(a); //2

```

上方代码输出为2，因为var是全局声明的

**总结**

使用var定义全部的变量，有时候会污染js变量的作用域。**尽量避免使用**var定义变量


### 2. let： 定义局部变量

举例1:

```javascript

    {
      let a = 1;
    }
    console.log(a); //reference error: a is not defined

```

举例2:

```javascript

    var a = 2;

    {
      let a = 1;
    }
    console.log(a); //2

```

从上两个代码可以看出，let定义变量只在块级作用域（局部）起作用

习惯使用let可以减少使用var带来的污染全局变量

**经典面试题**

```javascript

    for (var i = 0; i < 10; i++) {
      console.log('循环体中i = ' + i);
    }
    console.log('循环体外i = ' + i);

```

上述代码可以正常打印结果，循环体外的也能打印，因为循环体外的变量i是全局变量。

```javascript

    for (let i = 0; i < 10; i++) {
      console.log('循环体中i = ' + i);
    }
    console.log('循环体外i = ' + i);

```

上述代码的关键在于：每一次循环都会创建一个新的块级作用域，每个块级作用域中会重新定义一个新的变量i。而且循环体外的i打印会报错，因为let定义变量只在块级作用域中有效。


### 3. const: 定义常量

用const声明的常量只在块级作用域中起作用，而且const声明常量必须赋值，否则会报错

**重要:**

const声明的基本类型数据，无法修改

const声明的引用类型数据，无法修改的是对内存地址的引用，但是对象中的数据是可以修改的

## var, let, const 

### let和const的特点(重要)

1. 不存在变量提升

2. 禁止重复声明

3. 暂时性死区

4. 支持块级作用域

相反，`var`存在重复声明，变量提升和不支持块级作用域

### var/let/const 共同点


### var/let/const的区别

1. var声明的变量会挂载到window对象上，而let和const声明的变量不会

代码示例:

```javascript

    var a = 1;
    console.log(a); // 1
    console.log(window.a); //1

    let b = 2;
    console.log(b); // 2
    console.log(window.b); // undefined

    const c = 3;
    console.log(c); // 3
    console.log(window.c); // undefined

```

var的这一特性会污染window全局变量，

```javascript

var innerHeight = 100;
console.log(window.innerHeight); //100

```

使用var声明全局变量innerHeight时，值覆盖了window对象自带的innerHeight属性


2. var存在变量提升，而let和const不存在变量提升

3. var声明不存在块级作用域，而let和const声明存在块级作用域

4. 同一作用域下，var可以重复声明变量，let和const不行

```javascript

    var a = 1;
    var a = 2;
    console.log(a);

    let b = 1;
    let b = 3;
    console.log(b); //SyntaxError: redeclaraion of let b

```

5. let和const存在暂时性死区

6. const声明常量必须赋值


## for循环举例（经典案例）

```html

  <div>
    <button>btn1</button>
    <button>btn2</button>
    <button>btn3</button>
    <button>btn4</button>
  </div>

  <script>
    let myBtn = document.getElementsByTagName('button');
    let myDiv = document.getElementsByTagName('div');

    for (var i = 0; i < myBtn.length; i++) {
      myBtn[i].addEventListener('click', () => {
        alert(i);
      });
    }

    // 事件委托
    // myDiv[0].addEventListener('click', (e) => {
    //   alert(e.target.innerHTML);
    // });
  </script>

```

**使用var定义的是全局变量，for循环是同步代码，onclick点击事件是异步代码，当还没onclick时，for循环代码已经执行完毕，变量i值为4**

应该使用let替换var

```html


  <div>
    <button>btn1</button>
    <button>btn2</button>
    <button>btn3</button>
    <button>btn4</button>
  </div>

  <script>
    let myBtn = document.getElementsByTagName('button');
    let myDiv = document.getElementsByTagName('div');

    for (let i = 0; i < myBtn.length; i++) {
      myBtn[i].addEventListener('click', () => {
        alert(i);
      });
    }

    // 事件委托
    // myDiv[0].addEventListener('click', (e) => {
    //   alert(e.target.innerHTML);
    // });
  </script>

```

每次循环都会创建一个新的变量i


## 暂时性死区

使用let和const声明的变量，需要先声明再使用