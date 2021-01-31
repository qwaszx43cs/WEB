## Vue实例

每个Vue应用都是通过vue函数创建的Vue实例开始的
```html

    <div id="app">
        {{ msg }}
    </div>

    <!-- 开发环境版本，包含了有帮助的命令行警告 -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

    <script>
        var app = new Vue({
            el: '#app',
            data: {
                msg: 'hello world'
            }
        });
    </script>

```

### el属性

el属性指定了当前Vue实例管理的视图

```javascript

    app.$el  = document.getElementById('app');

```

不能直接管理html或body

## 数据与方法

当一个Vue实例被创建，它的选项对象中的`data`对象的所有property就会加入到vue的响应式系统中，值改变,视图也会响应。

**注意**:只有当vue实例被创建时就存在的data中的property才是响应式的

```html
    <div id="app">
        {{ msg }}
        {{ name }}
    </div>

    <!-- 开发环境版本，包含了有帮助的命令行警告 -->
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

    <script>
        var app = new Vue({
            el: '#app',
            data: {
                msg: 'hello world'
            }
        });

        app.name = 'sky'; //对name的改动不会影响视图
    </script>

```

上述代码`vue实例.属性名`可以直接访问数据，也可以`vue实例.$data.属性名`访问数据



若需要一些property，但是一开始值为空或不存在，需要设置一下初始值

``` javascript

        var app = new Vue({
            el: '#app',
            data: {
                msg: 'hello world',
                name: '',
                age: null
            }
        });

```

### 有用的实例与方法

除了数据property，还有一些有用的实例与方法。他们都有前缀`$`

例如: 

1. `$data`: Vue实例的数据对象。Vue实例代理了对其data对象property的访问

2. `$el`: Vue实例使用的根DOM元素

```javascript

console.log(app.$el); //div#app

```

