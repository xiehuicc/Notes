# Web APIS

> **0.1.0**

------

## DOM

### DOM 简介

> 1. **DOM : 文档对象模型(Document Object Model, 简称 DOM), 是 W3C 组织推荐的处理可扩展标记语言(HTML 或者 XML)的标准编程接口**
> 2. **作用 : 通过 DOM接口 可以改变网页的内容, 结构和样式** 

### DOM 树

<img src="E:\Typora插入图片时保存的图片\image-20201230144547232.png" alt="image-20201230144547232" style="zoom:100%;" />

> 1. **文档 : 一个页面就是一个文档, DOM 中使用 document 表示**
> 2. **元素 : 页面中所有的标签都是元素, DOM 中使用 element 表示**
> 3. **节点 : 网页中所有的内容都是节点(标签, 属性, 文本, 注释等), DOM 中使用 node 表示**
> 4. **DOM 把以上所有内容都看作对象**

### 获取页面元素

> 1. **根据 ID 获取**
> 2. **根据标签名获取**
> 3. **通过 HTML5 新增的方法获取**
> 4. **特殊元素获取**

#### 根据 ID 获取

##### document.getElementById()

> 1. **不同于其他 Element 查找方法(如Document.querySelector() 以及 Document.querySelectorAll()), getElementById()只有在作为 document 的方法时才起作用, 而在 DOM 中其他元素下无法生效, 这是因为 ID值 在整个网页中必须保持唯一, 因此没有必要为这个方法创建所谓的局部版本**
> 2. **语法 : let element = document.getElementById('id')**
>    - **id : 元素id,大小写敏感**
> 3. **作用 : 获取页面元素**
> 4. **返回值 : 一个 Element 对象, 如果不存在返回 null**

```javascript
  <div id="time">2020-12-30</div>

  <script>
    // 1. 因为我们文档页面从上往下加载  所以得现有标签  所以我们script写到标签下面
    // 2. 参数id 是大小写敏感的字符串
    // 3. 返回的是一个元素对象
    let time = document.getElementById('time')
    console.log(time);
    console.log(typeof time);
    // 4. console.dir() 打印我们返回的元素对象 更好地查看里面的属性和方法
    console.dir(time)
  </script>
```

#### 根据标签名获取

##### getElementsByTagName()

> 1. **语法 : let elements = element.getElementsByTagName('tagName')**
>    - **tagName : 会自动转化为小写, 不建议在 驼峰式命名的SVG元素中使用**
>    - **element ：搜索从 element 开始, 只有 element 的后代元素会被搜索, 但不包括自己**
> 2. **作用 : 获取页面元素**
> 3. **返回值 : 元素的动态HTML集合HTMLCollection, 它们顺序是在子树中出现的顺序, 如果没有搜索到则这个集合为空. (伪数组)(不建议用 for in 循环, 会遍历出其他无关属性)**

```javascript
  <ul>
    <li>知否知否, 应是等你好久</li>
    <li>知否知否, 应是等你好久</li>
    <li>知否知否, 应是等你好久</li>
  </ul>
  <ol id="ol">
    <li>傻逼</li>
    <li>傻逼</li>
  </ol>
  <script>
    // 1. 返回的是 获取过来的元素的集合 以伪数组的形式存储的
    let lis = document.getElementsByTagName('li')
    console.log(lis);
    console.log(lis[0]);
    // 2. 我们想要依次打印里面的元素对象我们可以采取遍历的方法
    for (let i = 0; i < lis.length; i++) {
      console.log(lis[i]);
    }
    // 3. 如果页面中只有一个 li, 返回的还是一个伪数组
    // 4. 如果页面中没有这个元素, 返回的还是空的伪数组的形式
    // 5. element.getElementsByTagName('标签名')
    let ol = document.getElementById('ol')
    console.log(ol.getElementsByTagName('li'));
  </script>
```

#### 通过 HTML5 新增的方法获取特殊元素获取

##### element.getElementsByClassName()

> 1. **语法 : let elements = element.getElementsByClassName('className')**
> 2. **作用 : 获取页面元素**
> 3. **返回值 : 元素的动态HTML集合HTMLCollection, 它们顺序是在子树中出现的顺序, 如果没有搜索到则这个集合为空. (伪数组)(不建议用 for in 循环, 会遍历出其他无关属性)**

