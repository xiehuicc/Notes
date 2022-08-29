### 1.异步I/O与非阻塞I/O

> 从实际效果看，异步和非阻塞都达到了我们并行I/O的目的。但是从计算机内核I/O而言，异步/同步和阻塞/非阻塞实际是两回事。

操作系统内核对于I/O只有两种方式：阻塞与非阻塞。

在调用**阻塞I/O**时，应用程序需要**等待I/O完成才返回结果**。阻塞I/O造成cpu等待I/O，浪费等待时间，cpu的处理能力得不到充分利用。

为了提高性能，内核提供了**非阻塞I/O**。非阻塞I/O跟阻塞I/O差别为调用之后会**立即返回**。但非阻塞I/O也存在一些问题，由于完整的I/O并没有完成，立即返回的并不是业务层期望的数据，而仅仅是**当前调用的状态**。为了获得完整的数据，需要重复调用I/O操作来确认是否完成。这种重复调用的判断操作是否完成的技术叫做**轮询**。

### 2. Node的异步I/O

> 介绍完系统的对异步I/O的支持后，我们继续介绍Node是如何实现异步I/O的。

#### 2.1事件循环

> Node自身的执行模型——事件循环，正是它使得毁掉函数十分普遍。事件循环是 Node.js 处理非阻塞 I/O 操作的机制
>
> 在进程启动时，Node便会创建一个类似while（true）的循环，每执行一次循环体的过程称为Tick。每个Tick的过程就是查看是否有事件待处理，如果有，就取出事件及相关的回调函数。如果不再有事件处理，就退出进程。

