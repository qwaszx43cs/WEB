## 执行期上下文

当**函数执行**时，会创建一个执行期上下文的内部对象。执行期上下文决定了函数的环境。

每调用一次函数，就会创建一个执行期上下文，每个执行期上下文互相独立。当函数执行完毕后，执行期上下文就会销毁。

## this

解析器调用函数时都会向函数传递一个隐含的参数this,this指向执行期上下文对象。


### 函数内this的指向

函数的调用有六种方式，根据函数调用方式不同,this指向的对象也不同:

1. 以函数的形式调用(比如普通函数、立即调用函数、定时器函数)，this的指向永远都是window.

代码示例:

```javascript
function foo() {
    console.log(this);
    console.log(this.name);
}

var obj1 = {
    name: 'name1',
    sayName: foo
}

var obj2 = {
    name: 'name2',
    sayName: foo
}

var name = 'foo name';

foo(); 

```
> 打印结果Window, this.name为foo name

2. 以方法的形式调用，this指向调用方法的那个对象

代码示例: 

```javascript
function foo() {
    console.log(this);
    console.log(this.name);
}

var obj1 = {
    name: 'name1',
    sayName: foo
}

var obj2 = {
    name: 'name2',
    sayName: foo
}

var name = 'foo name';

obj1.sayName();
```

> 打印结果为Object obj1, name1

3. 以构造函数调用函数时，this指向的是实例对象

4. 以事件绑定函数的形式调用时，this指向的是**绑定事件的对象**

5. 以call,apply调用时，this指向的是指定的对象


### 箭头函数的this指向

ES6的箭头函数this指向不依照上述五条规则，而是会继承外层函数调用的this对象。

### 改变函数内部this指向

JS提供了一些方法改变this指向，比如call(),bind(),apply()