##### element.querySelector()

> 1. **语法 : let element = baseElement.querySelector('selector')**
>    - **selector : 一组 CSS 选择器**
>    - **baseElement ：搜索从 baseElement 开始, 只有 baseElement 的后代元素会被搜索, 但不包括自己**
> 2. **作用 : 获取页面元素**
> 3. **返回值 : 满足条件的第一个 Element 对象, 如果不存在返回 null**

##### element.querySelectorAll()

> 1. **语法 : let elements = element.querySelectorAll('selector')**
>    - **selector : 一组 CSS 选择器**
>    - **element ：搜索从 element 开始, 只有 element 的后代元素会被搜索, 但不包括自己**
> 2. **作用 : 获取页面元素**
> 3. **返回值 : 元素的动态集合NodeList, 它们顺序是在子树中出现的顺序, 如果没有搜索到则这个集合为空. (伪数组)(不建议用 for in 循环, 会遍历出其他无关属性)**

```javascript
  <div class="box">盒子1</div>
  <div class="box">盒子2</div>
  <div id="nav">
    <ul>
      <li>首页</li>
      <li>产品</li>
    </ul>
  </div>

  <script>
    // 1. getElementsByClassName() 根据类名获取某些元素集合
    let boxs = document.getElementsByClassName('box')
    console.log(boxs);
    // 2. querySelector() 返回指定选择器的第一个元素对象
    // 3. 如果里面是 class 或者 id, 切记加符号 .box #nav
    let firstBox = document.querySelector('.box')
    console.log(firstBox);
    let nav = document.querySelector('#nav')
    console.log(nav);
    let li = document.querySelector('li')
    console.log(li);
	// 4. querySelectorAll()返回的是 获取过来的元素的集合 以伪数组的形式存储的
    let allBox = document.querySelectorAll('.box')
    console.log(allBox);
    let lis = document.querySelectorAll('li')
    console.log(lis);
  </script>
```

#### 获取特殊元素

##### 获取 body 

> **let bodyEle = document.body**

##### 获取 html

> **let htmlEle = document.documentElement**

```javascript
  <script>
    // 1. 获取 body 元素
    let bodyEle = document.body
    console.log(bodyEle)
    console.dir(bodyEle)
    // 2. 获取 html 元素
    let htmlEle = document.documentElement
    console.log(htmlEle)
    console.dir(htmlEle)
  </script>
```

### 事件基础

#### 事件三要素

> 1. 事件源 : 事件被触发的对象
> 2. 事件类型 : 通过什么事件触发
> 3. 事件处理程序 : 通过一个函数赋值的方式完成

```javascript
  <button id="btn">唐伯虎</button>

   <script>
    // 点击一个按钮弹出对话框
    // 1. 事件由三部分组成, 事件源 事件类型 事件处理程序 (事件三要素)
    //    1. 事件源: 事件被触发的对象  -> 按钮
    //    2. 事件类型: 通过什么事件触发, 鼠标点击(onclick) 鼠标经过 还是键盘按下
    //    3. 事件处理程序: 通过一个函数赋值的方式完成
    let btn = document.getElementById('btn')
    btn.onclick = function () {
      alert('点秋香')
    }
   </script>
```

#### 执行事件的步骤

> 1. 获取事件源
> 2. 注册事件(绑定事件)
> 3. 添加事件处理程序(采用函数赋值形式)

#### 常见鼠标事件

| 鼠标事件    | 触发条件         |
| ----------- | ---------------- |
| onclick     | 鼠标点击左键触发 |
| onmouseover | 鼠标经过触发     |
| onmouseout  | 鼠标离开触发     |
| onfocus     | 获得鼠标焦点触发 |
| onblur      | 失去鼠标焦点触发 |
| onmousemove | 鼠标移动触发     |
| onmouseup   | 鼠标弹起触发     |
| onmousedown | 鼠标按下触发     |

### 修改元素内容

>1. element.innerText  非标准
>2. element.innerHTML  W3C标准

