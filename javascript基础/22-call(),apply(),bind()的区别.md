## 前言
JS提供了一些方法来改变函数内部this指向，比如call(), apply(), bind()

## call()方法

call()方法的作用: 可以调用一个函数，并能够改变函数的this指向

call()方法基于上述作用，它也可以被用来实现继承

> fn.call(想要this指向哪里, 函数实参1, 函数实参2);

注意: 若不想改变函数指向, 则第一个参数传null

**代码示例1**

```javascript
function fn() {
    console.log(this);
    console.log(this.name);
}

const obj = {
    name: 'sky',
    age: 20
};

var name = 'windowName';
fn.call(this); //call()调用函数,但没有改变this的指向
```
> 打印结果: Window, windowName;

**代码示例2** 

```javascript
function fn(a, b) {
    console.log(this);
    console.log(this.name);
    console.log(a + b);
}

const obj = {
    name: 'sky',
    age: 20
};

fn.call(obj, 1, 1); //call()方法把this指向变成obj对象
```

> 打印结果: Object obj, sky , 2

**代码示例3**通过call()来实现继承:

```javascript
function Animal(myName, myAge) {
    this.name = myName;
    this.age = myAge;
}

function Dog(myName, myAge) {
    //通过call方法把Animal函数的this改为指向Dog，再给Dog加上相应的参数，使它拥有Animal中的属性，从而实现继承
    Animal.call(this, myName, myAge);
}

const beibei = new Dog('beibei', 2);
console.log(beibei); 
```

> 打印结果为: Object {name: 'beibei', age: 2};


## apply()方法

apply()方法的作用: 调用一个函数，并可以改变函数内部this的指向，这点和call()类似

apply()方法的应用: apply()需要传递数组，请看代码示例

> 语法: fn.apply(想要this指向哪里, [函数实参1, 函数实参2]);

**代码示例1: 通过apply()改变this指向**

```javascript
function fn(a) {
    console.log(this);
    console.log(this.name);
    console.log(a);
}

var obj1 = {
    name: 'sky',
    age: 18
}

fn.apply(obj1, ['hello world']);  //apply()先将this指向obj1对象，再执行fn函数
```

注意：apply()方法传入实参，需要以数组形式传入 

**代码示例2: 求数组最大值**

数组中没有直接求数组最大值的方法, 但数值里有`Math.max(num1, num2, num3)`方法，可以求多个数值的最大值。由于apply()方法传递实参必须是数组形式，所以可以两者组合

```javascript
var arr = [1, 0, 0.2, 100];

const maxValue = Math.max.apply(this, arr);
console.log(maxValue); //100

const minValue = Math.min.apply(this, arr);
console.log(minValue); //0
```

## bind()方法

bind()方法**不会调用函数**,但是可以改变函数内部this的指向

实际开发中bind()方法用的最频繁，当不需要立即调用函数，但是又想改变函数内部的this指向，就使用bind()

语法

> 新函数 = fn.bind(想要的this指向, 实参1, 实参2);

bind()不会调用fn函数，但是会返回指定this指向和指定实参的原函数拷贝