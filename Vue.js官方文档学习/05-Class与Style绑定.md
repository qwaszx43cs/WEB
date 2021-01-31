## 绑定HTML Class

### 对象语法

给v-bind指令传一个对象，class是否存在取决于数据property中的布尔值

```html

    <div id ="app">
        <div :class="{red: isRed,border: isBorder}"></div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                isRed: true,
                isBorder: true
            },
            methods: {}
        });
    </script>

```

v-bind指令绑定class可以和普通的class同时存在

```html

    <div  class="container" :class="{red: isRed,border: isBorder}"></div>

```


绑定的对象不必内联在模板里

```html

    <div id ="app">
        <div :class="myClass"></div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                myClass: {
                    red: true,
                    border: true
                }
            },
            methods: {}
        });
    </script>

```

也可以绑定一个给v-bind绑定一个计算属性

```html

    <div id ="app">
        <div :class="myClass"></div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                isActive: true,
                error: null
            },
            computed: {
                myClass: function() {
                    return {
                        active: this.isActive && !this.error;
                        'text-danger': this.error && this.error.type === 'fatal';
                    }
                }
            }
        });
    </script>


```