#### element.innerText

> 从起始位置到终止位置的全部内容, 但它去除 html 标签, 同时空格和换行也会去掉

#### element.innerHTML

> 起始位置到终止位置的全部内容, 保留 html 标签, 同时保留空格和换行

```javascript
  <div></div>
  <div></div>
  <p>
    我是文字
    <span>123</span>
  </p>
  <script>
    // innerText 和 innerHTML 的区别
    // 1. innerText 不识别 html 标签  非标准 去除空格和换行
    let divList = document.querySelectorAll('div')
    divList[0].innerText = `<h1>innertext</h1>`
    // 2. innerHTML 识别 html 标签  W3C标准
    divList[1].innerHTML = `<h1>innertext</h1>`
    // 3. 这两个属性是可读写的, 可以获取元素里的内容
    let p = document.querySelector('p')
    console.log(p.innerText);
    console.log(p.innerHTML);
  </script>
```

### 修改元素属性

#### 普通元素

>1. innerHTML, innerText
>2. src, href
>3. id, alt, title

> 通过 . 来获取属性, 如 img.src

```javascript
  <button id="btn1">图片1</button>
  <button id="btn2">图片2</button>
  <div>
    <img src="https://s3.ax1x.com/2020/11/19/DuGA8e.jpg" alt="" width="500px" title="图片1" id="img">
  </div>

  <script>
    let btn1 = document.getElementById('btn1')
    let btn2 = document.getElementById('btn2')
    let img = document.querySelector('img')
    btn1.onclick = () => {
      img.src = 'https://s3.ax1x.com/2020/11/19/DuGA8e.jpg'
      img.title = '图片1'
    }
    btn2.onclick = () => {
      img.src = 'https://s3.ax1x.com/2020/12/30/rOhlHf.jpg'
      img.title = '图片2'
    }
  </script>
```

#### 表单元素

> 1. type, value, checked, selected, disabled

```javascript
  <button>按钮</button>
  <input type="text" value="表单内容">

  <script>
    let btn = document.querySelector('button')
    let input = document.querySelector('input')
    btn.onclick = function () {
      // 1. 表单里面的值是通过 value 来修改的
      input.value  = '表单通过value属性改变'
      // 2. 如果想要某个按钮不被点击, 设置 disabled
      this.disabled = true
      // 3. this指向的事件函数的调用者 btn
    }
  </script>
```

#### 样式属性操作

> 1. element.style   行内样式操作
> 2. element.className   类名样式操作

> 1. JS 里面的样式采用驼峰命名法, 比如 fontSize, backgroundColor
> 2. JS 修改style样式操作, 产生的是行内样式
> 3. element.className 会直接更改元素的类名, 会覆盖原先的类名

```javascript
  <div></div>

  <script>
    let div = document.querySelector('div')
    div.onclick = function () {
      // 1. JS里的样式采用驼峰命名法
      this.style.backgroundColor = 'yellow'
    }
  </script>
```

### 排他思想

> 干掉其他, 留下自己

### 获取属性值

> 1. element.属性   获取内置属性值
> 2. element.getAttribute('属性')   主要获取自定义属性值

### 设置属性值

> 1. element.属性 = '值'  设置内置属性值
> 2. element.setAttribute('属性', '值')

> 1. H5 规定自定义属性 data- 开头作为属性名并且赋值

### 移除属性值

> 1. element.removeAttribute('属性')

> 1. dataset 是一个集合, 里面存放了所有以 data- 开头的自定义属性
> 2. H5 新增获取属性 element.dataset.属性名, 或 element.dataset['属性名']
> 3. 如果自定义属性里面有多个 - 单词, 获取属性的时候必须采取驼峰命名法, 如 data-list-name, 那么获取的时候需要驼峰, element.dataset.listName, 或 element.dataset['listName']

### 节点操作

#### 节点概述

> 1. 一般地, 节点至少拥有 nodeType(节点类型), nodeName(节点名称), nodeValue(节点值)这三个基本属性
> 2. 元素节点 nodeType 为 1
> 3. 属性节点 nodeType 为 2
> 4. 文本节点 nodeType 为 3 (文本节点包括文字, 空格, 换行等)

