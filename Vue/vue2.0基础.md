### 1.学习vue前的一些知识点

​	2.定义一个变量叫message

​	3.将message变量放在前面的div元素中显示s

​	4.修改message的数据

​	5.需要将修改后的数据再次替换到div元素中

#### 2).vue的做法（编程范式：声明式编程）

1.可以真正做到数据与界面	分离（数据都写在data里面就行，处理	界面在标签里面就可以）

2.修改数据，不用操作界面，界面会自动改变。（响应式）

### 2.创建Vue实例传入的options

#### el：

类型：string|HTMLElement 

作用：决定之后Vue实例会管理哪一个DOM

也可以用

`v.$mount('#root')`挂载实例     这种写法更灵活

例如：

```js
// 1秒之后再挂载元素
setTimeout(() => {
	v.$mount('#root')
},1000)
```



#### data：

类型：Object|Function

作用：Vue实例对应的数据对象

#### methods：

类型：{ [key:string]:Function }

作用：定义属于Vue的一些方法，可以在其他的地方调用，也可以在指令中使用。

#### 生命周期函数

#### Mustache

mustache语法（双大括号）

### 4.Vue的指令

#### 1.v-text

mustache 比这个好使，更灵活

#### 2.v-on

语法糖：@

#### v-on的修饰符

- .stop
- .prevent
- .capture
- .self
- .once
- .passive

```js
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即内部元素触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>

<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>

使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。
因此用`v-on:click.prevent.self`会阻止所有的点击，
而`v-on:click.self.prevent`只会阻止对元素自身的点击


<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div v-on:scroll.passive="onScroll">...</div>
```



@_.stop 处理停止冒泡  如在嵌套的事件会被停止 

![image-20210324230838394](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210324230838394.png)

​	.prevent 阻止默认事件

​	.keyCode|keyAlias(键盘的对应编码值)	只是当事件从特定键触发时才触发回调

![image-20210324231232247](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210324231232247.png)

.once  只触发一次回调

.native	多次想要在一个组件上监听组件根元素的原生事件

注意！！！
如果用了封装组件的话，比如element，这个时候使用按键修饰符需要加上.native

> <el-input v-model="account" placeholder="请输入账号" @keyup.enter.native="search()"></el-input>

**.sync修饰符**

> .sync修饰符可以实现子组件与父组件的双向绑定，并且可以实现子组件同步修改父组件的值。
>
> 一般情况下，想要实现父子组件间值的传递，通常使用的是 props 和自定义事件 $emit 。
> 其中，父组件通过 props 将值传给子组件，子组件再通过 $emit 将值传给父组件，父组件通过事件j监听获取子组件传过来的值。
> 如果想要简化这里的代码，可以使用.sync修饰符，实际上就是一个语法糖。

#### 3.v-show

#### 4.v-bind

##### 1.动态绑定基本属性

如动态绑定a元素的href属性

如动态绑定img元素的src属性

##### 2.动态绑定style

可以利用v-bind：style来绑定一些CSS内联样式。

再写CSS属性名时，如 font-size

​	我们可以使用驼峰式  fontSize  或短横线分隔 ‘font-size’

##### 3.绑定class有两种方式

​	对象语法和数组语法

#### 5.v-for

#### 6.v-if

#### 6.v-show 和 v-if的对比

**v-if 和 v-show都可以决定一个元素是否渲染，那么开发中如何选择呢？**

​	**v-if**当条件为flase时，压根不会有对应的元素在DOM中。

​	**v-show**当条件为flase时，仅仅将元素的display属性设置为none而已。

**开发中如何选择？**

​	当需要在显示与隐藏之间切片很频繁时，使用v-show

​	当只有一次切换时，通过使用v-if

#### 7.v-model

##### 1.基础

v-model其实是一个语法糖，它的背后本质是包含两个操作：

1. v-bind绑定一个value属性
2. v-on指令给当前元素绑定input事件 

