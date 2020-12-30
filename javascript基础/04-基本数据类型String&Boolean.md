## String字符串

字符串型是引号内的任意文本，因为为单引号`''`或双引号`""`。

### 引号的注意事项

1. 单引号和双引号不能混用；

2. 同类引号不能嵌套；

3. 单引号里可以嵌套双引号；双引号里可以嵌套单引号。

### 转义字符

在字符串中使用`\`作为转义字符，当表示一些特殊符号时可以使用`\`转义。

  - `\"`表示`"`双引号

  - `\'`表示`'`单引号

  - `\\`表示`\`

  - `\r`表示回车

  - `\n`表示换行，n表示new line

  - `\t`表示缩进，t表示tab

  - `\b`表示空格, b表示blank

  ### 获取字符串长度

  通过字符串的length属性获得字符串长度

  代码举例: 

  ```javascript
    var str = 'sky';
    console.log(str.length); // 3
  ```
  length属性在判断字符串长度时，一个中文字符、一个英文字符、一个中/英文标点符号、一个空格长度都为1。

  ### 字符串拼接

  **拼接语法**

  ```
  字符串 + 任意数据类型 = 拼接字符串
  ```

  代码举例(字符串拼接各种数据类型): 

  ```javascript
    var str1 = 'sky' + 'sky';
    var str2 = 'sky' + 1;
    var str3 = 'sky' + true;
    var str4 = 'sky' + null;
    var str5 = 'sky' + undefined;

    var obj = {name: 'sky'};
    var str6 = 'sky' + obj;

    console.log(str1); // skysky
    console.log(str2); // sky1
    console.log(str3); // skytrue
    console.log(str4); // skynull
    console.log(str5); // skyundefined
    console.log(str6); // sky[object Object]
  ```

## 字符串的不可变性

字符串的值不可改变，改变字符串变量的值实际上是改变了地址，内存中新开辟了内存空间。


## 模板字符串(ES6)

ES6语法中字符串拼接可以这么写: 

```javascript
  var name = 'sky';
  var age = 20;

  //传统写法
  console.log('my name is: ' + name + ' and I\'m ' + age);
  //ES6字符串拼接
  console.log(`my name is: ${name} and I\'m ${age}`);
```
> 使用反引号`\``,变量用${var}

## 模板字符串中可以换行

代码举例: 

```javascript
  var obj = {
    name: 'sky',
    age: 20,
    gender: 'male'
  };

  console.log(`
    name: ${obj.name}
    age: ${obj.age}
    gender: ${obj.gender}
  `);
```

## 模板字符串也可以调用函数

代码举例: 

```javascript
  var f = function() {
    return 'sky';
  }

  console.log(`my name is ${f()}`); //me name is sky
```

## 模板字符串支持嵌套使用

代码举例: 

```javascript
  const arr = ['sky', 'violet', 'maggie'];
  function f() {
    return `<ul>
      ${arrs
          .map((item)=> `<li>${item}</li>`)
          .join('')}
    </ul>`;
  }
  document.body.innerHTML = f();
```

### 布尔值Boolean

true表示真，false表示假