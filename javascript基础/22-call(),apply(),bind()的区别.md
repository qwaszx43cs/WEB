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