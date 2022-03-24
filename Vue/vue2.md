# Vue2.0

------

## 第一章：语法

## 第二章：生命周期

![Vue的生命周期](E:\Typora插入图片时保存的图片\Vue的生命周期.png)

## 第三章：组件

> 1. 组件是一个单独功能模块的封装
> 2. 这个模块有属于自己的HTML模板, 也应该有自己的数据data
> 3. 子组件的data属性必须是一个函数, 因为要保证组件的复用会创建不同的数据元, 如果是对象的话就是同一份数据元了

### 3.1 组件使用的步骤

#### 3.1.1 老式的使用步骤

> 1. 创建组件构造器
>
>    ```javascript
>    const YouComponent = Vue.extend({
>        template: `<h3>儿子</h3>`
>    })
>    ```
>
>    1. 要在组件里边使用组件, 还可以为它注册组件
>
>       ```javascript
>       const MyComponent = Vue.extend({
>             template: `
>               <div>
>                 <h2>爸爸</h2>
>                 <you-component></you-component>
>               </div>`,
>             components: {
>               YouComponent
>             }
>       })
>       ```
>
> 2. 注册组件
>
>    1. 全局注册
>
>       ```javascript
>       Vue.component('my-component', MyComponent)
>       ```
>
>    2. 局部注册
>
>       ```javascript
>       const app = new Vue({
>           el: '#app',
>           components: {
>               MyComponent
>           }
>       })
>       ```
>
> 3. 使用组件(组件名写成`-`的形式)
>
>    ```html
>    <div id="app">
>        <my-component></my-component>
>    </div>
>    ```

#### 3.1.2 注册组件语法糖

> 1. 全局注册
>
>    ```javascript
>    Vue.component('MyComponent', {
>        template: `<h2>语法糖</h2>`
>    })
>    ```
>
> 2. 局部注册
>
>    ```javascript
>    const app = new Vue({
>        el: '#app',
>        components: {
>            MyComponent: {
>                template: `<h2>语法糖</h2>`
>            }
>        }
>    })
>    ```
>
> 3. 在内部实际上还是调用了组件构造器`Vue.extend()`

#### 3.1.3 模板分离

> 1. 使用`<script type="text/x-template">`标签分离(不推荐)
>
>    ```javascript
>    <script type="text/x-template" id="mycomponent">
>        <h2>模板分离</h2>
>    </script>
>    ```
>
> 2. 使用`<template>`标签分离(推荐)
>
>    ```javascript
>    <template id="mycomponent">
>        <h2>模板分离</h2>
>    </template>
>    ```
>
> 3. 注册组件
>
>    ```javascript
>    Vue.component('MyComponent', {
>        template: '#mycomponent'
>    })
>    ```

### 3.2 父子组件的通信

#### 3.2.1 概念

> 1. 子组件是不能引用父组件或者Vue实例的数据的
> 2. 父向子：props
> 3. 子向父：this.$emit
> 4. 子组件里不能直接修改父组件的值, 也就是 props只读, 要修改必须返回父组件修改

#### 3.2.2 父向子

> 1. 父组件
>
>    ```javascript
>    <my-component :message="message"></my-component>
>    ```
>
> 2. 子组件
>
>    ```javascript
>    props: ['message']
>    ```
>
> 3. props选项
>
>    1. 选项可以是一个数组
>
>       ```javascript
>       props: ['message']
>       ```
>
>    2. 也可以是一个对象(可以约束类型和设置默认值)
>
>       ```javascript
>       props: {
>           message: {
>               type: String,
>               default: '娜娜米'
>           },
>           arr: {
>               type: Array,
>               default() {
>                   return ['1', '2']
>               }
>           }
>       }
>       ```
>   
>   3. 如果类型是数组或者对象, 设置默认值必须是函数

#### 3.2.3 子向父

