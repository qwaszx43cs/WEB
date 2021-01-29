## 异步调用

### 异步

javascript的执行环境是单线程

常见的异步模式有如下几种: 

- 定时器

- 接口调用

- 事件函数


### 接口调用的方式

js 中常见的接口调用方式

1. ajax

2. 基于jquery的ajax

3. Fetch

4. Promise

5. axios


## Promise概述

### Promise的介绍和优点

Promise对象**可以用同步的代码来写异步的代码**，使用Promise的好处:

1. 很好的解决**回调地狱**的问题(避免了层层嵌套的回调函数)

2. Promise提供了简介的api,使得异步操作更加容易


### Promise的基本用法

(1) 使用new实例化一个Promise实例，Promise构造函数中传递一个参数。这个参数是一个函数，该函数用来处理异步任务

(2) 函数中传入两个参数resolve和reject，是异步执行成功和失败后分别执行的回调函数

(3) 通过promise.then()处理返回结果,这里的`promise`是Promise实例


```javascript

    // 假设是数据库数据
    const myJson = {
      "name": "sky",
      "age": 12
    };

    // 第一步：model层的promise封装
    const promise = new Promise((resolve, reject) => {
      // 异步任务(ajax任务，这里用一个定时器代替)
      setTimeout(() => {
        let data = myJson; //接口返回的数据
        if (data.name === 'sky') {
          resolve(data);
        } else {
          reject(data);
        }
      }, 1000);

    });

    // 第二步: 业务层的接口调用
    promise
      .then((data) => {
        console.log(data);
      })
      .catch((data) => {
        console.log(data);
      })

```

### promise对象的三个状态(了解即可)

1. 初始化状态: pending

2. 成功状态: fulfiled

3. 失败状态: rejected


## 基于Promise处理多次ajax请求-链式调用(重要)

```javascript

    const request = require('request');

    // Promise封装接口1
    const request1 = () => {
      const promise = new Promise((resolve, reject) => {
        request('htttp://www.baidu.com', function (response) {
          if (response.retCode == 200) {
            //成功状态，返回数据
            resolve('request1 success' + response);
          } else {
            reject('request1 failed');
          }
        });
      });

      return promise;
    };

    // Promise封装接口2
    const request2 = () => {
      const promise = new Promise((resolve, reject) => {
        request('http://www.taobao.com', (response) => {
          if (response.retCode == 200) {
            resolve('request2 success' + response);
          } else {
            reject('request2 failed')
          }
        });

        return promise;
      });
    };

    request1()
      .then((response) => {
        // 接口1成功后
        console.log('request1 success');
        return request2();
      })
      .then((response) => {
        console.log('request2 success');
      }) //如果有更多ajax，方法同上，链式调用


```

## return的函数返回值

对promise的封装,return后面的返回值有两种情况：

(1) return返回promise实例, 该实例对象会调用下一个then

(2) return返回普通值,返回的普通值会直接传递给下一个then，通过then参数中的参数接收该值

情况：返回Promise普通值(返回promise的实例和上面ajax链式调用差不多)

```javascript

    /*基于Promise封装ajax*/
    function queryData(url) {
      return new Promise((resolve, reject) => {
        var xhr = new XMLHttpRequest();
        xhr.onreadystatechange = () => {
          if (xhr.readyState != 4) return;
          if (xhr.readyState == 4 && xhr.status == 200) {
            // 成功
            resolve(xhr.responseText);
          } else {
            reject('failed');
          }
        };

        xhr.responseType = 'json'; //设置返回的数据类型
        xhr.open('get', url);
        xhr.send(null); //请求接口
      });
    }

    queryData('http://www.baidu.com')
      .then((data1) => {
        console.log(JSON.stringfy(data1));
        return queryData('http://www.taobao.com');
      })
      .then((data2) => {
        console.log(JSON.stringfy(data2));
        return 'abcd';
      })


      // 上面返回的是一个普通值,那这里的then是谁来调用呢？
      // 这里会产生一个新的Promise实例，调用then
      .then((data3) => {
        console.log(data3); //abcd
      })

```

## Promise的常用api: 实例方法（重要）

(1) `promise.then();` 获取异步任务的正常结果

(2) `promise.catch();` 获取异步任务的异常结果

(3) `promise.finally();` 无论异步任务结果如何都会运行



## Promise的常用api：对象方法

(1) `Promise.all();` 并发处理多个异步任务，若所有任务都执行成功，返回结果

(2) `Promise.race();` 并发处理多个异步任务，只要有一个任务执行成功，返回结果

代码示例: 

```javascript

function queryDate(url) {
  //代码同上面的queryData()方法
}

var promise1 = queryData('http://www.baidu.com');
var promise2 = queryData('http://www.taobao.com');
var promise3 = queryData('http://www.google.com');

Promise.all([promise1, promise2, promise3])
.then((res) => {
  console.log(res);
});

Promise.race([promise1, promise2, promise3])
.then((res) => {
  console.log(res);
})

```

`Promise.all([promise实例1, promise实例2...])`