## 1.5 函数的防抖和节流


## 1.6 原型链

### 原型对象

1. 在JS中创建的每一个函数，解析器都会给函数添加一个属性prototype。这个属性对应着一个对象，就是原型对象

如果函数作为普通的函数调用原型没有任何作用，若是以构造函数调用原型，就可以在创建的实例对象中添加一个__proto__属性，该属性指向构造函数的原型，可以通过`实例.__proto__`来访问原型。

2. 使用`in`检查对象是否含有某个属性时，如果对象中没有但是原型中有，也会返回true

3. 使用`hasOwnProperty`可以检查对象自身是否含有属性


### 原型规则

1. 所有引用类型都具有对象特性，可以自由扩展属性,null除外

2. 所有的引用类型（对象，数组，函数）都有__proto__属性，隐式原型

3. 函数都有prototype对象，显式原型

4. 所有引用类型__proto__都指向prototype

5. 试图获取一个对象属性时，先到对象本身去寻找，若没有再到__proto__中寻找（即它的构造函数的prototype）


常见题目

1. 判断一个变量是不是数组类型/对象类型

```javascript

const arr = [];
const obj = {};

arr instanceof Array; // true
obj instanceof Object; // true

```

2. 写一个原型链继承的例子（封装DOM查询的例子）


3. 描述new一个对象的过程

  (1) 创建一个新对象

  (2) this指向这个对象

  (3) 执行代码(对this赋值)

  (4) 返回this


## 1.6.4 创建实例的方法

1. 字面量

```javascript

let obj1 = { 'name' : 'sky' };

```

2. 使用Object构造函数创建

```javascript

let obj2 = new Object();
obj2.name = 'sky';

```

3. 使用构造函数创建

```javascript

function Person(name) {
  this.name = name;
}

let obj3 = new Person('sky');

```

4. 使用工厂模式创建

```javascript

function createPerson(name) {
  let obj = new Object();
  obj.name = name;
  return obj;
}

let obj4 = createPerson('sky');

```

### 1.6.4 new运算符

```javascript

  function _new2(/*构造函数*/_constructor, /*构造函数参数*/...args) {
    // 创建一个空对象，继承构造函数的原型对象
    var o = Object.create(_constructor.prototype); 
    // 执行构造函数
    var result = _constructor.call(o, ...args); 
    // 判断是否是对象，是返回结果，不是返回构造函数的执行结果
    return result && result === 'Object' ? result : o; 
  }

```


## 1.7 继承的方式（重要！！）

1. 构造函数继承

```javascript

  function Parent(name) {
    this.name = name || 'Maggie';
  }

  Parent.prototype.sayHello = function() {
    console.log(123);
  }

  function Child(name) {
    Parent.call(this);
    this.name = name;
  }

  let sky = new Child('tianyu');
  console.log(sky.name);

```

缺点：不能继承父类的原型。 在父类原型中做修改，字类无法继承。


2. 原型链继承

```javascript
    function Parent(name) {
      this.name = name || 'zhang';
      this.age = 30;
      this.interests =  ['basketball', 'football'];
    }

    Parent.prototype.say = function() {
      console.log(13);
    };

    function Child(gender) {
      this.gender = gender;
    }

    Child.prototype = new Parent(); // 重要
    let sky = new Child();
    let violet = new Child();
    sky.age = 25;
    console.log(sky.age); // 25
    console.log(violet.age); // 30

    sky.interests.pop();
    console.log(sky.interests); // ['basketball']
    console.log(violet.interests); // ['basketball']

```

将父类实例赋值给子类原型，这样子类继承父类原型

缺点: 无法实现多继承; 如果child1实例修改父类中的属性，会影响child2（其他子类实例）的属性

原因： 子类共用原型，即`sky.__proto__ === violet.__proto__`

3. 组合继承 （原型链+构造函数继承）

解决了上述两种的问题，但是会让父类的构造方法执行两遍

```javascript

  function Parent(name) {
    this.name = name || 'zhang';
    this.age = 30;
    this.interests =  ['basketball', 'football'];
  }

  function Child() {
    Parent.call(this); // 重要1： 执行了Parent构造方法
  }

  Child.prototype = new Parent(); // 重要2：又重复执行了Parent构造方法

```