> 1. 子组件	
>
>    ```javascript
>    methods: {
>        btnClick(item) {
>            this.$emit('item-click', item)
>        }
>    }
>    ```
>
> 2. 父组件(默认会传递参数, 普通事件默认传递$event)
>
>    ```javascript
>    <my-component @item-click="fatherClick"></my-component>
>       
>    methods: {
>        fatherClick(item) {
>            console.log(item);
>        }
>    }
>    ```

### 3.3 父子组件的访问方式

#### 3.3.1 概念

> 1. 父访子: `$children` 或 `$ref`
> 2. 子访父: `$parent` 
> 3. 访问根组件`$root`

#### 3.3.2 访问方式

> 1. 父访子: `$children` 或 `$ref` 
>
>    1. `children`
>
>       ```javascript
>       methods: {
>           btnClick () {
>               console.log(this.$children[0].showMessage());
>           }
>       }
>       ```
>
>    2. `$ref`
>
>       ```javascript
>       <my-component ref="aaa"></my-component>
>          
>       methods: {
>           btnClick () {
>               console.log(this.$refs.aaa);
>           }
>       }
>       ```
>
> 2. 子访父: `$parent` 
>
>    ```javascript
>    methods: {
>        btnClick () {
>            console.log(this.$parent)
>        }
>    }
>    ```
>
> 3. 访问根组件`$root`
>
>    ```javascript
>    btnClick () {
>        console.log(this.$root);
>    }
>    ```

### 3.4 插槽

#### 3.4.1 概念

> 1. 在子组件中, 使用特殊的元素 `<slot>` 就可以为子组件开启一个插槽
> 2. 该插槽插入了什么内容取决于父组件如何使用

#### 3.4.2 插槽的基本使用

> ```html
> <my-component>
>     <h3>插槽的基本使用</h3>
> </my-component>
> 
> <template id="mycomponent">
>     <div>
>         <h2>插槽的使用</h2>
>         <slot><button>默认值</button></slot>
>     </div>
> </template>
> ```

#### 3.4.3 具名插槽

> 1. 有时候需要使用多个插槽, 就需要给插槽命名
>
>    ```html
>    <my-component>
>        <h3 slot="bottom">看下</h3>
>        <h4 slot="on">看上</h4>
>    </my-component>
>       
>    <template id="mycomponent">
>        <div>
>            <slot name="on"></slot>
>            <h2>插槽的使用</h2>
>            <slot name="bottom"></slot>
>        </div>
>    </template>
>    ```

#### 3.4.4 作用域插槽

> 1. 父组件替换插槽的标签, 但是内容由子组件来提供
>
>    ```html
>    <my-component>
>        <div slot-scope="slot">
>            <span v-for="item in slot.data">{{item}} - </span>
>        </div>
>    </my-component>
>       
>    <template id="mycomponent">
>        <div>
>            <h2>作用域插槽</h2>
>            <slot :data="arr"></slot>
>        </div>
>    </template>
>    ```
>
>    ```javascript
>    const app = new Vue({
>        el: '#app',
>        components: {
>            MyComponent: {
>                template: '#mycomponent',
>                data () {
>                    return {
>                        arr: ['JS', 'Vue', 'React', 'Node.js', 'CSS']
>                    }
>                }
>            }
>        }
>    })
>    ```

## 第四章：Vue CLI

### 4.1 安装 Vue 脚手架

> 1. 安装：`npm i @vue/cli -g`
> 2. 查看版本：`vue -V`

### 4.2 runtime-compiler与runtime-only

#### 4.2.1 Vue程序运行过程

> 1. runtime-compiler
>    - template --> ast(抽象语法树) --> render --> VDOM(虚拟DOM) --> UI
> 2. runtime-only
>    - render --> VDOM --> UI

<img src="E:\Typora插入图片时保存的图片\Snnn.png" alt="Snnn" style="zoom: 80%;" />

#### 4.2.2 render 函数的使用