#### 节点层级

> 1. 父级节点 : childNode.parentNode (离元素最近的父节点) 找不到返回 null
> 2. 子节点 : parentNode.childNodes (集合) (所有的子节点, 包含元素节点, 文本节点等)
> 3. 子节点 : parentNode.children
> 4. 第一个子节点 : parentNode.firstChild (不管节点类型)
> 5. 最后一个子节点 : parentNode.lastChild (不管节点类型)
> 6. 第一个元素子节点 : parentNode.firstElementChild (第一个元素节点, 但有兼容问题)
> 7. 最后一个元素子节点 : parentNode.lastElementChild (第一个元素节点, 但有兼容问题)
> 8. 获取第一个或最后一个子节点, 通常做法是利用 parentNode.children[下标]获取
> 9. 下一个兄弟节点 : node.nextSibling (不管节点类型)
> 10. 上一个兄弟节点 : node.previousSibling (不管节点类型)
> 11. 下一个元素兄弟节点 : node.nextElementSibling 
> 12. 上一个元素兄弟节点 : node.previousElementSibling
> 13. 创建节点 : document.createElement('tagName')
> 14. 添加节点 : node.appendChild(child) (放在子节点列表末尾)
> 15. 在指定节点前添加 : node.insertBefore(child, 指定元素)
> 16. 删除节点 : node.removeChild(child)
> 17. 复制节点 : node.cloneNode() (如果括号参数为空或者为false, 则是浅拷贝, 只克隆节点本身, 不可隆子节点, 括号参数为 true, 就是深拷贝, 会复制子节点)

#### 创建节点效率问题

> 1. document.write() 会页面重绘, 不要用
> 2. element.innerHTML 
> 3. document.createElement
> 4. element.innerHTML 效率高于 document.createElement 只要不拼接字符串

### 注册事件

> 1. 传统方法
>    - 注册事件的唯一性
>    - 同一个元素同一个事件只能设置一个处理函数, 最后的处理函数将会覆盖前面的处理函数
> 2. 监听注册
>    - addEventListener()
>    - 同一个元素一个事件可以设置多个监听器

#### 事件监听方式

> 1. eventTarget.addEventListener(type, listener[, useCapture])
> 2. type : 事件类型字符串, 比如 click, mouseover, 注意这里不要带 on
> 3. listener : 事件处理函数, 事件发生时, 会调用该监听函数
> 4. useCapture : 可选参数, 是一个布尔值, 默认为 false， 也就是冒泡事件流, 如果为true, 则为捕获事件流

#### 删除事件

> 1. 传统方式删除事件, 把事件处理函数设置为 null
> 2. 事件监听方式, eventTarget.removeEventListener(type, listener[, useCapture])
> 3. 采用事件监听方式的事件处理函数不能用匿名函数

### DOM事件流

> 1. 事件流描述的是从页面接收事件的顺序
> 2. 事件发生时会在元素节点之间按照特定的顺序传播, 这个传播过程即DOM事件流

![image-20210102135925969](E:\Typora插入图片时保存的图片\image-20210102135925969.png)

#### DOM事件流分为三个阶段

>1. 捕获阶段
>2. 当前目标阶段 
>3. 冒泡阶段

> 1. JS 代码中只能执行捕获或者冒泡的一个阶段
> 2. onclick 只能得到冒泡阶段
> 3. 有些事件是没有冒泡的, 比如 onblur, onfocus, onmouseleave

### 事件对象

> 1. event : 在事件处理函数的小括号里边儿
> 2. 事件对象只有有了对象才会存在
> 3. 他是系统自动创建的
> 4. 事件对象是我们事件的一系列相关数据的集合, 跟事件相关的

#### 事件对象的常见属性和方法

| 事件对象属性方法    | 说明                     |
| ------------------- | ------------------------ |
| e.target            | 返回触发事件的对象  标准 |
| e.type              | 返回事件的类型 不带 on   |
| e.preventDefault()  | 该方法阻止默认事件       |
| e.stopPropagation() | 阻止冒泡  标准           |
| e.srcElement        | 返回触发事件的对象       |
| e.cancelBubble      | 该属性阻止冒泡           |
| e.returnValue       | 该属性阻止默认事件       |

