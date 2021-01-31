## MVVM模式

MODEL: 负责数据存储

VIEW：负责页面展示

VIEW-MODEL: 负责业务逻辑处理(如ajax请求),对数据处理后返回给视图

## Vue框架

### 特点: 

1. 模板渲染: 基于html的模板语法

2. 响应式的更新机制: 数据有变化，视图自动刷新

3. 渐进式框架 (?)

4. 组件化/模块化

5. 轻量


## 利用vue-cli新建项目

vue-cli 是官方的cli为SPA快速搭建繁杂的脚手架, 只需几分钟就能运行起来带有热重载，保存时lint校验，
以及生产环境可用的构建版本

vue-cli 3.x的初始化方法

```

npm install -g @vue-cli

vue create my-app

cd my-app

npm run serve

```

个人使用的是vue-cli 2.x的初始化方法

```
vue init webpack my-project

cd my-project

npm run dev

```

## vue项目结构分析

- `build`: 打包配置的文件夹

- `config`: webpack相对应的配置

- `src`： 项目开发的源码

    1. App.vue： 入口组件

    2. main.jsL: 项目入口文件

- `static`：存放静态资源文件夹

- `.babelrc`: 解析ES6的配置文件

- `.editorconfig`: 编辑器配置

- `.postcssrc.js`: html添加前缀的配置

- `index.html`: 单页面的入口，通过webpack打包后，会把src代码进行编译，插入到这个html中

- `package.json`: 项目的基础配置，包含项目信息，版本号，脚本命令，项目依赖等