`也就是说下面的代码：等同于下面的代码`

![image-20210330161504849](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210330161504849.png)

实现表单元素和数据的双向绑定

v-model和radio的结合使用

v-model和checkbox的结合使用

v-model和select的结合使用

##### 2.input中的值绑定

利用v-bind 实现

##### 3.v-model修饰符的使用

- lazy修饰符：

  > 默认情况下，v-model默认是在input事件中同步输入框的数据的

  > 也就是说，一旦有数据发生改变对应的data中的数据就会自动发生改变	

  > lazy修饰符可以让数据在失去焦点或回车时才更新

- number修饰符：

  > 默认情况下，在输入框中无论是输入字母还是数字，都会被当作String类型进行处理

  > 但是如果我么希望处理的是数字类型，那么最好直接将内容当作数字处理

  > number修饰符可以让在输入框中输入的内容自动转成数字类型

- trim修饰符：

  > 如果输入的内容首尾有很多空格，trim修饰符可以过滤左右两边的空格

#### 8.v-once

1.该指令后面不需要跟任何表达式

2。该指令表示元素和组件只渲染一次，不会随着数据的改变而改变。

#### 9.v-html

1.可以解析HTML格式的代码，例如在data中可以直接写a标签

#### 10.v-pre

用于跳过这个元素和他子元素的编译过程，用于显示原本的mustache语法。{{message}}不会解析，直接输出{{message}}

#### 11.v-cloak

在某些情况下，我们浏览器可能会直接显示出未编译的mustache，可能vue解析之前浏览器就直接显示了，就会显示出代码，利用cloak可以隐藏 不让用户看到。 了解

cloak：斗篷

![image-20210323171014069](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210323171014069.png)



### 5.数组的响应式方法

#### 1）push方法

```
this.letters.push('aaa')
```

#### 2) pop()	删除数组中的最后一个元素

```
this.letter.pop();
```

#### 3） shift() 删除数组的第一个元素

```
this.letters.shift();
```

#### 4) unshift() 	在数组最前面添加元素

```
this.letters.unshift('aaa')
```

#### 5) splice()	

作用：删除元素/插入/替换

删除元素：第二个参数传入你需要删除几个元素

替换元素：第二个参数，表示我们要替换几个元素，后面是用于替换前面的元素。

插入元素：第二个参数，传入0，并且后面跟上要插入的元素。

splice(start)

```
this.splice(1,3,'m','n')	//从第一个元素起，删除后面三个元素，再加入'm','n'
```

#### 6) sort() 排序

#### 7）reverse() 反转

```
this.letters.reverse()
```

(...item:T[]) 可变参数，可以传入多个值

#### 8) 注意

```
this.letters[0] = 'bbbb'
```

通过索引值修改数组中的元素，vue没有通过此方法监听，来改变数组，不会刷新界面，不会渲染dom

响应式方法一：

```
this.letters.splice(0,1,'bbbb')
```

方法二：vue内部实现的方式

set(要修改的对象，索引值，修改后的值)

```
Vue.set(this.letters,0,'bbbb')
```

### 6.计算属性  computed

**基础例子**

```js
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
```

```js
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})
```

```js
结果：

Original message: "Hello"

Computed reversed message: "olleH"
```

这里我们声明了一个计算属性 `reversedMessage`。我们提供的函数将用作 property `vm.reversedMessage` 的 getter 函数：

```js
console.log(vm.reversedMessage) // => 'olleH'
vm.message = 'Goodbye'
console.log(vm.reversedMessage) // => 'eybdooG'
```



> 与methods相比computed节省性能，自带缓存功能，在被调用时只被调用一次，而methods用几次就调用几次。

> 计算属性是基于响应式依赖进行缓存的,只有在相关响应式依赖发生改变时，它们才会重新求值，这就意味着只要`message`还没有发生改变，多次访问`reversedMessage`计算属性会立即返回之前的计算结果，再不必再次执行函数