> 1. runtime-compiler(最原始的)
>
>    ```javascript
>    new Vue({
>        el: '#app',
>        components: {App},
>        template: '<App/>'
>    })
>    ```
>
> 2. runtime-only
>
>    ```javascript
>    new Vue({
>        render: h => h(App)
>    }).$mount('#app')
>    ```
>
> 3. 当中的 `h` 其实是 `createElement`, 有两种使用方法
>
>    1. 第一种：可以接收三个参数, `createElement('标签', {相关属性对象(可以不要)}, [内容数组(可以不要)])`
>
>       ```javascript
>       new Vue({
>           el: '#app',
>           render: createElement => { // 就是创建元素
>               return createElement('div', {class: 'box'}, ['hello', createElement('h2', ['Vue'])])
>           }
>       })
>       ```
>
>    2. 第二种：传入一个组件对象
>
>       ```javascript
>       new Vue({
>           el: '#app',
>           render: createElement => {
>               return createElement(Component)
>           }
>       })
>       ```
>
> 4. 效果是一样的, 但是 runtime-only 效率更高, 省略了两环节

### 4.3 创建项目

> 1. `vue create test`
> 2. 空格是确认与取消, 回车是下一步, 箭头是上下
> 3. 要修改默认配置的话, 在项目的根目录下添加 `vue.config.js` 文件

## 第五章：前端路由

### 5.1 路由概念

#### 5.1.1 什么是路由

> 1. 一个路由就是一个映射关系(key-value)
> 2. key为路径, value可能是function或component

#### 5.1.2 路由分类

> 1. 前端路由
>    1. 核心：改变URL，但是页面不进行整体的刷新
>    2. value是component，用于展示页面内容
>    3. 当浏览器的path变为/test时, 当前路由组件就会变为Test组件
> 2. 后端路由
>    1. value是function, 用来处理客户端提交的请求
>    2. 当node接收到一个请求时, 根据请求路径找到匹配的路由, 调用路由中的函数来处理请求, 返回响应数据

### 5.2 vue-router

#### 5.2.1 介绍

> 1. vue-router是基于路由和组件的Vue.js官方路由插件
> 2. 路由用于设定访问路径, 将路径和组件映射起来
> 3. 在vue-router的单页面应用中, 页面的路径的改变就是组件的切换

#### 5.2.2 安装和使用vue-router

> 1. 安装：`npm i vue-router -S`
> 2. 引入路由类：`import VueRouter from 'vue-router'`
> 3. Vue中安装路由功能：`Vue.use(VueRouter)`
> 4. 创建路由实例：`const router = new VueRouter({routes})`
> 5. 配置路由映射：`const routes = [{path: '/', component: '某组件'}]`
> 6. 模块导出：`export default router`
> 7. `main.js`中引入对象：`import router from '@/router'`
> 8. 将路由对象挂载到Vue实例：`new Vue({router})
> 9. `App.vue`中通过`<router-link>` `<router-view>` 使用路由

```javascript
// router/index.js
import Vue from 'vue'
import VueRouter from 'vue-router' // 引入路由类

import Home from '@/components/Home' // 引入组件
import About from '@/components/About'

Vue.use(VueRouter) // Vue中安装路由功能

const routes = [ // 配置映射
  {
    path: '/home',
    component: Home
  },
  {
    path: '/about',
    component: About
  }
]

export default new VueRouter({ // 创建路由实例
  routes
})
```

```javascript
// main.js
import Vue from 'vue'
import App from './App.vue'
import router from './router' // 引入路由实例

Vue.config.productionTip = false

new Vue({
  router, // 挂载到Vue实例
  render: h => h(App),
}).$mount('#app')
```

```vue
// App.vue
<template>
  <div id="app">
    <router-link to="/home">首页</router-link> <!-- 相当于 a 标签-->
    <router-link to="/about">关于</router-link>
    <router-view></router-view> <!-- 渲染位置-->
  </div>