## 1.8 高阶函数

函数的参数是函数或返回函数

代码示例： 

```javascript

  function fn(a, b, callback) {
    console.log(a + b);
    callback&&callback(); //执行完console.log()语句再执行callback函数
  }

  function test() {
    console.log('1234');
  }

  fn(1,2); // 3
  fn(1,2, test); // 3 1234

```

### 1.8.1 常见的高阶函数

1. map()

`map`方法将数组所有成员依次传入函数参数，然后把每一次执行的结果组成一个新数组返回

代码示例

```javascript

  const arr = [1, 2, 3, 4];
  const arr2 = arr.map((n) => {
    return n + 2;
  });
  console.log(arr2); // [3, 4, 5, 6]
  console.log(arr); // [1, 2, 3, 4]

```

map返回的是一个新数组，原数组没有变化

`map`方法接收一个函数作为参数，参数函数可以传入三个参数：当前成员，当前成员位置，数组本身

```javascript

  const arr = [1, 2, 3, 4];
  const arr2 = arr.map((elem, index, arr) => {
    return elem *= index;
  });

  console.log(arr2); // [1, 4, 9, 16]

```


2. reduce(), reduceRight()

依次处理数组每个成员，最终累计成一个值。他们的差别是`reduce()`是从左到右处理，`reduceRight()`是从右到左处理。

这两种方法的第一个参数都是一个函数，该函数接受四种参数

  1） 累计变量，默认为数组第一个成员

  2） 当前变量，默认为数组第二个成员

  3） 当前位置，从0开始

  4） 原数组

如果要改变初始累计值，可以给`reduce()`和`reduceRight()`传递第二个参数

```javascript

  let arr = [1, 2, 3, 4];
  let total = arr.reduce((a, b) => {
    return a * b;
  }, 10); // 设置初始累计值为10

  console.log(total); // 10 * 1 * 2 * 3 * 4 = 240

```

3. filter()

`filter()`方法过滤数组，满足条件的成员组组成一个新数组返回。 `filter`方法参数是一个函数，该函数接受三个参数 1）当前成员 2）成员位置 3）当前数组

```javascript

  let arr = [1, 2, 3, 4];
  let arr2 = arr.filter((elem, index, array) => {
    return elem > 3;
  });
  console.log(arr2); // [ 4 ]

```

4. sort()

`sort()`方法对数组进行排序，默认是按照**字典顺序**排序。**排序后，原数组将被更改**

```javascript

  let arr1 = ['a', 'c', 'o', 'm'];
  let arr2 = [11, 101];
  let arr3 = [10111, 1101, 111];

  arr1.sort();
  arr2.sort();
  arr3.sort();
  console.log(arr1); // [ "a", "c", "m", "o" ]
  console.log(arr2); // [ 101, 11 ] 数值先被转换为字符串，再按照字典顺序进行比较
  console.log(arr3); // [ 10111, 1101, 111 ]

```

如果想让数组成员按照自定义规则排序，则传入一个函数作为参数。

参数函数本身可以传入两个参数，表示进行比较的两个数组成员。

```javascript

  let friends = [
    {name : 'child1', age: 12},
    {name : 'child2', age: 15},
    {name : 'child3', age: 9}
  ];

  friends.sort((a, b) => {
    return a.age - b.age;
  })

  console.log(friends);
//     0: Object { name: "child3", age: 9 }
// ​
//     1: Object { name: "child1", age: 12 }
// ​
//     2: Object { name: "child2", age: 15 } 

```

注意，自定义的排序函数应该返回数值，否则不同浏览器有不同的实现

```javascript

  let arr = [1, 4, 24, 2, 55, 0];
  arr.sort((a, b) => a < b); // bad
  arr.sort((a, b) => a - b); // good

```

## 2.1 对象的声明方式

```javascript

  // 1.字面量声明
  const obj1 = {a: 1}; // {a: 1}

  // 2.构造函数声明
  const obj2 = new Object({a: 1}); // {a: 1}

  // 3.Object.create()
  const obj3 = Object.create({a: 1}); // {}

```

### 2.1.4 三种方法的特点