```js
console.log(vm.reversedMessage) // => 'olleH'
vm.message = 'Goodbye'
console.log(vm.reversedMessage) // => 'eybdooG'
```



**计算属性VS侦听属性**

> 侦听属性：用来观察和响应Vue实例上的数据变动。
>
> 通常更好的做法是使用计算属性而不是命令式的`watch`回调

但是当需要在数据变化时执行异步或是开销比较大的操作时，Vue通过watch选项提供一个更通用的方法。

#### 1）计算属性的setter和getter

![image-20210324100648035](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210324100648035.png)



### .高阶函数的使用filter/map/reduce

高阶函数：函数本身需要的参数 可能也是一个函数

编程范式：命令式编程/声明式编程

编程范式：面向对象编程(第一公民：对象)/函数式编程(第一公民：函数)

#### 1.filter函数的使用

filter 中的回调函数有一个要求：必须返回一个Boolean值

true：当返回true时，函数内部会自动将这次回调的n加入到新数组中

false:当返回false时，函数内部会过滤掉这次的n

//计算<100的值

![image-20210330150743185](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210330150743185.png)

#### 2.map函数的使用

![image-20210330151200551](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210330151200551.png)

#### 3.reduce函数的使用

reduce函数需要两个参数

作用：reduce作用对数组中所有的内容进行汇总

![image-20210330153350563](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210330153350563.png)

#### 4.使用高阶函数式编程

![image-20210330153533127](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210330153533127.png)

**使用箭头函数**

![image-20210330153755854](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210330153755854.png)

### 8.组件

#### 1.vue组件化思想

组件化是vue.js中重要思想

任何的应用都会被抽象成一颗组件树

![image-20210330172411231](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210330172411231.png)

#### 2.注册组件的基本步骤

1. 创建组件构造器
2. 注册组件
3. 使用组件

![image-20210330173101014](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210330173101014.png)

![image-20210330181757013](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210330181757013.png)

#### 3.组件的语法糖

主要是省去了调用Vue.extend()的步骤，而是可以直接使用一个对象来代替

注册组件的语法糖

```
Vue.component('cpn1',{
	template:`
        <div>
          <h2>我是标题</h2>
          <p>我是内容，哈哈哈</P>
        </div>`
})
```

![image-20210330182506922](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210330182506922.png)

#### 4.组件模板的分离写法

还有一个地方的写法比较麻烦，就是template模块写法

Vue提供了两种方案来定义HTML模块内容：

- 使用<script>标签

- 使用<template>标签

  ![image-20210330185156537](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210330185156537.png)

#### 5.组件可以访问Vue实例数据吗？

组件是一个单独功能模块的封装：

> 这个模块有属于自己的HTML模板，也应该有属性自己的数据data

----组件不能直接访问Vue实例中的data

> Vue组件有自己保存数据的地方
>
> 并且组件**data必须是函数，而且这个函数返回一个对象，对象内部保存着数据**

**为什么组件data必须是函数？**

------因为在每一次调用data中的对象时，返回的是不同的对象，在调用时对象的内存地址是不同的。如果data不是函数的话，相互之间会造成影响，会造成数据污染。

（函数才有作用域）

#### 6.父子组件间的通信

如何进行父子组件的通信呢？

- > 通过props向子组件传递数据

- > 通过事件向父组件发送消息

![image-20210331092722719](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210331092722719.png)

直接将Vue实例当作父组件，并且其中包含子组件来简化代码。

**props**

视频 64懵

**父子组件的访问方式**

> 父组件访问子组件：使用$children（不常用） 或 $refs	常用 	(reference引用)

> 子组件访问父组件：使用$parent
>
> 子访问根组件:$root

### 9.插槽slot

#### 1）为什么使用插槽

**组件的插槽：**

> 组件的插槽是为了让封装的组件具有扩展性
>
> 预留位置，让使用者决定组件内部的一些内容到底展示什么