</template>
```

#### 5.2.3 路由的hash模式与history模式

> 1. Vue 默认使用的是 hash 模式
>
> 2. 如果要修改成 history 模式, 创建路由实例的时候声明即可
>
>    ```javascript
>    export default new VueRouter({ // 创建路由实例
>      routes,
>      mode: 'history'
>    })
>    ```

#### 5.2.4 路由的默认路径

> 1. 用 `redirect` 重定向
>
>    ```javascript
>    const routes = [
>      {
>        path: '/',
>        redirect: '/about'
>      }
>    ]
>    ```

#### 5.2.5  router-link 的属性

> 1. `to`: 指定跳转的路径, 可以是一个对象, 包括path和query
> 2. `tag`: 可以指定`<router-link>`之后渲染成什么组件, 默认是`<a>`标签
> 3. `replace`: replace不会留下history记录, 所以指定replace的情况下, 后退键返回不能返回到上一个页面中
> 4. `active-class`: 当`<router-link>`对应的路由匹配成功时, 会自动给当前元素设置一个`router-link-active`的class, 设置`active-class`可以修改默认的名称, 也可以在router实例里修改

```vue
<router-link to="/home" replace tag="button" active-class="active">首页</router-link> 
```

```javascript
export default new VueRouter({
  routes,
  linkActiveClass: 'active' // 修改所有的默认active
})
```

### 5.3 路由代码跳转

> 1. 当不使用`<router-link>`时, 也可以实现路由跳转
>
> 2. `this.$router.push()`
>
> 3. `this.$router.replace()`
>
>    ```vue
>    <button @click="homeClick">首页</button> |
>    <button @click="aboutClick">关于</button>
>    <router-view></router-view>
>    ```
>
>    ```javascript
>    methods: {
>        homeClick () {
>            this.$router.push('/home')
>        },
>        aboutClick() {
>            this.$router.push('/about')
>        }
>    }
>    ```

### 5.4 动态路由

> 1. 在某些情况下, 一个页面的path路径可能是不确定的, 比如我们进入用户界面时, 希望后面加上用户的ID --> /user/001 | /user/002
>
> 2. 这种path和Component的匹配关系, 我们称之为动态路由
>
> 3. 这也是路由传递数据的一种方式
>
> 4. `$route`: 哪个路由处于活跃状态就代表谁
>
> 5. 代码实现
>
>    ```javascript
>    // router/index.js中的routes
>    {
>        path: '/user/:userName', // 用 ` : ` 传递数据
>        component: User
>    }
>    ```
>
>    ```vue
>    <router-link :to="'/user/' + userName">我的</router-link>
>    <h3>{{$route.params.userName}}</h3>
>    ```

### 5.5 路由的懒加载

#### 5.5.1 介绍

> 1. 当使用 Webpack 打包构建应用时, JavaScript 包会变得非常大, 影响页面加载
> 2. 路由懒加载的主要作用就是将路由对应的组件打包成一个个的js代码块
> 3. 只有在这个路由被访问到的时候, 才加载对应的组件

![image-20210307080424368](E:\Typora插入图片时保存的图片\image-20210307080424368.png)

#### 5.5.2 懒加载的使用方式

```javascript
{
    path: '/home',
    component: () => import('@/components/Home')
}
```

### 5.6 嵌套路由

#### 5.6.1 介绍

> 1. 比如在 About 页面中, 我们希望通过/about/lemon 和 /about/egg 访问一些内容,
> 2. 这就需使用嵌套路由, 也就是路由里边还有路由
> 3. 先配置子路由(通过 children 属性), 再在路由组件里边使用 `<router-link` 与 `router-view`

#### 5.6.2 嵌套路由的使用方式

```javascript
// router/index.js 中的 routes 里边的 about
{
    path: '/about',
    component: () => import('@/components/About'),
    children: [ // 通过 children 配置子路由, 也是一个数组
      {
        path: '/', // 子路由的默认路径重定向
        redirect: 'lemon'
      },
      {
        path: 'lemon', // 子路由前边的 '/' 可以不写
        component: () => import('@/components/Lemon')
      },
      {
        path: 'egg',
        component: () => import('@/components/Egg')
      }
    ]
}
```

```vue
// About 组件
<template>
  <div id="about">
    <h2>我是About</h2>
    <router-link to="/about/lemon">Lemon</router-link> |
    <router-link to="/about/egg">Egg</router-link>
    <router-view></router-view>
  </div>