1. 功能：都能实现对象的声明，并能够赋值和取值

2. 继承性：`Object.create`创建的对象继承到了`__proto__`属性上

3. 隐藏属性: 三种方式创建的对象都会为每个成员(属性或方法)生成一些隐藏属性

4. 属性读取: `ObjectGetOwnPropertyDescriptor()`或`GetOwnPropertyDescriptor()`

5. 属性设置: `Object.definePropertype`或`Object.defineProperties`


## 2.2 对象的属性


## 2.3 Symbol


## 2.4 遍历

### 2.4.1 一级对象遍历方法

- `for...in`方法 -> 遍历对象自身和继承的可遍历属性

- `Object.keys(obj)` -> 返回一个数组，包含自身（不包括继承）的可枚举属性

```javascript

  let obj1 = {
    name: 'obj1',
    author: 'sky'
  }

  console.log(Object.keys(obj1)); // ['name', 'author']

```

- `Object.getOwnPropertyNames(obj)` -> 返回一个数组，包括自身的所有可枚举属性

### 2.4.2 多级对象遍历

递归实现

```javascript

  let treeNodes = [
    {
      id: 1,
      name: '1',
      children: [
        {
          id: 11,
          name: '11',
          children: [
            {
              id: 111,
              name: '111'
            }
          ]
        }, 
        {
          id: 12,
          name: '12'
        }
      ]
    }, 
    {
      id: 2,
      name: '2',
      children: [
        {
          id: 21,
          name: '21',
          children: []
        }
      ]
    }
  ]

  let parseTreeJson = function(treeNodes) {
    if (!treeNodes || !treeNodes.length) return;

    for(let i = 0; i < treeNodes.length; i++) {
      let childs = treeNodes[i].children;

      console.log(treeNodes[i].id);

      if(childs && childs.length > 0 ) {
        parseTreeJson(childs);
      }
    }
  }

  parseTreeJson(treeNodes);

```


## 2.5 深度拷贝

### 2.5.1 Object.assign

`Object.assign`将对象的可枚举属性，复制到目标对象

语法:` Object.assign(目标对象, 源对象1, 源对象2...);`

代码示例:

```javascript

  let obj = {};
  let obj1 = { name: 'sky', gender: 'male' };
  let obj2 = { name: 'alice', age: 20 };

  Object.assign(obj, obj1, obj2); // 将obj1, obj2对象属性追加到obj中
  console.log(JSON.stringify(obj)); // {name: 'alice', gender: 'male', age: 20};

```

`JSON.stringfy`将对象转化为字符串

注意: *这是浅拷贝，只拷贝第一层对象*

### 2.5.2 深拷贝 

递归拷贝


## 2.6 数据拦截

-----暂时跳过-----

# 3. 数组

数组的考察基本是对数组方法多一点

## 3.1 扁平化N维数组

Array.flat(depth)方法会按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回, depth默认值为1


## 3.2 数组去重

`Set`是ES6新增加的数据结构，它**类似于数组**，但是成员都是唯一的，没有重复的值

```javascript

    const arr = [1, 4, 4, 2, 2, 3, 3, 4];
    const s = new Set(arr);
    console.log(s); // [ 1, 4, 2, 3 ]
    console.log(s.size) // size属性提供Set类型的长度

```

`Array.from`方法是ES6新增的数组方法，用于将两类对象转化为真正的数组：类数组对象和可遍历的对象(包括ES6新增的Set对象和Map对象)

```javascript

  function fn() {
    return Array.from(arguments);
  }

  console.log(fn(1,2,3)); // [1, 2, 3]

```

**数组去重**

代码示例:

```javascript

  let arr = [1, 10, 23, 10, 'abc', 'abc'];
  const s = new Set(arr); // 使用Set数据类型给数组去重
  arr = Array.from(s); // 使用Array.from方法把可遍历的对象类型Set转换为数组
  console.log(arr); // [ 1, 10, 23, "abc" ]

```

## 3.3 排序

### 3.3.1 sort方法

sort是js内置的排序方法，参数是一个函数