#### 2）具名插槽

有时候需要使用多个插槽，就需要给插槽命名

![image-20210331151143974](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210331151143974.png)

#### 3）作用域插槽

> 父组件替换插槽的标签，但是内容是由子组件来提供的

### 10.前端模块化开发	

#### 1）ES6模块化的导入与导出

导出方式一：导入{ }中定义的变量

> ```
> export{
> 	flag,sum
> }
> ```

导出方式二：直接导入export定义的变量

> ```
> export var num1 = 1000;
> export var height = 1.88
> ```

导出方式三：导出函数/类

> ```
> export function name(num1,num2){
> 	return num1 +num2
> }
> ```

导出方式四：export defalut

> 在某些情况下，一个模块中包含某一个功能，我们不希望给这个功能命名，而是让导入者自己来命名

另外，需注意：

export defalut 在同一个模块中，不允许同时存在多个

全部导入

```
import *
```

### 11.Webpack

#### 1)什么是Webpack

> 从本质来讲，webpack是一个现代的JavaScript应用的静态模块打包工具

前端模块化：

> webpack其中一个核心就是让我们可能进行模块化开发，并且会帮助我们处理模块间的依赖关系

> 不仅仅是JavaScript文件，css、图片、json文件等，在webpack中都可以被当作模块来使用

### 12.CLI

#### 1)什么是Vue CLI

command-line interface	俗称脚手架

> 使用vue-cli可以快速搭建Vue开发环境以及对应的webpack配置

#### 2）vue cli的使用

​	**vue cli 初始化项目**

旧版本

> ```
> vue init webpack my-project
> ```

新版本

>```
>vue create my-project
>```

**npm run build**

![image-20210401112338510](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210401112338510.png)

npm run dev

![image-20210401112412688](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210401112412688.png)

### 13.路由

#### 1）序言

前端渲染：

> 浏览器中显示的网页中的大部分内容，都是有前端写的js代码在浏览器中执行，最终渲染出来的网页。

后端渲染：

前后端分离阶段：

> ajax的出现
>
> 后端专注于数据上，前端专注于交互和可视化上

后端路由：

> 后端处理URL和页面之间的映射关系

SPA页面（单页面富应用）

> 整个网页只有一个html页面
>
> 主要特点：在前后端分离的基础上加了一层前端路由
>
> 也就是前端来维护一套路由规则

前端路由

> 核心：改变URL时，页面不进行整体刷新

#### 2)vue router 安装和使用

1.通过Vue.use(插件)，安装插件

```
Vue.use(VueRouter)
```

2.创建VueRouter对象

**注意**：这里只能是routes（随便定义其他好像都不行）

```
const routes = [
 path:'/',
 component:
]
const router = new VueRouter({
	//配置路由和组件之间的应用关系
	routes
})
```

3.将router对象传入到vue实例中

```
 export default router
```

#### 3）路由的默认路径

默认情况下，进入网站首页，我们希望<router-view>渲染首页内容

只需多配置一个映射即可

```javascript
const routes = [
	{
		path:'/',		//配置的是根路径
		redirece:'/home'	//将根路径重定向到home路径下
	},
]
```

#### 4）router-link与router-view补充

要想显示其他页面组件，就必须再**app.vue**文件中添加 `router-link`

如果不想要类似a标签的形式，可以利用==tag==属性

```
 <router-link to="/home" tag="button">首页</router-link>
```

==replace==:replace 不会留下history记录，所以指定replace的情况下，后退键返回不能回到上一个页面

==active-class==：当<router-link>对应的路由匹配成功时，会自动给当前元素设置一个router-link-active的class，设置active-class可以修改默认的名称。

用的时候直接再style中添加样式 

```
.router-linke-active {
	color:red
}
//或者
//在路由的index文件中添加linkActiveClass属性,在前端的vue界面中添加active样式，<router-link active-class="active"></router-link>
const router = new VueRouter({
	routes,
	mode:'history'
	linkActiveClass:'active'
})
```



