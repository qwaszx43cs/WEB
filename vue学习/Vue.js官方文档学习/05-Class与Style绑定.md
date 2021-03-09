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

### 数据语法

可以给`v-bind:class`绑定一个数组来应用一个class列表

```html

<div :class="[activeClass, redClass]"></div>

```

代码示例: 

```html

    <div id="app">
        <div :class="[activeClass, redClass]">123</div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                activeClass: 'active',
                redClass: 'red'
            }
        });
    </script>

```

被渲染为`<div :class="active, red">123</div>`


## 绑定行内样式

### 对象语法

`v-bind:style`的对象语法看上去像CSS, CSS的property名可以用驼峰式或短横线分隔,**同样的对象语法常常与计算属性结合使用**

```html

    <div :style="{color: activeColor, border: activeBorder}">   
    <!-- 直接绑定到一个样式对象中，更加直观 -->
    <div :style="myStlyle">123</div> 

```
代码示例: 

```html

    <div id="app">
        <div :style="{color: activeColor, border: activeBorder}">321</div>
        <div :style="myStlyle">123</div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                activeColor: 'red',
                activeBorder: 'solid 1px black',
                myStlyle: {
                    color: 'red',
                    border: 'solid 1px black'
                }
            }
        });
    </script>

```

### 数组语法