</template>
```

### 5.7 路由组件传递参数

#### 5.7.1 介绍

> 1. 路由组件传递参数主要有两种类型: params 和 query

#### 5.7.2 params 类型

> 1. 配置路由格式: /router/:id
>
> 2. 传递的方式: 在path后面跟上对应的值
>
> 3. 传递后形成的路径: /router/123, /router/lemon
>
> 4. 通过`<router-link>`实现
>
>    1. 配置路由
>
>       ```javascript
>       {
>           path: '/user/:userName',
>           component: () => import('@/components/User')
>       }
>       ```
>
>    2. 传递参数的方式
>
>       ```vue
>       <router-link :to="'/user/' + userName">我的</router-link>
>       ```
>
>    3. 在标签里边儿获取参数(在App组件和这个路由组件都能使用)
>
>       ```vue
>       <h3>{{$route.params.userName}}</h3>
>       ```
>
> 5. 通过JS代码实现
>
>    1. 传递参数方式
>
>       ```vue
>       <button @click="userClick">我的</button>
>       ```
>
>       ```javascript
>       userClick() {
>             this.$router.push('/user/' + this.userName)
>       }
>       ```
>
>    2. 在JS里边儿获取参数
>
>       ```javascript
>       this.$route.params.userName
>       ```

#### 5.7.3 query 类型

> 1. 配置路由格式: /router, 也就是普通配置
>
> 2. 传递的方式: 对象中使用query的key作为传递方式
>
> 3. 传递后形成的路径: /router?id=123, /router?id=lemon
>
> 4. 通过`<router-link>`实现
>
>    1. 配置路由(普通配置)
>
>       ```javascript
>       {
>           path: '/home',
>           component: () => import('@/components/Home')
>       }
>       ```
>
>    2. 传递参数的方式(to是一个对象)
>
>       ```vue
>       <router-link :to="{path: '/home', query: {userName}}">首页</router-link>
>       ```
>
>    3. 在标签里边儿获取参数(在App组件和这个路由组件都能使用)
>
>       ```vue
>       <h3>{{$route.query.userName}}</h3>
>       ```
>
> 5. 通过JS代码实现
>
>    1. 传递参数方式
>
>       ```javascript
>       homeClick () {
>           this.$router.push({
>               path: '/home',
>               query: {
>                   userName: this.userName
>               }
>           })
>       }
>       ```
>
>    2. 在JS里边儿获取参数
>
>       ```javascript
>       this.$route.query.userName
>       ```

### 5.8 $router 与 $route 的区别

> 1. 所有的组件都继承自 Vue 的原型
> 2. $router为VueRouter实例，想要导航到不同URL，则使用`$router.push`方法
> 3. $route为当前router跳转对象(活跃对象) 

### 5.9 导航守卫

#### 5.9.1 介绍

> 1. vue-router 提供的导航守卫主要用来监听监听路由的进入和离开的
> 2. vue-router提供了 `beforeEach `和 `afterEach ` (全局导航守卫)钩子函数, 它们会在路由即将改变前和改变后触发
> 3. `beforeEach((to, from, next) => {next()})`  -->  next必须有, 要不然不会跳
> 4. `afterEach((to, from) => {})` --> 这个因为是跳转后触发, 所以没有 next

#### 5.9.2 修改网页标题

> 1. 在路由配置中加入数据元 meta 属性
>
>    ```javascript
>    {
>        path: '/home',
>            meta: {
>                title: '首页'
>            },
>                component: () => import('@/components/Home')
>    }
>    ```
>
> 2. 利用全局导航守卫
>
>    ```javascript
>    router.beforeEach((to, from, next) => {
>      document.title = to.matched[0].meta.title // matched 为第一个匹配的路由
>      next()
>    })
>    ```
>
>    <img src="E:\Typora插入图片时保存的图片\image-20210307104110864.png" alt="image-20210307104110864"  />

#### 5.9.3 局部守卫

> 1. 在路由配置的时候可以加入局部守卫, 参数和全局守卫一样的
>
> 2. `beforeEnter: (to, from, next) => {}`
>
>    ```javascript
>    {
>        path: '/home',
>            meta: {
>                title: '首页'
>            },
>            beforeEnter: (to, from, next) => {
>                // ...
>            },
>            component: () => import('@/components/Home')
>    }
>    ```

### 5.10 keep-alive

#### 5.10.1 介绍

> 1. keep-alive 是 Vue 内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染, 可配合created(生命周期函数)检测, 它有两个非常重要的属性
>    1. include - 字符串或正则表达，只有匹配的组件会被缓存, 值为组件的name
>    2. exclude - 字符串或正则表达式，任何匹配的组件都不会被缓存, 值为组件的name
> 2. router-view 也是一个组件，如果直接被包在 keep-alive 里面，所有路径匹配到的视图组件都会被缓存
> 3. 包裹keep-alive后, 组件就拥有了两个函数
>    1. `activated () {}` 不活跃 --> 活跃 时触发
>    2. `deactivated() {}` 活跃 --> 不活跃 时触发
> 4. 常用：`beforeRouteLeave (to, from, next) {}` 路由离开触发

#### 5.10.2 保存路由离开时的状态

```vue
<keep-alive>
    <router-view></router-view>
