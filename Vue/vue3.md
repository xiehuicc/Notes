# Vue 3.0 (One Piece)

------

## 第一章: Vue 3.0 简介

> 1. 性能提升 1.3 ~ 2X
> 2. TS 支持, 新增: Fragment、Teleport、Suspense
> 3. 按需加载(配合 vite) & 组合API

### 1.1 Compiler 原理篇

> 1. 静态 Node 不再作更新处理 (hoistStatic -> SSR优化)
> 2. 静态绑定的 class, id不再作更新处理
> 3. 结合打包标记PatchFlag, 进行更新分析(动态绑定)
> 4. 事件监听器 Cache 缓存处理 (cacheHandlers)
> 5. hoistStatic 自动针对多静态节点进行优化, 输出字符串

### 1.2 新增功能

> 1. Fragment: 不受根节点限制, 渲染函数可接收 Array
> 2. Teleport: 类似 Portal, 随用随取, e.g.弹窗, Actions
> 3. Suspense: 嵌套的异步依赖, e.g. async setup()

### 1.3 Vue 3.0 的变化

> 1. 组合式API + 函数式编程
> 2. 组件间逻辑共享

### 1.4 为什么要用 Composition API

> 1. Vue 2.0 对于复杂的逻辑组件, 在后期变得无法维护
> 2. Vue 2.0 中代码复用办法, 如: Mixin, Filters 都有缺陷
> 3. Vue 2.0 对 TS 支持不充分

### 1.5 Options API

> 1. components -- props -- data -- computed -- methods -- 生命周期的办法