#### target 与 this 的区别

> 1. event.target 是触发事件的对象, 点击了谁就是谁
> 2. this 是绑定事件的对象, 哪个元素绑定的就是谁

#### 事件委托

> 1. 事件委托的原理 : 不是给每个子节点单独设置事件监听器, 而是将事件监听器设置在其父节点上, 然后利用冒泡原理影响每个子节点

#### 常用鼠标事件

> 1. contextmenu : 禁用右键菜单
> 2. selectstart : 禁止选中文字

#### 鼠标事件对象

> MouseEvent

| 鼠标事件对象 | 说明                                    |
| ------------ | --------------------------------------- |
| e.clientX    | 返回鼠标相对于浏览器窗口可视区的X坐标   |
| e.clientY    | 返回鼠标相对于浏览器窗口可视区的Y坐标   |
| e.pageX      | 返回鼠标相对于文档页面的X坐标, IE9+支持 |
| e.pageY      | 返回鼠标相对于文档页面的Y坐标, IE9+支持 |
| e.screenX    | 返回鼠标相对于电脑屏幕的X坐标           |
| e.screenY    | 返回鼠标相对于电脑屏幕的Y坐标           |

#### 常用键盘事件

| 键盘事件   | 触发条件                                                     |
| ---------- | ------------------------------------------------------------ |
| onkeyup    | 某个键盘按键被松开时触发                                     |
| onkeydown  | 某个键盘按键被按下时触发                                     |
| onkeypress | 某个键盘按键被按下时触发, 但他不能识别功能键, 比如 Shift, Ctrl, 箭头等 |

> 1. 三个事件执行顺序 : onkeydown  ->  onkeypress  ->  onkeyup

#### 键盘事件对象

> 1. 键盘事件 : 键盘事件对象中的 keyCode 属性可以得到相应的 ASCLL码值
> 2. keyup 和 keydown 不区分大小写, keypress 区分大小写
> 3. JS 里 focus() 可以让 input 框获取焦点

## BOM

### BOM简介

> BOM (Browser Object Model) 即浏览器对象模型, 它提供了独立于内容而与浏览器窗口进行交互的对象, 其核心对象是 window

### BOM的构成

> 1. window 对象是浏览器的顶级对象, 它具有双重角色
> 2. 它是 JS 访问浏览器窗口的一个接口
> 3. 它是一个全局对象, 定义在全局作用域中的变量, 函数都会变成 window 对象的属性和方法
> 4. 在调用的时候可以省略 window, 前面学习的对话框都属于 window 对象方法, 如 alert(), prompt()
> 5. 注意一下 window 下的一个特殊属性 window.name

### window对象常见事件

#### 窗口加载事件

> 1. window.onload 是窗口(页面)加载事件, 当文档内容完全加载完成会触发该事件(包含图像, 脚本文件, CSS文件等), 就调用的处理函数
> 2. 有了 window.onload 就可以把 JS 代码写到页面元素的上方, 因为 onload 是等页面内容全部加载完毕, 再去执行处理函数
> 3. window.onload 传统注册事件方式只能写一次, 如果有多个, 会以最后一个 window.onload为准
> 4. 如果使用 addEventListener 则没有限制

> 1. DOMContentLoaded 事件触发时, 仅当 DOM 加载完成, 不包括样式表, 图片, flash等
> 2. IE9 以上才支持
> 3. 如果页面的图片很多的话, 从用户访问到 onload 触发可能需要较长的时间, 交互效果就不能实现, 必然影响用户的体验, 此时用DOMContentLoaded事件比较合适

#### 调整窗口大小事件

> 1. window.onresize 是调整窗口大小加载事件, 当触发时就调用的处理函数
> 2. 我们常常利用这个事件完成响应式布局, window.innerWidth 当前屏幕的宽度

#### 定时器

> 1. setTimeout(调用函数, [延迟的毫秒数])
> 2. setTimeout() 方法用于设置一个定时器, 该定时器在定时器到期后执行调用函数

> 1. setInterval(回调函数, [间隔的毫秒数])
> 2. setInterval() 方法重复调用一个函数, 每隔这段时间, 就去调用一次回调函数