![](http://showdoc.hongqiaomall.com.cn/server/index.php?s=/api/attachment/visitFile/sign/d87d1cad6e5d46e73dae7be448ecf5a4&showdoc=.jpg)

#### 2.2 Node循环事件的阶段

**一次完整的事件循环Tick分成很多个阶段：**

- 1. 定时器（Timers）：本阶段执行已经被`setTimeout()`和`setInterval()`的调度的回调函数
- 2. 待定回调（pending callback）：对某些系统操作（如TCP错误类型）执行回调，比如TCP连接时收到ECONNREFUSED
- 3. idle，prepare：仅系统内部使用
- 4. 轮询（Poll）：检查新的I/O事件；执行与I/O相关回调
- 5. 检测：`setImmediate()` 回调函数在这里执行
- 6. 关闭的回调函数：一些关闭的回调函数，如：socke.on('close',...)

#### 2.3 阶段详情概述

**轮询阶段**

**`setTimeout()`和`setImmediate()`对比**

- `setTimeout()`设计为一旦在当前轮询阶段完成，就执行脚本。
- `setImmdiate()`在最小阈值（ms单位）过后的脚本。

例如，如果运行以下不在 I/O 周期（即主模块）内的脚本，则执行两个计时器的顺序是非确定性的，因为它受进程性能的约束：

```js
// timeout_vs_immediate.js
setTimeout(() => {
  console.log('timeout');
}, 0);

setImmediate(() => {
  console.log('immediate');
});


$ node timeout_vs_immediate.js
timeout
immediate

$ node timeout_vs_immediate.js
immediate
timeout
```

但是，如果你把这两个函数放入一个 I/O 循环内调用，`setImmediate` 总是被优先调用：

```js
// timeout_vs_immediate.js
const fs = require('fs');

fs.readFile(__filename, () => {
  setTimeout(() => {
    console.log('timeout');
  }, 0);
  setImmediate(() => {
    console.log('immediate');
  });
});


$ node timeout_vs_immediate.js
immediate
timeout

$ node timeout_vs_immediate.js
immediate
timeout
```

#### 2.4观察者模式

> 在每个Tick过程中，用于判断是否有事件需要处理。
>
> 每个事件循环中有一个或者多个观察者，而判断是否有事件要处理的过程就是向这些观察者询问
> 是否有要处理的事件。

事件循环是一个典型的生产者/消费者模型。异步I/O、网络请求等则是事件的生产者，源源
不断为Node提供不同类型的事件，这些事件被传递到对应的观察者那里，事件循环则从观察者那
里取出事件并处理。

#### 2.5请求对象

#### 2.6执行回调

组装好请求对象、送入I/O线程池等待执行，实际上完成了异步I/O的第一部分，回调通知是
第二部分

#### 2.7总结

事件循环、观察者、请求对象、I/O线程池这四者共同构成了Node异步I/O模型的基本要素。

![](http://showdoc.hongqiaomall.com.cn/server/index.php?s=/api/attachment/visitFile/sign/9a0313fa19a8b1c435af1723e7fe3b05&showdoc=.jpg)

我们知道JavaScript是单线程的，所以按常识很容易理解为它不能充分利用多核CPU。事实上，在Node中，
除了JavaScript是单线程外，Node自身其实是多线程的，只是I/O线程使用的CPU较少。另一个需要重视的观点则是，除了用户代码无法并行执行外，所有的I/O（磁盘I/O和网络I/O等）则是可以并行起来的。

### 3.非I/O的异步API

- `setTimeout()`
- `setInterval()`
- `setImmediate()`
- `process.nextTick()`

#### 3.1定时器

`setTimeout()`和`setInterval()`与浏览器中的API是一致的，分别用于单次和多次定时执行任务。

> 它们的实现原理与异步I/O比较类似，只是不需要I/O线程池的参与。调用`setTimeout()`或者
> `setInterval()`创建的定时器会被插入到定时器观察者内部的一个红黑树中。每次Tick执行时，会
> 从该红黑树中迭代取出定时器对象，检查是否超过定时时间，如果超过，就形成一个事件，它的
> 回调函数将立即执行。

定时器的问题在于，它并非精确的（在容忍范围内）。尽管事件循环十分快，但是如果某一
次循环占用的时间较多，那么下次循环时，它也许已经超时很久了。譬如通过`setTimeout()`设定
一个任务在10毫秒后执行，但是在9毫秒后，有一个任务占用了5毫秒的CPU时间片，再次轮到定
时器执行时，时间就已经过期4毫秒。

![](http://showdoc.hongqiaomall.com.cn/server/index.php?s=/api/attachment/visitFile/sign/f17f24392c326df5753fc8bf8cd0a8ce&showdoc=.jpg)



#### 3.2`process.nextTick()`

由于事件循环自身的特点，定时器的精确度不够。而事实上，采用定时器需要动用红黑树，
创建定时器对象和迭代等操作，而`setTimeout(fn, 0)`的方式较为浪费性能。实际上，
`process.nextTick()`方法的操作相对较为轻量，具体代码如下：

```js
process.nextTick = function(callback) { 
 // on the way out, don't bother. 
 // it won't get fired anyway 
 if (process._exiting) return; 
 if (tickDepth >= process.maxTickDepth) 
 maxTickWarn(); 
 var tock = { callback: callback }; 
 if (process.domain) tock.domain = process.domain; 
 nextTickQueue.push(tock); 
 if (nextTickQueue.length) { 
 process._needTickCallback(); 
 } 
}; 

```

每次调用`process.nextTick()`方法，只会将回调函数放入队列中，在下一轮Tick时取出执行。
定时器中采用红黑树的操作时间复杂度为O(lg(n))，nextTick()的时间复杂度为O(1)。相较之下，
`process.nextTick()`更高效。

#### 3.3 `setImmediate()`

`setImmediate()`方法与`process.nextTick()`方法十分类似，都是**将回调函数延迟执行**。在Node 
v0.9.1 之前，`setImmediate()`还没有实现，那时候实现类似的功能主要是通过`process.nextTick()`
来完成，该方法的代码如下所示：

```js
// process.nextTick()
process.nextTick(function () { 
 console.log('延迟执行'); 
}); 
console.log('正常执行'); 

// 输出
正常执行
延迟执行

// setImmediate()
setImmediate(function () { 
 console.log('延迟执行'); 
}); 
console.log('正常执行'); 

// 输出
正常执行
延迟执行

// 将它们放在一起时，又会是怎样的优先级呢
process.nextTick(function () { 
 console.log('nextTick延迟执行'); 
}); 
setImmediate(function () { 
 console.log('setImmediate延迟执行'); 
}); 
console.log('正常执行'); 
// 其执行结果如下：
正常执行
nextTick延迟执行
setImmediate延迟执
```

从结果可以看出，`process.nextTick()`中的回调执行的优先级要高于`setImmediate()`,这是因为事件循环对观察者的检查是有先后顺序的。

### 4.事件驱动与高性能服务器

Node通过**事件驱动**的方式处理请求，无需为每一个请求创建额外的对应线程，可以省掉创建线程和销毁线程的开销。同时操作系统在调度任务时因为线程较少，上下文切换的代价很低。这使得服务器能够有条不紊地处理请求，即使在大量的连接情况下，也不受线程上下文切换开销的影响，这是Node高性能的一个原因。