</keep-alive>
```

```javascript
activated() {
    this.$router.push(this.path) // 活跃时跳转子路由
},
beforeRouteLeave (to, from, next) {
    this.path = this.$route.path // 保存离开时的状态
    next()
}
```

## 第六章：Vuex

### 6.1 Vuex介绍

> 1. Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式
> 2. Vuex 就是为了提供这样一个在多个组件间共享状态的插件
> 3. 相当于全局变量, 但是响应式的

![image-20210307130915346](E:\Typora插入图片时保存的图片\image-20210307130915346.png)

### 6.2 Vuex 的使用

> 1. 安装：`npm i vuex -S`
> 2. 引入Vuex：`import Vuex from 'vuex'`
> 3. Vue中安装Vuex功能：`Vue.use(Vuex)`
> 4. 创建store：`const store = new Vuex.Store({})`
> 5. 模块导出：`export default store
> 6. `main.js`中引入对象：`import router from '@/store'`
> 7. 将Store对象挂载到Vue实例：`new Vue({store})
> 8. 使用：`$store.xxx`

```javascript
// store/index.js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
  state: {},
  mutations: {},
  actions: {},
  getters: {},
  modules: {}
})

export default store
```

```javascript
// main.js
import Vue from 'vue'
import App from './App.vue'
import store from './store'

Vue.config.productionTip = false

