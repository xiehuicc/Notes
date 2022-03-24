### 1.koa-router allowedMethods

#### 1)allowedMethods 方法

> allowedMethods处理的业务是当所有路由中间件执行完成之后，若ctx.status为空或者404的时候，丰富`response`对象的`header`头

#### 2）allowedMethods 应用场景

1.全局应用

```javascript
  var Koa = require('koa');
  var Router = require('koa-router');
 
  var app = new Koa();
  var router = new Router();
 
  app.use(router.routes());
  app.use(router.allowedMethods());
```

2。局部应用

```javascript
  var Koa = require('koa');
  var Router = require('koa-router');
 
  var app = new Koa();
  var router = new Router();
  router.use('/test',router.allowedMethods())
  app.use(router.routes());

//这时只有当请求路径匹配到了/test才会执行allowedMethods，然后根据ctx.status设置response响应头
```