## Javascript部分

### 1. JS内置数据类型

> 面试题1: 如何理解“一切皆是对象?” number是对象吗，字符串是对象吗

不是，一切引用类型都是对象

> 面试题2：基础类型存放在栈上，引用类型存放在堆上，请问是为什么？ 字符串是存放在栈上么？对象中有一个 number 属性，那么 number 属性是存放在堆上还是栈上？

对象中的基本类型变量是保存在栈内存上，对象是保存在堆内存中


### 2. typeof

1. typeof 和 instanceof 区别

- typeof是一元运算符，返回数据类型的字符串。对基本类型能准确判断，引用类型(null, Object)除了函数类型返回`'function'`以外，都返回`Object`

typeof 对于未声明和声明后未初始化的变量都返回`'undefined'` 

准确返回对象数据类型`Object.prototype.toString.call()`, 返回`[object ObjectType]`字符串

- A instanceof B用来检查A的原型链中是否有B构造函数的原型

手写一个instanceof方法

```javascript

  /*
    param a 要判断的对象
    param b 构造函数
  */    
  function _instanceof(a, b) {
    while (a.__proto__ == null) {
      if (a.__proto__ == null) {
        return false;
      }
      if (a.__proto__ == b.prototype) {
        return true;
      } 
      a.__proto__ = a.__proto__.__proto__;
    }
  }

  console.log([] instanceof Array); // true

```


### 3. Array

1. 类数组转换为数组

```javascript

Array.prototype.slice.call(null, arguments);
Array.from(arguments); // ES6

```
2. 扁平化多维数组

`Array.flat(depth)`

3. 数组去重

  - set数据类型 + Array.from()

  set是ES6新出的数据结构，它类似于数组，没有重复的成员；

  Array.from()将两类对象转化为数组，类数组和可遍历的对象

  ```javascript
  
    let arr = [1, 2, 'sky', 'sky', 20 ,9 , 9, 1];

    let arr1 = Array.from(new Set(arr));
    
    console.log(arr1); // [ 1, 2, "sky", 20, 9 ]
  
  ```

4. 数组排序

  - `sort()`: 无参数时按照Unicode排序，从小到大

  ```javascript
  
    let arr = [1, 2, 20 ,9 , 9, 1];

    result = arr.sort((a, b) => a - b); // 从小到大
    result = arr.sort((a, b) => b - a); // 从大到小
    console.log(result); // [ 1, 1, 2, 9, 9, 20 ]

  ```

  - 冒泡排序

  左右两个成员比较大小，大的放右边

  ```javascript
  
    let arr = [1, 2, 20 ,99 , 9, 1, 31];

    function bubbleSort(arr) {
      let length = arr.length;
      for (let i = 0; i < length - 1; i++) {
        for (let j = 0; j < length - 1 - i; j ++) {
          if (arr[j] > arr[j + 1]) {
            [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
          }
        }
      }
      return arr;
    }

  
  ```

  - 选择排序

  从第一个位置开始比较，找出最小的和第一个换位置，再到下一轮

  ```javascript
  
    let arr = [1, 2, 20 ,99 , 9, 1, 31];

    function selectSort(arr) {
      let length = arr.length;
      for (let i = 0; i < length - 1; i++) {
        for (let j = i + 1; j < length; j ++) {
          if (arr[i] > arr[j]) {
            [arr[i], arr[j]] = [arr[j], arr[i]];
          }
        }
      }
      return arr;
    }

    console.log(selectSort(arr));

  
  ```

5. 求数组最大值

  - Math.max()与apply()方法结合

  ```javascript

    let arr = [1, 2, 20 ,99 , 9, 1, 31];

    let max = Math.max.apply(null, arr);
    console.log(max); // 99

  ```
  
  - reduce()方法

  ```javascript
  
    let arr = [1, 2, 20 ,99 , 9, 1, 31];

    let max = arr.reduce((a, b) => {
      return Math.max(a, b);
    });

    console.log(max); // 99
  
  ```

6. 数组求和

- Array.reduce()

```javascript

    let arr = [1, 2, 20 ,99 , 9, 1, 31];
    let result = arr.reduce((a, b) => {
      return a + b;
    });

    console.log(result); // 163

```

