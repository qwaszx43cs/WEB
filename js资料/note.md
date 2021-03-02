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

1. map

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