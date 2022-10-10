### 1. JS中栈(stack)和堆(heap)的理解

> 通常这与JS的内存回收机制有关系，为了使程序运行时内存占用最小。



#### 1.1 JS中变量的基本类型

- 基本类型：`null`、`undefined`、`BigInt`、`Number`、`string`、`boolean`、`symbol`
- 引用类型：`Object`  (`Array`、`function`、`Date`、`Map`、`Set`、`WeakMap`、`WeakSet`、`RegExp`)



#### 1.2 栈内存

> 主要用于存放基本类型和对象变量的指针，定义一个变量时，计算机会在内存中开辟一块存储空间，这块空间就叫栈。
>
> 栈内存自动分配内存空间，并由系统自动释放。



#### 1.3 堆内存

> 主要用于存放引用类型,例如`Object`这类，存储的对象类型数据大小是未知的。（大概为什么null是Object类型而存放在栈内存中）
>
> 堆内存是动态分配内存，内存大小不一，也不会自动释放，一般由程序员主动释放，即将这个变量置为null



#### 1.4 `const`关键字

- `const`关键字用来定义常量，`const`实际保证的 并不是变量的值不变，而是变量指向的那个内存地址所保存的数据不得改动。
- 对于复合类型的数据（主要是数组和对象），变量指向的栈内存地址，保存的是一个栈内存指针（该指针指向堆内存地址），`const`保证的是栈内存中保存的指针不得改动。
- 对于简单类型（数值、字符串、布尔值），值就保存在变量指向的那个栈内存地址，`const`保证的是栈内存地址所保存的数据不得改动。

*****



### 2. 模块化机制理解



#### 2.1 require的加载机制



#### 2.2 module.exports 和 exports的区别

> 为了实现模块的导出，Node中使用的是Module的类，每一个模块都是Module的一个实例，也就是module;（每一个js文件都有一个module对象）
>
> 为了方便，Node为每个模块提供一个exports变量，指向module.exports。

exports 相当于 module.exports 的快捷方式如下所示:

```js
var exports = module.exports;
```

但是要注意，不能改变exports的指向，而module.exports可以改变指向

```js
// 错误的写法 将会得到 undefined
exports = {
  'a': 1,
  'b': 2
}

// 正确的写法 module.exports = {},会在堆内存中新开辟一个内存空间
modules.exports = {
  'a': 1,
  'b': 2
}
```