7. 数组合并

- Array.concat()

在数组最后添加新成员，返回一个新成员，不会改变原数组

```javascript

  let arr1 = [1, 2, 3];
  let arr2 = ['a', 'b', '123'];

  let newArr = arr1.concat(arr2);
  console.log(newArr); // [ 1, 2, 3, "a", "b", "123" ]

```

- Array.prototype.push.apply()

```javascript

  let arr1 = [1, 2, 3];
  let arr2 = ['a', 'b', '123'];

  Array.prototype.push.apply(arr1, arr2);
  console.log(arr1); // [ 1, 2, 3, "a", "b", "123" ]

```

8. 判断数组是否包含某个值

- `indexOf()`可以返回某个值是否在数组存在，存在返回索引，不存在返回`-1`

```javascript

  let arr1 = [1, 2, 3, 'a', 'bbb'];
  console.log(arr1.indexOf('a')); // 3
  console.log(arr1.indexOf('c')); // -1
  console.log(arr1.include(1)); // true
```

- `includes()` ES6语法,返回布尔值

9. 类数组转换

- `Array.from()`可以将两类对象转为数组：1）类数组 2）可遍历的对象(如Set, Map等)

```javascript

Array.prototype.slice.apply(null, arguments);
Array.from(arguments);

```
- `Array.prototype.slice.apply()`

10. 为数组的每一项设置值

`fill`方法给定数值，覆盖原有数组成员，填充数组

```javascript

let arr = [1 , 2];
arr.fill(5); // [5, 5]
let arr2 = new Array(8).fill(1);

```

11. 数组的每一项是否满足

`every`方法, 参数是一个返回函数，全部满足返回true

```javascript

  let arr1 = [1, 2, 3, 4, 2];
  console.log(arr1.every(item => item > 3;})); //false

```

12. 过滤数组

`filter`方法

```javascript

  let arr1 = [1, 2, 3, 4, 2];
  let newArr = arr1.filter(item => item < 4);
  console.log(newArr); // [ 1, 2, 3, 2 ]

```

13. 对象和数组转换

- `Object.keys`方法, 该方法可以返回对象的可枚举属性数组

```javascript

  let obj = {
    name : '123',
    age: 23
  };

  console.log(Object.keys(obj)); // [ "name", "age" ]

```

### 4. Number 

1. 0.1 + 0.2 === 0.3 为什么是false

0.1 和 0.2 的二进制表示不精确，加起来的值不是精确的0.3

2. 上述问题解决方法

将错误舍入做一个小的容差

```javascript

  function numberComparison(a, b) {
    return Math.abs(a - b) < Number.EPSILON;
  }

  console.log(numberComparison(0.1 + 0.2, 0.3));


```

3. 数值的进制

- 十进制：没有前导的数值 + 数字0-9

- 八进制：前导0o或0O + 数字0-7

- 十六进制：前导0x或0X

- 二进制： 前导0b或0B + 数字0-1

### 5. 类型转换（强制类型转换&隐式转换）

### 6. 作用域

> 面试题1：作用域的本质是什么， 闭包和作用域的关系是什么？

> 面试题2：let,const,var三者的区别，为什么不推荐var

> 面试题3：关于变量提升

### 7. 闭包

- 什么是闭包，闭包的作用，闭包的应用

闭包是指有权访问另一个私有作用域的函数。最常见的创建闭包的方式，就是在函数中再创建另一个函数

闭包应用1：可以在函数的外部访问函数内部的变量

闭包应用2：使已经运行结束的函数上下文中的变量对象继续存在内存中。因为闭包保持了对对象的引用，所以该对象不会被回收

- 题目：闭包定时器

```javascript

for (var i = 0; i < 5; i++) {
  ((x) => {
    setTimeout(()=> {
      console.log(x);
    }, x * 1000);
  })(i);
}

```

### 8. 引用和操作符优先级

- 题1： 尝试答出下图的打印结果

```javascript

    let a = { x : 1 };
    let b = a;
    a = a.x = { x : 1 };
    console.log(a); 
    console.log(b);

```