`router-view`相当于一个占位的作用，决定组件显示的位置

#### 5）通过代码跳转路由

1.当不使用<router-link>时，也可以实现路由跳转

2.有返回的跳转：

```
this.$router.push()
```

3.没有返回的跳转

```
this.$router.replace()
```

```
<button @click="homeClick">首页</button> |
<button @click="aboutClick">关于</button>
<router-view></router-view>
```

```javascript
methods:{
	homeClick(){
		this.$router.push('/home')
	},
	aboutClick(){
		this.$router.replace()
	}
}
```

#### 6)动态路由

若我们希望的路径是：/user/userId	（除了前面的user之外，后面还跟了用户的id）

```
{
	path:'user/:id',
	component:user
}
```

```vue
<div>
	<h2>
        {{$toute.params.id}}
    </h2>
</div>
```

```vue
<router-link to="/user/zhangsan">用户</router-link>
```

#### 7）路由的懒加载

**1.懒加载**

在打包构建应用时，JavaScript包会变得非常大，影响页面加载/

如果能把不同路由对应的组件分割成不同的代码块，当路由访问时加载对应组件，就更高效了。

懒加载：用到时才加载

**2.懒加载的方式**

方式一：AMD写法

```javascript
const About = resolve => require(['../components/About.vue'],resolve)
```

方式二：在ES6中，我们可以有更加简单有效的写法来组织Vue异步组件和webpack的代码分割	(推荐)

```javascript
const Home = () => import('../components/Home.vue')
```

#### 8）嵌套路由

1.实现嵌套路由俩个步骤

> 创建对应的子组件，并在路由映射中配置对应的子路由（通过children属性）

> 在组件内部使用<router-view>标签

2.使用方式:

例如在home页面中，通过home/news访问一些内容，这就需要使用嵌套路由

```javascript

router下的index.js
const HomeNews = ()=>import('../components/HomeNews')
//在home路由层
children:[
    {
        path:'news',
        component:HomeNews
    }
]
```

```jav
Home.vue
<router-link to="/Home/news">新闻</router>
<router-view></router-view>
```

#### 9）vue-router 传递参数

传递参数主要有两种类型：params和query

**params的类型：**

> 配置路由格式：/router/:id
>
> 传递的方式：在path后面跟上对应的值
>
> 传递后形成的路径：/router/123,/router/abc

**query**

>配置路由格式：/router，也就是普通配置
>
>传递的方式：对象中使用query的key作为传递方式
>
>传递后形成的路径：/router?id=123，/router?id=abcv

#### 10）$route和$router区别

> $router为VueRouter实例，想要导航到不同的URL，则使用$router.push方法

> $route 为当前route跳转对象，里面可以获取name，path，query，params等。

#### 11）vue-router导航守卫

**介绍**

> 1.vue-router 提供的导航守卫主要用来==监听路由==的进入和离开的。
>
> 2.vue-router提供了`beforeEach` 和`afterEach`（全局导航守卫）钩子函数，它们会在路由即将改变和改变后触发。
>
> 3.`beforEach((to,from,next)=>{ next() })`   next必须有，不然不会跳转。其中==next==里面用法很灵活，可以向next对象传递任意对象，可以在里面加跳转路径。
>
> 4.`afterEach((to,from) => {})` 	这个是因为是跳转后触发，所以没有next

**分类**

- 全局导航守卫
- 路由独享守卫
- 组件类守卫

**修改网页标题**

1.在路由配置中加入数据元mata属性

```JavaScript
{
	path:'/home',
	meta:{
		title:'首页'
    }
}
```

2.利用全局导航守卫

```javascript
router.beforeEach((to, from, next) => {
  //从from跳转到to，直接to.meta.title嵌套的home将会拿不到数据，显示没定义，则可以利用matched[0]用永远都取第一个
  document.title = to.matched[0].meta.title
  console.log(to);
  // next 必须要
  next()
})
```

