## 前言

2021/03/09号去了两家20人左右的小公司面试，笔试和面试都只关注vue水平。所以特别在此准备vue知识

可参考的资料: 

1. https://www.axihe.com/map/vue-focus.html

### （解决）1. vue生命周期是什么？ 各个生命周期的阶段是什么？ 

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

引申问题： 

1. 第一次页面加载会触发哪几个钩子函数
-
答：beforeCreated, create, beforeMounted, mounted

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

### （解决）5. Vue组件的通信方式有哪些？简单说一下

答：

- 父子间通信：
  
  1) 父 -> 子通过props, 子 -> 父 $on $emit

  2) `$parent`,`$children`可获得父子组件实例，访问组件实例中的属性和方法(若存在父子组件)。$children是一个数组，只有需要拿到所有子组件才会使用$children。实际开发中经常使用`$refs`

  ```html
  
  <div id="app">
    <my-blog :title="blogName" ref="myBlog"></my-blog>
    <button @click="getBlogName()">获取博客名</button>
  </div>

  <!-- component my-Blog -->
  <template id="my-blog">
    <h3>{{ title }}</h3>
  </template>
  <script>
    var vm = new Vue({
      el: '#app',
      data() {
        return {
          'blogName': 'Blog'
        }
      },
      methods: {
        getBlogName() {
          console.log(this.$refs.myBlog.name);
        }
      },
      components: {
        'my-blog' : {
          data() {
            return { name: 'msg'}
          },
          template: '#my-blog',
          props: { title: String }
        }
      }
    });
  </script>  
  
  ```

- 兄弟组件通信： 

  1) 子 -> 父，父 -> 子

  2）eventbus

  3) Vuex


  引申: Prop的大小写问题，因为html的Attribute对大小写不敏感，所以在组件中的驼峰式命名的prop转换为等价的短横线分隔命名

  ```javascript

    var componentA = {
    template: `<h1>{{ blogTitle }}</h1>`,
    props: {
      'blogTitle': String // 驼峰式命名blogTitle
    }
  }
  
  ```

  ```html
  
  <my-blog-title blog-title="hello!"></my-blog-title>
  
  ```
  
### （解决）6. v-if和v-show的相同和不同点

  相同点: `v-if`和`v-show`都是条件渲染内容

  不同点： 

  (1) v-if有更高的切换开销，总是销毁和重建节点;

  (2) v-show有更高的启动开销，即使`v-show='false'`也会渲染元素，只是单纯的切换CSS(display="none")

### （解决）7. 路由之间如何跳转，如何传参

  (1) 声明式跳转router-link
      
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


### （解决）9. vue中$route和$router的区别

答: `$route`是"路由参数对象"，包含了name, params, path, query等。而`$router`是路由实例对象，包含了路由跳转，钩子函数等方法

### 10. 绑定key

### （解决）11. 为什么v-for和v-if不能同时使用？

  因为`v-for`的优先级较高，所以连用的话会给每个元素都添加`v-if`,会造成性能问题

### （解决）12. vue.js如何只让style只在当前组件起作用？

答： `<style>` 改成 `<style scoped>` 

### （解决）13. vue.js中keep-alive的作用是什么？

答： 包裹动态组件时，会缓存不活动的组件实例，主要用于保留动态组件状态或重新渲染

### （解决）14. vue.js中使用插件的方法

答: import .. from ...语法, Vue.ues(plugin)使用插件

### （解决）15. vue.js中ajax请求放在哪个生命周期中

答: `mounted`生命周期中DOM已经渲染出来了，可以操作DOM所以放在`mounted`中

### （解决）16. vue.js中v-model的实现原理，如何自定义v-model

答: `v-model`是`value`和`input方法`的语法糖

```html
  <!-- 两者一样 -->
  <input @v-model="msg">
  <input :value="msg" @input="msg=$event.target.value">

```

### 17. vuex是什么，怎么使用， 有哪些功能场景去使用？

vuex是vue项目开发时使用的状态管理工具，用来管理和维护组件间复用的数据。

成员列表有：

(1) state：存放状态

(2) mutation: 对state数据的方法的集合，包括数据的增删改

(3) getters: 过滤state数据

(4) actions: 异步操作数据

(5) moudles：项目复杂时可以让每个模块拥有state,mutation, getters, actions

### 18. 谈谈对MVVM的理解