```javascript

  const arr = [1, 20, 8, 5];
  arr.sort((a, b) => a - b); // 升序  [ 1, 5, 8, 20 ]
  arr.sort((a, b) => b - a); // 降序  [ 20, 8, 5, 1 ]

```

### 3.3.2 冒泡排序&选择排序

冒泡排序： 左右两个成员比大小，大的放后面

```javascript

  Array.prototype.bubleSort = function() {
    let arr = this;
    let len = arr.length;
    for (let i = 0; i < len - 1; i++) {
      for (let j = 0; j < len - 1 - i; j++) {
        if (arr[j] > arr[j+1]) {
          [arr[j], arr[j+1]] = [arr[j+1], arr[j]]; // 数组解构赋值
        }
      }
    }
    return arr;
  }

```

选择排序：从第一个位置开始比较，找出最小的和第一个位置互换，继续下一轮

```javascript

  Array.prototype.selectSort = function() {
    let arr = this;
    let len = arr.length;
    for (let i = 0; i < len - 1; i++) {
      for (let j = i + 1; j < len; j++) {
        if (arr[i] > arr[j]) {
          [arr[i], arr[j]] = [arr[j], arr[i]];
        }
      }
    }
    return arr;  
  }

```

## 3.4 求数组最大值

`Math.max()`返回两个参数中较大值的那个

1. Math.max()与apply()结合

```javascript

  const arr = [1, 0.2, 20, 14];
  console.log(Math.max.apply(null, arr));

```

2. 数组reduce()方法

```javascript

  let max = arr.reduce((a, b) => {
    return Math.max(a, b);
  });
  console.log(max); // 20

```


## 3.5 数组求和

1. Array.reduce()

```javascript

  const arr = [1, 0.2, 20, 14];
  let sum = arr.reduce((pre, next) => {
    return pre + next;
  });
  console.log(sum); // 35.2

```

## 3.6 数组合并

1. concat() 

`concat()`方法用于多个数组的合并。它将新数组的成员添加到原数组之后，然后返回一个新数组，原数组不会改变

```javascript

  const arr1 = [1, 2, 'a', 200];
  const arr2 = [222, 333];
  const arr3 = arr1.concat(arr2);
  console.log(arr3); // [ 1, 2, "a", 200, 222, 333 ]
  console.log(arr1); // [ 1, 2, "a", 200 ] 原数组不会改变

```

2. Array.prototype.push.apply()

```javascript

  const arr1 = [1, 2, 'a', 200];
  const arr2 = [222, 333];
  Array.prototype.push.apply(arr1, arr2);
  console.log(arr1); // [ 1, 2, "a", 200, 222, 333 ]

```

## 3.7 判断数组是否包含某个值

```javascript

  const arr1 = [1, 2, 'a', 200];
  arr1.includes(4); // false
  arr1.indexOf(4); // -1， 如果存在返回索引

```
includes(), find(), findIndex()是ES6的api


## 3.8 类数组转换

`slice()`方法用于提取数组的一部分，返回一个新数组。接收两个参数，表示起始和终止位置。

```javascript

Array.prototype.slice.call(null, arguments);
Array.prototype.slice.apply(null, arguments);
Array.from(arguments); // ES6 api

```

## 3.9 每一项设置值

`fill`方法使用给定值，填充数组。该方法会填充掉已有的元素。

```javascript

  [1, 2, 3].fill(5); // [ 5, 5, 5 ]

  let arr = new Array(3).fill(8);
  console.log(arr); // [ 8, 8, 8 ]

```

## 3.10 每一项是否满足

every方法

`[1, 2, 3].every(item => item < 100); // true`


## 3.11 某一项满足条件

some方法

`[1, 100, 82].some(item => item < 0); // false`


## 3.12 过滤数组 

filter方法

```javascript

  let arr = [1, 2, 'a', 'b'];
  let arr2 = arr.filter(item => {
    return typeof item === 'string';
  })
  console.log(arr2); // [ "a", "b" ]

```


## 3.13 对象和数组转化

1. Object.keys()返回对象可枚举的属性名数组

```javascript

  const obj = {
    name: 'sky',
    age: 12
  }

  let arr = Object.keys(obj);
  console.log(arr); // [ "name", "age" ]

```


# 4. 数据结构篇