#### 12）keep-alive

**介绍**

> 1.keep-alive 是vue内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染，可以配合created(生命周期函数)检测，他又两个非常重要的属性
>
> ​	1）.include-字符串或正则表达式，只有匹配的组件会被缓存，值为组件的name
>
> ​	2）.exclude - 字符串或正则表达式，任何匹配的组件都不会被缓存，所有路径匹配到的试图组件都会被缓存
>
> 2.router-view也是一个组件，如果直接被包在keep-alive里面，所有路径匹配到的视图组件都会被缓存。

#### 13）

### 14文件路径引用的问题（采用别名）

#### 1).js下的文件路径路径引用

```javascript
import Tabbar from '@/component/tabbar/Tabbar'
```

#### 2).html中的文件路径引用

```html
<img src="~assets/img/tabbar/home.png"
```

#### 3)别名配置

![image-20210406100159899](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210406100159899.png)

<img src="C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210406100140786.png" alt="image-20210406100140786" style="zoom: 80%;" />

### 15.Vuex

#### 1）Vuex官方解释：

> Vuex是一个专为Vue.js应用程序开发的状态管理模式

状态管理模式到底是什么

- 可以简单的将其看成把需要多个组件共享的变量全部存储到一个对象中。

- 然后，将这个对象放在顶层的Vue实例中，让其他组件可以使用

- 那么，多个组件就可以共享这个对象中的所有属性了。

- Vuex最大的便利：==响应式==（即如果有数据改变，页面对应也会发生改变）

  ![image-20210407084539987](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210407084539987.png)

#### 2）Vuex管理什么状态呢？

> 用户登陆状态，用户名称，头像，地理位置信息等等
>
> 商品的收藏，购物车中的物品等

#### 3)Vuex多界面状态管理（放在store文件下）

#### 4）Vuex核心概念

- State
- Getters
- Mutation
- Action
- Module

#### 5)State单一状态树

single source of truth 

单一状态树能够让我们最直接的方式找到某一个转台的片段，而在之后的维护和调试中，也可以非常方便的管理和维护

```
state：{
	counter:1000
}
```

使用时：再vue页面

```vue
<h2>{{$store.state.counter}}</h2>
```



#### 6)Getters基本使用

类似计算属性 （当数据需要经过一系列变化之后再到界面上来使用）

```javascript
getter:{
    powerCounter(state){
        return state.counter * state.conuter
    }
}
```

使用时，再vue页面中

```vue
<h2>
    {{$store.getters.powerCounter}}
</h2>
```



#### 7）Mutation

**1.Mutation状态更新**

- Vuex的store状态的更新唯一方式：==提交Mutation==

- Mutation主要包括两部分

  ​	1.字符串的事件类型（type）

  ​	2.一个回调函数（handler），该回调函数的第一个参数就是state

**2.Mutation的定义方式**

```javascript
mutations: {
    //方法
    increment(state) {
      state.counter++
    },
    decrement(state) {
      state.counter--
    }
  },
```

#### 8)Mutation参数

**传入的是一个数值**

```javascript
APP.vue中的methods方法中
addCount(count){
    //提交count 传参
    this.$store.commit('incrementCount',count)
},
```

```javascript
store下的index.js 
mutations:{
    incrementCount(state,count) {
      state.counter += count
    },
}
```

**传入的是对象时**

```javascript
addStudent() {
      const stu = {id:5,name:"Allen",age:35}
      this.$store.commit('addStudent',stu)
}
```

```javascript
 addStudent(state, stu) {
    state.students.push(stu)
 }
```

#### 9）Mutation的提交风格

1.普通的提交方式

```javascript
addCount(count){
   this.$store.commit('incrementCount',count)
 },
```

2.特殊的提交方式

```javascript
addCount(count){
      this.$store.commit({
          type:'incrementCount',
          count
      })
},
```

