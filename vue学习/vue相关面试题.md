## 前言

2021/03/09号去了两家20人左右的小公司面试，笔试和面试都只关注vue水平。所以特别在此准备vue知识

### 1. vue生命周期是什么？ 各个生命周期的阶段是什么？ 

vue组件的创建到销毁的过程是vue生命周期。

vue生命周期共有八个阶段, 每个阶段都对应了一个生命周期函数：

(1) `beforeCreated`

(2) `created`: 可以访问data(), methods属性。但组件还没被挂载到页面中，所以访问不到$el。 `$el`就是视图挂载的目标

(3) `beforeMount`

(4) `mounted`: 可以操作DOM API

(5) `beforeUpdate`

(6) `updated`

(7)  `beforeDestory`

(8) `destoryed`


### 2. 编写Vue组件继承的代码


### 3. 列举全局守卫/路由守卫/组件内守卫的函数


### （解决）4. computed, methods和watch的区别

    computed VS methods

    1. computed是响应式的，methods是非响应式的；

    2. 调用方式不同，computed可以像属性一样访问，method必须以函数形式调用；

    3. computed是响应式依赖缓存的，只有当依赖数据变化或第一次生成计算属性时，才会重新计算。若其依赖没有发生变化，会马上返回结果。而每次重新渲染时，都会调用method方法,若不需要缓存，请使用方法；

    4. computed不支持异步，当computed属性中有异步任务时失效，无法监听异步任务；


    computed VS watch

    1. 当数据变化时进行异步任务时，使用watch

    2. computed是计算一个新的数据挂载到实例上，而watch是监听一个已经挂载到实例上的数据

    3. computed是有响应式缓存的，只有依赖数据或第一次生成计算属性时，才会计算。而watch监听的数据变化就会调用函数；

    ```html

    <body>

    <div id="app">
        <p>message is {{ msg }}</p>
        <p>its reversed message with computed {{ reversedMsg }}</p>
        <p>its reversed message with methods {{ recersedMsg2() }}</p>
    </div>

    <script>
        var vm = new Vue({
        el: '#app',
        data() {
            return {
            msg: 'sky'
            }
        },
        methods: {
            recersedMsg2() {
            console.log('methods');
            return this.msg.split('').reverse().join('');
            }
        },

        computed: {
            reversedMsg: function() {
            console.log('computed');
            return this.msg.split('').reverse().join('');
            }
        }
        })
    </script>
    </body>

    ```

### 5. Vue组件的通信方式有哪些？简单说一下


### （解决）6. v-if和v-show的相同和不同点

    相同点: `v-if`和`v-show`都是条件渲染内容

    不同点： 

    (1) v-if有更高的切换开销，总是销毁和重建节点;

    (2) v-show有更高的启动开销，即使`v-show='false'`也会渲染元素，只是单纯的切换CSS(display="none")

### （解决）7. 路由之间如何跳转，如何传参

    (1) router-link
        
        不带参数
        <router-link :to="{name: '组件名'}"></router-link>
        <router-link :to="{path: '/path'}"></router-link>

        带参数
        <router-link :to="{name: '组件名', params: {id: 1}}">

    (2) 编程式导航 **params传参只能用name**

        ```javascript

        // 不带参数 
        this.$router.push('/path'); 
        this.$router.push({name: '组件名'}); 
        this.$router.push({path: '/path'}); 
        
        // 带参数
        this.$router.push({
            name: '组件名',
            params: {
                id: 1
            }})
        ```


### 8. 写一个axios的详细请求配置(post, get)


### 9. vue中$route和$router的区别


### 10. 绑定key