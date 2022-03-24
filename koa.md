### koa核心概念

- koa Application（应用程序）
- Context（上下文）
- Request（请求）、Response（响应）



### koa工作原理

![image-20210609094825863](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210609094825863.png)

- 执行的顺序：顺序执行
- 回调的顺序：反向执行
- 先进后出



**Content对象**

> Koa 提供一个 Context 对象，表示一次对话的上下文（包括 HTTP 请求和 HTTP 回复）。通过加工这个对象，就可以控制返回给用户的内容。