> 用这种提交方式提交的参数是一个对象形式
>
> 在接受参数时，并不是仅仅是一个count，而是一个对象，且名字可以随便取，而普通方式则不可以

```javascript
mutations:{
    incrementCount(state,payload) {
        //将count取出来得用,payload.count
      state.counter += payload.count
    },
}
```

**Mutation响应规则**

- Vuex的store中的state是响应式的，当state中的数据发生改变时，Vue组件会自动更新

- 这就要求我们必须遵守一些Vuex对应规则

  - 提前在store中初始化好所需的属性

  - 当给state中的对象添加新属性时，使用下面方式

    方式一：使用Vue.set(obj,'newProp',123)

    方式二：用新对象给就对象重新赋值

#### 10）Mutation类型常量

> 在mutation中，定义了很多事件类型
>
> 项目大时，需要花大量的时间记住这些方法名，如果不复制粘贴，还会有写错可能

1.新增一个专门存放类型的js文件mutationType.js

```javascript
export const INCREMENT ='increment'
```

2.APP.vue

```javascript
import {INCREMENT} from './mutationType'
addition(){
      this.$store.commit('INCREMENT')
},
```

3.store/index.js

```javascript
import {INCREMENT} from './mutationType'
mutations:{
     ['INCREMENT'](state) {
      state.counter++
    },
}
```

#### 11）Mutation同步函数

> 通常情况下， Vuex要求Mutation中的方法必须是同步方法
>
> 因为如果是异步操作，那么devtools工具将不能很好的追踪这个操作什么时候被完成

#### 12）Action

**1.定义**

> Action类似Mutation，但是时用来替代Mutation进行异步操作的
>
> Vue页面中利用dispatch提交

==视频 139==



**Actions的写法**

接收一个context（上下文）参数对象

```javascript
actions: {
    AupdateInfo (context) {
     setTimeout(() =>{
         context.commit('updateInfo')
     }，1000)
    }
}
```



1.局部状态通过context.state暴露出来，根节点状态为context.rootState

```javascript
const modules ={
    actions:{
        //这里通过结构赋值，拿出context中对应的对象出来
        increment({state,commit,rootState}){
            ...
        }
    }
}
```

2.如果getters中也需要使用全局的状态，可以接受更多的参数

```javascript
cosnt muduleA = {
    getters:{
        sumwithRootCount(state,getters,rootState){
            return state.count + rootState.count
        }
    }
}
```

3.Action 通过 `store.dispatch` 方法触发：





#### 13）Module（模块）

**1.为什么Vuex中要使用模块呢？**

- Vuex使用单一状态树，那么意味着很多状态都会交给Vuex来管理
- 当应用变得非常复杂时，store对象就有可能变得相当臃肿
- 为了解决这个问题，Vuex允许我们将store分割成模块（Module），而每一个模块拥有自己的state,mutations,action,getters等

**2.模块的组织方式**
![image-20210407092408983](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210407092408983.png)

注意：如果模块要获取根模块中的数据的话

可以通过在方法中传入参数（rootState....)

![image-20210407095248746](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210407095248746.png)

#### 14）补充（Vuex中的辅助函数）



- **mapState**

  > 通过引用State中定义的变量或引用定义在computed中的代码，如果需要Vuex中的多个数据时，这样写就太罗嗦了，Vuex提供了更简洁的mapState方法

```javascript
import {mapState} from 'vuex'
export default {
  name: 'home',
  computed: mapState(['nickname','age','gender'])
}
```

```text
mapState(['nickname','age','gender']) //映射哪些字段就填入哪些字段
```

这一句代码相当于这些

```javascript
nickname(){return this.$store.state.nickname}
age(){return this.$store.state.age}
gender(){return this.$store.state.gender}
```

**记住：用mapState等这种辅助函数的时候，前面的方法名和获取的属性名是一致的。**

如果我们需要自定义一个计算属性怎么办呢？怎么添加呢？