### 9. 原型和继承

继承是一种代码复用方式

- new 运算符

  1) 创建一个空对象

  2) 设置对象__proto__指向构造函数prototype

  3) 将构造函数的this指向该对象，执行构造函数

  4) 若无其他返回结果，返回新对象

  手写一个new运算符

  ```javascript
  
    // params 
    // _constructor 构造函数
    // ...args 其他参数
    function _new(_constructor, ...args) {
      // 创建新对象,并将__proto__指向构造函数的prototype
      let obj = Object.create(_constructor.prototype);
      // 更改this指向,运行构造函数
      let result = Object.prototype.apply(_constructor, args);
      // 判断返回结果
      return (result && typeof result === 'object') ? result : obj;
    }
  
  ```

- 创建对象实例的几种方式，字面量和Object构造函数创建的区别

  1）字面量

  2) Object构造函数（new Object()）

  3) Object.create()创建对象

  4) 构造函数创建

  5) 工厂模式创建

  **字面量创建对象** 1）代码简洁高效 2）代码速度快 3）构造函数接收参数，会委托内置的另一个构造函数，返回一个对象实例，但结果往往不是想要的

  **new/字面量创建对象和Object.create(null)区别**： 通过前两种方式创建的对象都能继承原型，而Object.create（null)把新建对象的__proto__指向null, 也就没有继承属性和方法了

- 继承的方法及其特点

  1. 构造函数继承

    在子类构造函数中调用父类构造方法，改变this指向。优缺点为可以实现多继承，但是不能继承父类原型属性和方法

    ```javascript
    
    function Person(name) {
      this.name = name || 'sky';
    }

    Person.prototype.say = function() {
      console.log('hello');
    };

    function Student(height) {
      Person.call(this);
      this.height = height;
    }

    let sky = new Student(175);
    sky.say(); // Uncaught TypeError

    ```

  2. 组合继承

  可以多继承及继承父类原型的属性和方法，但是会调用两次父类构造函数

  ```javascript
  
    function Person(name) {
      this.name = name || 'sky';
    }

    Person.prototype.say = function() {
      console.log('hello');
    };

    function Student(height) {
      Person.call(this);
      this.height = height;
    }

    // 使用原型链的方法实现对父类原型的继承
    Student.prototype = new Person();
    Student.prototype.constructor = Student;

    let sky = new Student(175);
    sky.say(); 
  
  ```

  3.  寄生组合继承

- 原型是什么，原型链是什么，怎么解释？

  1. 所有函数除了`Object.prototype.bind`都有一个prototype对象属性，该对象属性可以被构造函数创建的实例继承。

  2. 所有对象都有`__proto__`属性，指向其构造函数的原型对象。原型对象又有其原型对象。原型链就靠着__proto__链接。

  3. 所有函数对象的__proto__指向`Function()`的prototype

  4. 所有原型对象的__proto__指向`Object()`的prototype，而Object()的__proto__指向原型链顶端null

  5. Function() 构造函数本身就是Function()实例，所以__proto__指向Function()原型对象


### 10. this

箭头函数的this指向为其外层第一个不是箭头函数的函数的this指向

### 11. 防抖和节流（暂时不看）

防抖和节流都是防止函数多次被调用；

防抖是函数一直被触发，且每次触发的间隙小于wait，则在防抖的情况下只调用一次

节流是函数每隔一定时间(wait参数)运行一次

### 12. 深浅拷贝

对基本类型的拷贝是拷贝值，对引用类型的拷贝是拷贝值的地址


- `Object.assign`实现浅拷贝

- 深拷贝-递归浅拷贝

```javascript

    function deepClone(target) {
      const obj = target.constructor == Array ? [] : {};
      for(let keys in target) {
        if (target.hasOwnProperty(i)) {
          if (typeof target[keys] == 'object') {
            obj[keys] = target[keys].constructor === Array ? [] : {};
            obj[keys] = deepClone(target[keys]);   
          } else {
            obj[keys] = target[keys];
          }
        }
      }
      return obj;
    }


```


### 13. 异步操作

### 14. Promise

> 面试题： Promise是什么？用来解决什么问题？如何使用？