### this

> 1. this 的指向在函数定义的时候是确定不了的, 只有函数执行的时候才能确定 this 到底指向谁, 一般情况下 this 的最终指向的是那个调用它的对象

### JS执行机制

> 1. JS 是单线程

#### 同步和异步

##### 同步任务

> 1. 同步任务都在主线程上执行, 形成一个执行栈

##### 异步任务

> 1. JS 的异步是通过回调函数实现的
> 2. 一般而言, 异步任务有以下三种类型
>    - 普通事件 : 如 click, resize等
>    - 资源加载 : 如 load, error等
>    - 定时器 : setInterval, setTimeout等
> 3. 异步任务相关的回调函数添加到任务队列中(任务队列也叫作消息队列)

> 1. 先执行执行栈中的同步任务
> 2. 异步任务(回调函数)放入任务队列中
> 3. 一旦执行栈中的所有同步任务执行完毕, 系统就会按次序读取任务队列中的异步任务, 于是被读取的异步任务结束等待状态, 进入执行栈, 开始执行
> 4. 由于主线程不断的重复获取任务, 执行任务, 再获取任务, 再执行, 所以这种机制被称为事件循环

![image-20210106160007736](E:\Typora插入图片时保存的图片\image-20210106160007736.png)

### location 对象

#### 什么是 location 对象

> 1. window 对象给我们提供了一个 location 属性用于获取或设置窗体的 URL, 并且可以解析 URL. 因为这个属性返回的是一个对象, 所以我们将这个属性也称为 location 对象

#### URL

> 1. 统一资源定位符(Uniform Resource Locator, URL)是互联网上标准资源的地址, 互联网上的每个文件都有唯一的一个 URL, 它包含的信息指出文件的位置以及浏览器怎么处理它
> 2. URL 的语法一般为:
>    - protocol://host[:port]/path/[?query]#fragment
>    - http://www.itcast.cn/index.html?name=andy&age=18#link

| 组成     | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| protocol | 通信协议, 常用的 http, ftp, maito等                          |
| host     | 主机(域名)                                                   |
| port     | 端口号, 可选, 省略时使用方案的默认端口, 如http 的默认端口为 80 |
| path     | 路径由 0 或 多个 '/' 符号隔开的字符串, 一般用来表示主机上的一个目录或文件地址 |
| query    | 参数, 以键值对的形式, 通过 & 符号分隔开来                    |
| fragment | 片段, #后面内容 常用于链接, 锚点                             |

### location 对象的属性

| location对象属性  | 返回值                               |
| ----------------- | ------------------------------------ |
| location.href     | 获取或设置整个 URL                   |
| location.host     | 返回主机(域名)                       |
| location.port     | 返回端口号, 如果未填写返回空字符串   |
| location.pathname | 返回路径                             |
| location.search   | 返回参数                             |
| location.hash     | 返回片段, # 后面内容, 常见于连接锚点 |

#### location 对象的方法

| location对象方法   | 返回值                                                       |
| ------------------ | ------------------------------------------------------------ |
| location.assign()  | 跟 href 一样, 可以跳转页面, (也称重定向页面)                 |
| location.replace() | 替换当前页面, 因为不记录历史, 所以不能后退页面               |
| location.reload()  | 重新加载页面, 相当于刷新按钮或者 f5, 如果参数为 true, 强制刷新 ctrl + f5 |

### navigator 对象

> 1. 常用的是 userAgent, 该属性可以返回由客户机发送服务器的user-agent头部的值, 可以判断是手机还是电脑

### history 对象

> 1. window对象给我们提供了一个 history 对象, 与浏览器历史记录进行交互, 该对象包含用户(在浏览器窗口中)访问的 URL

| history对象方法 | 作用                                                         |
| --------------- | ------------------------------------------------------------ |
| back()          | 可以后退功能                                                 |
| forward()       | 前进功能                                                     |
| go(参数)        | 前进后退功能, 参数如果是 1 前进 1个页面, 如果是 -1 后退 1 个页面 |

### 元素偏移量 offset 系列