这时候我们就需要es6中的展开运算符：...

```javascript
computed: {   //computed是不能传参数的
  value(){
   return this.val/7
},
  ...mapState(['nickname','age','gender'])
}
```

- **mapGetters** 

vue部分

```JavaScript
computed: {  //computed是不能传参数的
 valued(){
   return this.value/7
 },
 ...mapGetters(['realname','money_us'])
}
```

- **mapMutations**
- **mapActions**



### 16.axios

#### 1.功能特点

- 在浏览器中发送XMLHttpRequests请求
- 在node.js中发送http请求
- 支持promise API
- 拦截请求和响应
- 转换请求和响应数据
- 等

#### 2.axios的基本使用

**1)安装**：

npm install axios --save

**2)发送并发请求**

1.使用axios.all，可以放入多个请求的数组

```javascript
//axios发送并发请求
axios.all([axios({
  url:'http://123.207.32.32:8000/home/multidata'
}), axios({
  url: 'http://123.207.32.32:8000/home/data',
  params: {
    type: 'sell',
    page:1
  }
})])
  .then(results => {
    //打印一条data数据，其中包含了两个请求的数据
  console.log(results);
})
```

2.axios.all([])，返回的结果是一个数组，使用axios.spread可以将数组[res1,res2] 展开为res1，res2

```javascript
//axios发送并发请求
axios.all([axios({
  url:'http://123.207.32.32:8000/home/multidata'
}), axios({
  url: 'http://123.207.32.32:8000/home/data',
  params: {
    type: 'sell',
    page:1
  }
})])
  .then(axios.spread((res1,res2) => {
    //打印两条data数据
    console.log(res1);
    console.log(res2);
}))
```

#### 3.axios的配置信息相关

> 因为BaseURL是固定的，开发中很多参数都是固定的
>
> 这个时候可以进行一些抽取，也可以利用axios的全局配置

```javascript
//url的抽离
axios.defaults.baseURL = 'http://123.207.32.32:8000'
//header(自定义请求头)里面东西的抽离
axios.defaults.headers.post.['Content-Type'] = 'application/x-www-from-urlencoded';
```

### 补充

#### 1.vue中ref标签属性和$ref的关系

> javascript原生语法和jquery都是属于操作DOM节点元素,去实现功能的
>
> Vue给我们提供了一个专门用来获取DOM节点的方法 ，使用元素的ref属性，使用起来非常方便

**vue的ref属性**

ref 被用来给元素或子组件注册引用信息。引用信息将会注册在父组件的 $refs 对象上。如果在普通的 DOM 元素上使用，引用指向的就是 DOM 元素；如果用在子组件上，引用就指向组件实例：

```vue
<!-- `vm.$refs.p` will be the DOM node -->
<p ref="p">hello</p>

<!-- `vm.$refs.child` will be the child component instance -->
<child-component ref="child"></child-component>

```

可以通过this.$ref获取页面中含有ref属性的DOM元素

> 当 v-for 用于元素或组件的时候，引用信息将是包含 DOM 节点或组件实例的数组。
>
> 关于 ref 注册时间的重要说明：因为 ref 本身是作为渲染结果被创建的，在初始渲染的时候你不能访问它们 - 它们还不存在！$refs 也不是响应式的，因此你不应该试图用它在模板中做数据绑定。

**vm.$refs**



#### 2.浏览器本地存储

**localStorage**  关闭浏览器后数据并不会清空，需要手动清除

```js
let p = {name:'zhangsan',age:18}

function saveData() {
	locationStorage.setItem('msg',JSON.stringfy(p))  // JSON.stringfy(p)将json格式转为字符串
}
// 如果不进行转换的话，p不是一个字符串 ，p会调用toString方法，则再浏览器缓存中无法显示p中的数据

//读取
localStorage.getItem('msg') 
//删除
localStorage.removeItem('msg') 
```

**sessionStorage**  只要浏览器关闭，数据就会清空
