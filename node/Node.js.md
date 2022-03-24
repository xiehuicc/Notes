### 1.node.js简介

#### 1）介绍

- Node.js® is a JavaScript runtime built on [Chrome's V8 JavaScript engine](https://v8.dev/).

  > - Node.js 不是一门语言
  >
  > - Node.js不是库，不是框架
  >
  > - Node.js 是一个JavaScript运行时环境
  >
  > - 简单来讲就是Node.js可以解析和执行JavaScript代码
  >
  > - 以前只有浏览器可以解析执行JavaScript代码
  >
  > - 也就是说现在的JavaScript可以完全脱离浏览器来运行，一切都归于：Node.js

- 浏览器中的JavaScript

  - Ecmascript
    * 基本的语法
    * if
    * var
    * function
    * Object
    * Array

  - BOM
  - DOM

- Node.js中的JavaScript

  - **没有BOM、DOM**

  - Ecmascript

  - 在Node.js这个JavaScript执行环境中提供了一些服务级别的操作==API==
    - 例如文件读写
    - 网络服务的构建
    - 网络通信
    - http服务器
    - 等。。。

![image-20210412124030766](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210412124030766.png)

#### 2）Node.js能做什么

- Web 服务器后台
- 命令行工具
- 对于前端开发来说，接触node最多的时它的命令行工具
  - 自己写的很少，主要是使用第三方
  - webpack
  - gulp
  - npm

#### 3）一些学习资源

- 《深入浅出Node.js》
  - 朴灵
  - 偏理论，几乎没有实战内容
  - 理解原理底层
- 《Node.js权威指南》
  - API讲解
  - 也没有业务，实战
- JavaScript标准参考教程https://javascript.ruanyifeng.com/
- Node.js入门 https://www.nodebeginner.org/
- Node.js中文社区 https://cnodejs.org/

#### 4）这门课能学到什么？

- B/S编程模型

  - Broswer-Server
  - back-end
  - 任何服务器技术这种BS编程模型都是一样的，和语言无关
  - Node只是作为我们学习BS编程模型的工具

- 模块化编程

  - RequireJs
  - SeaJs
  - css引入文件`@import('文件路径')`
  - 以前的认值中，JavaScript只能通过script标签加载

- Node常用API

- 异步编程

  - 回调函数
  - Promise
  - async
  - generator

- Express Web开发框架

- EcmaScript 6

  

### 2.起步

#### 1）安装Node环境

#### 2）使用node执行js脚本文件

**1.使用fs读写文件**

> fs（file-system）文件系统  浏览器是没有这个方法的，只有node有。
>
> 在Node中如果想要进行文件操作，就必须引入fs这个核心模块
>
> 在fs这个核心模块中，就提供了所有文件操作的API
>
> 例如：fs.readFile 就是用来读取文件的
>
> ```javascript
>  var fs = require('fs')
>  //读取文件 第一个参数书文件路径，第二个参数是一个回调函数(两个参数顺序不能错)
>  fs.readFile('文件路径',function(err,data ){
>      
>  })
> ```

2.http

```javascript
var http = require('http')

//返回一个serve实例
var server = http.createServer()

//3.服务器要干嘛
//  提供服务：对数据的服务
//  发送请求
//  处理请求
//  给个反馈
//  注册request请求事件
//  当客户端请求过来，就会自动触发服务器的request请求事件，然后执行第二个参数：回调处理
server.on('request', function () {
  console.log('客户端的请求收到了');
})

//4.绑定端口号，启动服务器
server.listen(3000, function () {
  console.log('服务器启动成功！通过http://127.0.0.1:3000进行访问');
})
```

```javascript
http-res.js
var http = require('http')

var server = http.createServer()
/* request 请求事件处理函数，需要接受两个参数：
      Requset 请求对象
        请求对象可以用来获取客户端的一些请求信息，例如请求路径
      Response响应对象
        响应对象可以用来给客户断发送相应信息
      
  */
server.on('request', function (request,response) {
  console.log('客户端的请求收到了,请求路径是：'+request.url);
  /* response 对象有一个方法：write可以给客户端发送响应数据
  write可以使用多次，但是最后一定要用end来结束响应，否则客户端一致等待 */
  // response.write('hello')
  // response.write('node.js')
  // response.end()
  //上面的方式比较麻烦，推荐使用简单的方式，直接end的同时传送数据
  response.end('hello node.js')
})

server.listen(3000, function () {
  console.log('服务器启动成功！通过http://127.0.0.1:3000进行访问');
})
```

#### 3）配置nodemon 监听文件变化 自动重启服务

1. npm install nodemon -D
2. 修改 package.json 中的启动命令  

```js
// 例如
"scripts"{
    "dev":"nodemon src/app.js"
}

```

​	3.通过增加nodemon.json 配置 指定特殊 watch 的文件

```js
//例如指定src下的所有文件下的js文件
{
    "watch":["./src/**/*.js"]
}
```

#### 4）nrm管理npm源，nvm管理nodejs版本

#### 5）express中的中间件

**中间件的完整结构**

1. 是一个函数
2. err，req，res，next

**中间件的使用**

**	**app级别（全局）比如 日志可能是全局的

- 注册的时候，一定要在最顶级
- app.use ---->api去加载进来

```js
//app.js
function valid_name_middleware(err,req,res,next) {
	let {name} = req.query
    if(!name || !name.length) {
		res.json({
			message:"缺少name参数"
        })
    }else{
		next()
    }
}
function log_middleware(req,res,next){
	console.log("请求来了")
    next()
}

app.use( log_middleware)
app.all('*',valid_name_middleware)	// 所有的请求都要校验name参数

```



**2**.router级别

```js
// router.js

//1. 第一个场景
router.use(function	(req,res,next){
	console.log('log from router')
    next()
})


function valid_login_middleware(err,req,res,next) {
	let {name，password} = req.query
    if(!name || !password) {
		res.json({
			message:"参数校验失败"
        })
    }else{
        req.formdata = {	// 成功则将参数传入给formdata
			name,
            password
        }
		next()
    }
}

// 2.路由内部使用	
router.get('/login',[valid_login_middleware],(req,res)=>{
	let {formdata} = req
    res.json({
        formdata
		message:"from router demo"
    })
})
```



**3**.异常处理--->app级别----->router

```js
// app.js

app.get('/demo',(req,res)=>{
    throw new Error("测试功能异常")
})

function error_handle_middleware(err,req,res,next) {
	if(err) {
		res.status(500)
        .json({
			message:"服务器异常"
        })
    }else{
        
    }
}

function 404_handle_middleware(err,req,res,next) {
	res.json({
		message:"api不存在"
    })
}
app.use(404_handle_middleware)
app.use(error_handle_middleware)
```





### 3.Node中的JavaScript

#### 1）核心模块

> Node为JavaScript提供了许多服务器级别的API，这些API绝大多数都被包装到了一个具名的核心模块中。
>
> 例如：文件操作中的fs核心模块，`http`服务构建的`http`模块，`path`路径模块操作，`os`操作系统信息模块...

#### 2)使用核心模块

```javascript
var http = require('http')
...
```

#### 3）模块化

> require 是一个方法
>
> 它的作用：加载模块的
>
> 在Node中，模块有三种：
>
> ​	1.具名的核心模块：例如：fs、http
>
> ​	2.用户自己编写的文件模块
>
> ​		相对路径必须加 ./
>
> ​		可以省略后缀名
>
> ​		相对路径中的./不能省略
>
> ​	3.在Node中，没有全局作用域，只有作用域
>
> ​		外部访问不到内部
>
> ​		内部也访问不到外部

#### 4）module.exports 与exports的区别

> require方能看到的只有module.exports这个对象，他是看不到exports对象的，而我们在编写模块时用到的exports对象实际上只是对module.exports的引用

```js
var module = {
    exports: {
        name:"hello world"
    }
}

var exports = module.exports
console.log(module.exports); // { name: 'hello world' }
console.log(exports); // { name: 'hello world' }
```

```js
export.name = "你好 世界"

//exports = {
//	name:'hello'
//	age:"male"
//}	
//exports默认为module.exports的快捷方式，只能往里面添加属性，不能改变它的指向，否则exports跟普通的对象没有区别

module.exports = {
	name: "hello",
    age: "male"
}
```



### 4.web服务器开发

#### 1）ip地址和端口号

> 所有联网的程序都需要进行网络通信
>
> 计算机中只有一个物理网卡，而且同一个局域网中，网卡的位置必须是唯一的。
>
> 网卡是通过唯一的IP地址来进行定位的。

> IP地址用来定位计算机
>
> 端口号用来定位具体的应用程序（所有需要联网通信的软件都必须具有端口号）
>
> 可以开启多个服务，但一定要确保	不同的服务占用的端口号不一致	

### 5.node基础

#### global（全局可用）

- CommonJS
- Buffer、process( argv,argv0,execArgv,execPath )、console
- timer

package.json 包描述文件	（json文件中不能写注释）

npm（Node Package Manager）

npm命令：

> npm  search 包名   ——搜索npm包
>
> npm init    ——初始化package.json 文件， 有package.json 文件时 npm install 下载的模块才会在该目录下
>
> npm remove 包名 ——删除包
>
> npm install --save 包名  ——安装 包并添加到依赖中
>
> npm install 包名 -g  ——全局安装包（一般是一些工具）

#### 1）Buffer（缓存区）

> Buffer 的结构和数组很想，	操作的方法也和数组类似
>
> 数组中不能存储二进制文件，而Buffer就是专门用来存储二进制数剧
>
> 使用Buffer不用引入模块，直接使用即可
>
> 在Buffer存储的都是二进制数，但是在显示中都是以16进制的形式显示
>
> Buffer中每一个元素的大小是从 00 —FF	 0- 255
>
> Buffer中的一个元素占用一个字节 （8位 = 1）
>
> Buffer的大小一旦确定，则不能修改，Buffer实际上是对底层内存的直接操作

- Buffer 用于处理二进制数据流
- 实例类似整数数组，大小固定
- c++代码在V8堆外分配物理内存

``` js
var str = "hello world"
// 将一个字符串保存到Buffer中
var Buf = Buffer.from(str)
```

创建一个10个字节的BUffer

```js
let Buf = Buffer.alloc(10)
```

`Buffer.allocUnsafe(size) `创建一个指定大小的buffer，但是buffer中可能含有敏感数据

> 用Buffer.allocUnsafe(size)创建一个指定大小的buffer时，如果该内存地址存在数据，则不会进行删除原本的数据，只分配空间并不清空内存地址中的数据（Buffer.alloc(size)则会进行删除）

```js
// 将缓冲区的数据转化为字符串
- Buf.toString()
// 查看字节长度
- Buffer.byteLength()
// 查看是不是Buffer对象
- Buffer.isBuffer()
// 拼接Buffer，并不是n个参数，而是一个参数里面是Buffer数组
- Buffer.concat()
// buffer 的长度 （Buffer占的字节数）
- buf.length
// 
- buf.toString()
// 往Buffer中填充值
- buf.fill()
// 比较两个Buffer是否一致
- buf.equals()
// 和数组的一样，没找到返回-1
- buf.indexOf()
//
- buf.copy()
```

``` js
const bf1 = Buffer.from('This')
const bf2 = Buffer.from('is')
cosnt bf3 = Buffer.form('a')
const bf4 = Buffer.from('test')

const bf = Buffer.concat([bf1,bf2,bf3,bf4]) 
console.log(bf.toString())// This is test
```



#### 2）fs（文件系统）

> 在Node 中，服务器的本质就是将本地文件发送给远程的客户端
>
> 文件系统简单来说就是通过Node来操作系统中的文件
>
> Node通过fs模块来和文件系统进行交互
>
> 该模块提供了一些标准文件访问API来打开，读取，写入文件，以及与器交互

**同步**

### API学习

#### 1）fs

#### 2）http

#### 3) path

- dirname、

> `_dirname`、`_filename`总是返回文件的绝对路径
>
> `process.cwd()`总是返回执行node命令所在文件夹

#### 4）Buffer