> 1. offset 概述 : offset 翻译过来就是偏移量, 我们使用 offset 系列相关的属性可以动态的得到该元素的位置(偏移), 大小等
> 2. 获得元素距离带有定位父元素的位置
> 3. 获得元素自身的大小(宽度高度)
> 4. 注意 : 返回的数值都不带单位

#### offset 系列常用属性

| offset 系列属性      | 作用                                                         |
| -------------------- | ------------------------------------------------------------ |
| element.offsetParent | 返回作为该元素带有定位的父级元素 如果父级元素都没有定位则返回 body |
| element.offsetTop    | 返回元素相对带有定位父元素上方的偏移                         |
| element.offsetLeft   | 返回元素相对带有定位父元素左边框的偏移                       |
| element.offsetWidth  | 返回自身包括 padding, 边框, 内容区的宽度, 返回数值不带单位   |
| element.offsetHeight | 返回自身包括 padding, 边框, 内容区的高度, 返回数值不带单位   |

#### offset 与 style 区别

##### offset

> 1. offset 可以得到任意样式表中的样式值
> 2. offset 系列获得的数值是没有单位的
> 3. offsetWidth 包含 padding + border + width
> 4. offsetWidth 等属性是只读属性, 只能获取不能赋值
> 5. 所以, 我们想要获取元素大小位置, 用 offset 更合适

##### style

> 1. style 只能得到行内样式表中的样式值
> 2. style.width 获得的是带有单位的字符串
> 3. style.width 获得不包含 padding 和 border 的值
> 4. style.width 是可读写属性, 可以获取也可以赋值
> 5. 所以, 我们想要给元素更改值, 则需要用 style 改变

### 元素可视区 client 系列

> client 翻译过来就是客户端, 我们使用 client 系列的相关属性来获取元素可视区的相关信息, 通过 client 系列的相关属性可以动态的得到该元素的边框大小, 元素大小等

| client 系列属性      | 作用                                                         |
| -------------------- | ------------------------------------------------------------ |
| element.clientTop    | 返回元素上边框的大小                                         |
| element.clientLeft   | 返回元素左边框的大小                                         |
| element.clientWidth  | 返回自身包括 padding, 内容区的宽度, 不含边框, 返回数值不带单位 |
| element.clientHeight | 返回自身包括 padding, 内容区的高度, 不含边框, 返回数值不带单位 |

### 物理像素比

> 1. window.devicePixelRatio   物理像素比

### 元素滚动scroll系列

> scroll 翻译过来就是滚动的, 我们使用 scroll 系列的相关属性可以动态的得到该元素的大小, 滚动距离等

| scroll系列属性       | 作用                                           |
| -------------------- | ---------------------------------------------- |
| element.scrollTop    | 返回被卷去的上侧距离, 返回数值不带单位         |
| element.scrollLeft   | 返回被卷去的左侧距离, 返回数值不带单位         |
| element.scrollWidth  | 返回自身实际的宽度, 不含边框, 返回数值不带单位 |
| element.scrollHeight | 返回自身实际的高度, 不含边框, 返回数值不带单位 |

> 1. 页面被卷去的头部 : 可以通过 window.pageYOffset 获得, 如果是被卷去的左侧 window.pageXOffset
> 2. 元素被卷去的头部是 element.scrollTop

### mouseenter 和 mouseover 的区别

#### mouseenter

> 1. 当鼠标移动到元素上时就会触发 mouseenter 事件
> 2. 类似 mouseover, 他们两者之间的区别是
> 3. mouseover 鼠标经过自身盒子会触发, 经过子盒子还会触发, mouseenter 只会经过自身盒子触发
> 4. 之所以这样, 就是因为 mouseenter 不会冒泡
> 5. 跟 mouseenter 搭配鼠标离开 mouseleave 同样不会冒泡

### 动画实现原理

#### 核心原理

> 1. 通过定时器 setInterval() 不断移动盒子位置

#### 实现步骤

> 1. 获得盒子当前位置
> 2. 让盒子在当前位置加上1个移动距离
> 3. 利用定时器不断重复这个操作
> 4. 加一个结束定时器条件
> 5. 注意此元素需要添加定位, 才能使用 element.style.left