new Vue({
  store,
  render: h => h(App),
}).$mount('#app')
```

### 6.3 Vuex 核心概念

> 1. State
> 2. Getter
> 3. Mutations
> 4. Actions
> 5. Modules

#### 6.3.1 State

> 1. State: 单一状态树
> 2. 就是用这一个对象管理数据就可以了, 不要弄很多个

#### 6.3.2 Getter

> 1. 有点类似计算属性
>
> 2. 从store中获取一些state变异后的状态, 比如平方
>
> 3. 默认接收一个参数 state, 也只有这个
>
>    ```javascript
>    getters: {
>        countPower (state) {
>            return state.count * state.count
>        }
>    }
>    
>    // <h3>{{$store.getters.countPower}}</h3>
>    ```
>
> 4. getters默认是不能传递其他参数的, 如果希望传递参数, 那么只能让getters本身返回另一个函数, 柯里化
>
>    ```javascript
>    getters: {
>        countPower (state) {
>            return (n) => state.count * n
>        }
>    }
>       
>    // <h3>{{$store.getters.countPower(3)}}</h3>
>    ```

#### 6.3.3 Mutation

> 1. Vuex的store状态的更新唯一方式：提交Mutation
>
> 2. Mutation主要包括两部分
>
>    1. 字符串的事件类型(type)
>    2. 一个回调函数(handle) ,该回调函数的第一个参数就是state
>
> 3. 定义Mutation
>
>    ```javascript
>    mutations: {
>        increment (state) {
>            state.count++
>        }
>    }
>    ```
>
> 4. 提交Mutation
>
>    ```javascript
>    addition () {
>        this.$store.commit('increment')
>    }
>    ```
>
> 5.  Mutation传递参数
>
>    ```javascript
>    increment (state, num) {
>        state.count += num
>    }
>    
>    // 提交
>    addition () {
>        this.$store.commit('increment', 10)
>    }
>    ```
>
> 6. 另外一种传递参数的风格(payload)(适用多个参数)
>
>    ```javascript
>    increment (state, payload) {
>        state.count = state.count + payload.num1 + payload.num2
>    }
>    
>    // 提交
>    addition () {
>        this.$store.commit({
>            type: 'increment',
>            num1: 10,
>            num2: 25
>        })
>    }
>    ```
>
> 7. Mutation响应原则
>
>    1. Vuex的store中的state是响应式的, 当state中的数据发生改变时, Vue组件会自动更新
>    2. 这就要求我们必须遵守一些Vuex对应的规则
>       1. 提前在store中初始化好所需的属性
>       2. 当给state中的对象添加新属性时, 使用下面的方式
>          1. 使用`Vue.set(obj, 'key', value)`
>          2. 用新对象给旧对象重新赋值
>
> 8. Vuex要求我们Mutation中的方法必须是同步方法
>
>    1. 主要的原因是当我们使用devtools时, 可以devtools可以帮助我们捕捉mutation的快照
>    2. 但是如果是异步操作, 那么devtools将不能很好的追踪这个操作什么时候会被完成

#### 6.3.4 Action

> 1. 我们强调, 不要再Mutation中进行异步操作
>
> 2. 但是某些情况, 我们确实希望在Vuex中进行一些异步操作, 比如网络请求
>
> 3. Action类似于Mutation, 但是是用来代替Mutation进行异步操作的
>
> 4. context是和store对象具有相同方法和属性的对象, 但是它们并不是同一个对象
>
> 5. 也可以接收参数, 一般都是封装成返回一个 Promise 对象
>
> 6. 在actions里边提交mutation：`context.commit('xx', payload)`
>
> 7. 通过 `this.$store.dispatch('xx', payload)` 分发任务
>
>    ```javascript
>    actions: {
>        countPowerAsync(context, payload) {
>            return new Promise((resolve, reject) => {
>                setTimeout(() => {
>                    context.commit('increment', payload)
>                    resolve()
>                }, 2000)
>            })
>        }
>    }
>    
>    addition () {
>        this.$store.dispatch('countPowerAsync', {
>            num1: 10,
>            num2: 25
>        }).then(() => {
>            console.log('OK');
>        })
>    }
>    ```
>
> 8. 然后就可以使用then方法

#### 6.3.5 Module

> 1. Vue使用单一状态树,那么也意味着很多状态都会交给Vuex来管理
> 2. 当应用变得非常复杂时,store对象就有可能变得相当臃肿
> 3. 为了解决这个问题, Vuex允许我们将store分割成模块(Module), 而每个模块拥有自己的state、mutation、action、getters等

```javascript
const moduleA = {
    state: {},
    mutations: {}
}

const moduleB = {
    state: {},
    mutations: {}
}

const store = new Vuex.Store({
    a: moduleA,
    b: moduleB
})

store.state.a // moduleA 的状态
store.state.a // moduleB 的状态
```

### 6.4 项目目录

> 1. 避免臃肿, 可以把state, mutation等分离成单独的js文件

