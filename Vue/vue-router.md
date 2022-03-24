### 1.vue路由原理

> 无刷新跳转页面，是单页应用的一大优势，用户体验好，加载速度快，vue 路由的跳转就是无刷新的。**`其本质就是:建立并管理url和对应组件之间的映射关系.`**

> 它有两种形式，可以在定义路由的时候通过 `mode` 字段去配置，如果不配置这个字段，那么默认使用的就是 `hash` 模式。 

- hash 模式，即通过在链接后添加 # + 路由名字，根据匹配这个字段的变化，触发 `hashchange` 事件，动态的渲染出页面。就有点类似像 a 链接用作页面上的锚点一样，不会刷新页面。

- `history` 模式，也就是使用的浏览器的 history API，`pushState` 和 `replaceState`。通过调用 `pushState` 操作浏览器的 `history` 对象，改变当前地址，同时结合`window.onpopstate` 监听浏览器的返回和前进事件，同样可以实现无刷新的跳转页面。`replaceState` 与 `pushStete` 不同的就是，前者是替换一条记录，后者是添加一条记录。

有关`pushState`的用法，【MDN文档】(https://developer.mozilla.org/zh-CN/docs/Web/API/History_API)



> 这个 API  与 `ajax` 合起来构成的 `pjax` 技术，也是用于实现页面无刷新加载的一种方式，常用于 PC 长列表页面的翻页啥的~ history 相对于 hash 模式来说，最大的好处就是没有讨厌的 # 符号，比如同样一个 list 页面，在 `hash` 模式下，url 链接表现为 `http://yoursite.com/#/list`，看着就难受。在 `history` 模式下面，url 链接表现为 `http://yoursite.com/list` 



### 2.vue Router 基础

#### 重定向和别名

> 重定向也是通过 `routes` 配置来完成
>
> - “重定向”的意思是，当用户访问 `/a`时，URL 将会被替换成 `/b`，然后匹配路由为 `/b`，
> - “别名”是   **`/a` 的别名是 `/b`，意味着，当用户访问 `/b` 时，URL 会保持为 `/b`，但是路由匹配则为 `/a`，就像用户访问 `/a` 一样**

```js
// 是从 /a 重定向到 /b：
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: '/b' }
  ]
})

//重定向的目标是一个命名的路由
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: { name: 'foo' }}
  ]
})

//重定向的目标是一个方法，动态返回重定向目标
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: to => {
      // 方法接收 目标路由 作为参数
      // return 重定向的 字符串路径/路径对象
    }}
  ]
})
```

```js
//访问 /b 时，URL 会保持为 /b，但是路由匹配则为 /a，就像用户访问 /a 一样。
//“别名”的功能让你可以自由地将 UI 结构映射到任意的 URL，而不是受限于配置的嵌套路由结构。
const router = new VueRouter({
  routes: [
    { path: '/a', component: A, alias: '/b' }
  ]
})
```

### 3.vue路由传参的两种方式

- params 是以 `/params` 的形式表现在 url 上，使用 params 参数还需要配置到路由定义上，不然不会展示在 url 上，并且刷新就会消失。
- query 是以 `?query=query1` 这种形式表现在 url 上，

**需要注意的地方就是**：如果传 params 参数，不能使用 `path` 字段跳转，否则没效果。而 query 参数则没有这个限制，使用 `name` 和 `path` 字段都可以。



### 4.vue路由的跳转

跳转分为两种：

1. 第一种声明式，使用`<router-link>`声明跳转，`to`属性定义跳转的参数。
2. 另一种是编程式，使用 `router.go()`、`router.push()`、`router.replace()`方法进行跳转，`go`方法就是与浏览器的`history` api 的方法相同，可以进行返回上一页等操作。

> `push`方法和`replace`方法的区别在于，前者会把当前页面加入 history 记录里面，可以通过`history.go(-1)`回到这个页面。而`replace`方法则会在 history 记录里面替换掉当前记录，如果你在跳转后的新页面返回上一页，它==不会回到跳转前的页面==，会回到上上个页面，如果上上个页面没有记录，则不会跳转。



### 5.vue路由守卫

> vue 路由守卫分为三种，
>
> - 一种是全局的路由守卫，通常在实例化路由之后设置，来做一些通用的路由操作，它所有的路由跳转都会执行的操作；
> - 一种是单个路由独享的守卫，在单个路由定义的时候进行设置，所有跳转这个路由都会执行；
> - 另外一种就是组件内的守卫，只在组件内生效。

1. 全局路由守卫类型：
   - `router.beforeEach(to, from, next)`
   - `router.afterEach(to, from, next)`
2. 路由独享的守卫：
   - `beforeEnter(to, from, next)`
3. 组件独享的守卫:
   - `beforeRouteEnter(to, from, next)`
   - `beforeRouteUpdate(to, from, next)` —— 动态参数路径改变时，组件实例被复用的时候调用。
   - `beforeRouteLeave(to, from, next)` —— 导航离开组件所在路由时被调用。



### 6.vue路由的一些应用场景处理

#### 1.什么时候用 push，什么时候用 replace

> 简单来说，当你需要从A页面跳转到B页面，再跳转到C页面，然后在C页面返回，能直接返回到A页面。那么在B页面跳转C页面的时候，使用`replace()`方法进行跳转即可。

#### 2.动态改变页面的title

> - 定义路由的时候，在路由的 meta 字段里面添加一个 title 属性，定义好页面的标题名称。
> - 在全局的路由守卫`beforeEach()`方法里面添加一个判断，获取路由的 meta 字段，动态的改变页面的 title



``` javascript
// router.js
{
	path: '/index',
	name: 'index',
	meta: {
		title: '首页'
	}
}

// main.js
router.beforeEach(to, from, next){
	if(to.meta.title){
		document.title = to.meta.title
	}
	next()  // 这个方法必须调用 不然路由不会跳转
}

```

