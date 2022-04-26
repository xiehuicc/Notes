### 1.数据类型

#### 1.数值

#### 2.null,undefined和布尔值

#### 3.字符串

#### 4.对象

> 简单说，对象就是一组“键值对”（key-value）的集合，是一种无序的复合数据集合。

**键名**

> 对象的每一个键名又称为“属性”，它的“键值”可以是任何数据类型。如果一个属性的值为函数，通常把这个属性称为“方法”，它可以像函数那样调用。

```js
var obj = {
  p: function (x) {
    return 2 * x;
  }
};

obj.p(1) // 2
```



如果属性的值还是一个对象，就形成了链式引用

```js
var o1 = {};
var o2 = { bar: 'hello' };

o1.foo = o2;
o1.foo.bar // "hello"
```

**对象的引用**



**2.属性的操作**

1.**属性的读取**

- 使用点运算符
- 使用方括号运算符，键名必须放在引号里面（数字键可以不加引号，因为会自动转为字符串）

```js
var obj = {
  p: 'Hello World'
};

obj.p // "Hello World"
obj['p'] // "Hello World"
```



2.**属性的查看**  	Object.keys 

查看一个对象本身的所有属性



3.**属性的删除**：delete命令

delete命令用于删除对象的属性，删除成功会返回true

```js
var obj = { p: 1 };
Object.keys(obj) // ["p"]

delete obj.p // true
obj.p // undefined
Object.keys(obj) // []
```



4.**判断属性是否存在**

- `for...in`遍历对象,如果存在则返回true，否则返回false
- 2.利用JSON自带的`JSON.stringify()`方法来判断，大概思路就是转化为字符串'{}'来进行判断

```kotlin
if (JSON.stringify(obj) === '{}') {
    return true;
}
```

- 利用ES6中`Object.keys()`来进行判断 （推荐）

  `Object.keys()`方法会返回一个由一个给定对象的自身可枚举属性组成的数组。
  如果我们的对象为空，他会返回一个空数组。

  ```livecodeserver
  Object.keys(obj).length === 0 ? '空' : '不为空'
  ```

5.**属性的遍历：for...in 循环**

`for...in`循环用来遍历一个对象的全部属性

```js
var obj = {a: 1, b: 2, c: 3};

for (var i in obj) {
  console.log('键名：', i);
  console.log('键值：', obj[i]);
}
// 键名： a
// 键值： 1
// 键名： b
// 键值： 2
// 键名： c
// 键值： 3
```

`for...in`循环有两个	使用注意点

- 它遍历的是对象所有可遍历的属性，会跳过不可遍历的属性

  （toString属性默认是不可遍历的）

- 它不仅遍历对象自身的属性，还遍历继承的属性

**with语句**

`with`语句的格式：

```js
with(对象) {
	语句；
}
```



> 它的作用是操作同一个对象的多个属性时，提供一些书写的方便

```js
// 例一
var obj = {
  p1: 1,
  p2: 2,
};
with (obj) {
  p1 = 4;
  p2 = 5;
}
// 等同于
obj.p1 = 4;
obj.p2 = 5;

// 例二
with (document.links[0]){
  console.log(href);
  console.log(title);
  console.log(style);
}
// 等同于
console.log(document.links[0].href);
console.log(document.links[0].title);
console.log(document.links[0].style);
```

注意：如果`with`区块内部有变量赋值操作，必须是当前对象已经存在的属性，否则会创造一个当前作用域的全局变量。

#### 5.函数

**1.4第一等公民**

> JavaScript语言将函数看作一种值，与其他值（数值，字符串，布尔值）地位相同。凡是可以使用值的地方，就能使用函数。
>
> JavaScript语言中又称函数为第一等公民

```js
function add(x, y) {
  return x + y;
}

// 将函数赋值给一个变量
var operator = add;

// 将函数作为参数和返回值
function a(op){
  return op;
}
a(add)(1, 1)
// 2
```



**闭包**

> 正常情况下，函数外部无法读取到函数内部声明的变量
>
> 如果出于种种原因，需要得到函数内的局部变量。正常情况下，这是办不到的，值用通过在函数的内部，再定义一个函数。

```js
function f1() {
  var n = 999;
  function f2() {
    console.log(n);
  }
  return f2;
}

var result = f1();
result(); // 999
```

闭包就是函数`f2`,即能够读取其他函数内部变量的函数。

闭包的两个最大的用处：

- 可以读取外层函数内部的变量
- 让这些变量始终保持在内存中，即闭包可以使得它诞生环境一直存在

```js
//闭包使得内部变量记住上一次调用的运算结果
function createIncrementor(start) {
  return function () {
    return start++;
  };
}

var inc = createIncrementor(5);

inc() // 5
inc() // 6
inc() // 7
```

为什么闭包能够返回函数的内部变量？

------  原因是闭包(上例的`inc`)用到了外层变量(`start`)，导致外层函数(`crateIncreamentor`)不能从内存释放。只要比u包没有被垃圾回收机制清除，外层函数提供的运行环境也不会被清除。

#### 6.数组



### 2.标准库

#### 1.Object对象

**（1）Object对象本身的方法**

所谓“本身的方法” 就是直接定义再Object对象的方法

```js
Object.print = function(o) {
	console.log(o)
}
// print方法就是直接定义在Object对象上
```

**(2)object的实例方法**

所谓实例方法就是定义在Object原型对象`Object.prototype`上的方法。它可以被Object实例直接使用

```js
Object.prototype.print = function () {
	console.log(this)
}
var obj = new Object()
obj.print()	//Object
```

> 凡是定义在`Object.prototype`对象上面的属性和方法，将被所有实例对象共享

**2.Object()**

> Object本身是一个函数，可以当工具方法使用，将任意值转为对象。这个用于保证某一个值一定是对象
>
> 如果参数为空（或者undefined和null），Object()返回一个空对象

**3.Object构造函数**



**4.Object的静态用法**

**（1）Object.keys(),Object.getOwnPropertyNames()**

两个都是用来遍历对象的属性。

Object.keys()方法的参数是一个对象，返回一个数组。

```js
var obj = {
	p1:123,
    p2:456
}
Object.keys(obj) // ["p1","p2"]

```

Object.getOwnPropertyNames()方法与其类似返回结果是一样的。只有涉及不可枚举属性时，才会有不一样的结果

**5,Object的实例方法**



#### 2.Array对象

**new Set()**

用来对数组进行去重。

```JavaScript
const arr = [3,4,4,5,4,6,5,7];
console.log(new Set(arr)); // {3,4,5,6,7}
const a = Array.from(new Set(arr)) // [3, 4, 5, 6, 7]
复制代码
```

**sort()**

对数组元素进行排序（改变原数组）。

```JavaScript
const arr = [3,4,4,5,4,6,5,7];
console.log(arr.sort()) // [3, 4, 4, 4, 5, 5, 6, 7]
复制代码
```

**reverse()**

反转数组中的元素（改变原数组）。

```JavaScript
const arr = [3,4,4,5,4,6,5,7];
conosle.log(arr.reverse());  // [7, 6, 5, 5, 4, 4, 4, 3]
复制代码
```

**delete()**

删除一个数组成员，会形成空位，并不会影响length属性（改变原数组）,同样适用于对象。

```JavaScript
//数组
const arr = [3,4,4,5,4,6,5,7];
delete arr[1];
conosle.log(arr); // [3, empty, 4, 5, 4, 6, 5, 7]
//对象
const obj = {name: 'pboebe',age: '23',sex: '女'};
delete obj.sex;
console.log(obj); // {name: "pboebe", age: "23"}
复制代码
```

**shift()**

把数组的第一个元素从其中删除，并返回第一个元素的值（改变原数组）。

```JavaScript
const arr = [3,4,4,5,4,6,5,7];
const a = arr.shift(); // 3
console.log(arr); // [empty, 4, 5, 4, 6, 5, 7]
复制代码
```

**unshift()**

向数组的起始处添加一个或多个元素,并返回新的长度（改变原数组）。

```JavaScript
const arr = [3,4,4,5,4,6,5,7];
const a = arr.unshift(8);
console.log(a); // 9(a为返回的数组的新长度)
console.log(arr); // [8, 3, 4, 4, 5, 4, 6, 5, 7]
复制代码
```

**push()**

在数组的末端添加一个或多个元素，并返回添加新元素后的数组长度（改变原数组）。

```JavaScript
const arr = [3,4,4,5,4,6,5,7];
const a = arr.push(8,9);
console.log(a); // 10(a为返回的数组的新长度)
console.log(arr); // [3, 4, 4, 5, 4, 6, 5, 7, 8, 9]
复制代码
```

**valueOf()**

返回数组本身。

```JavaScript
const arr = [3,4,4,5,4,6,5,7];
console.log(arr.valueOf()); // [3,4,4,5,4,6,5,7]
复制代码
```

**toString()**

可把值转换成字符串。

```JavaScript
const arr = [3,4,4,5,4,6,5,7];
console.log(arr.toString()); // 3,4,4,5,4,6,5,7
复制代码
```

**concat()**

在原始数据尾部添加另外数据组成新数据（字符串适用）。

```JavaScript
//数组
const a = [1,2,3];
const b = [4,5];
const c = a.concat(b); // [1, 2, 3, 4, 5]
//字符串
const x = 'abc';
const y = 'def';
const z = x.concat(y); // abcdef
复制代码
```

**join()**

以参数作为分隔符，将所有参数组成一个字符串返回（一般默认逗号隔开）。

```JavaScript
const arr = [3,4,4,5,4,6,5,7];
console.log(arr.join('-')); // 3-4-4-5-4-6-5-7
复制代码
```

**slice(start, end)**

用于提取原来数组的一部分，会返回一个提取的新数组，**原数组不变**（**字符串适用**,不包括end)。

```JavaScript
//数组
const arr = [3,4,4,5,4,6,5,7];
const a = arr.slice(2, 5); // [4, 5, 4]
//字符串
const x = 'abcdefgh';
const y = x.slice(3, 6); // def
复制代码
```

**splice()**

用于删除**原数组**的一部分，并且可以在删除的位置添加新的数组成员，返回值是被删除的数组元素。（**改变原数组**）
 splice（t, v, s）t:被删除元素的起始位置；v：被删除元素个数；s:s以及后面的元素为被插入的新元素。

```JavaScript
const arr = [3,4,4,5,4,6,5,7];
const a = arr.splice(3, 2, 12); // [5, 4]
console.log(arr); // [3, 4, 4, 12, 6, 5, 7]
```

**map()**

依次遍历数组成员，根据遍历结果返回一个新数组。（map方法同样适用于字符串，但是不能直接调用，需要通过函数的call方法，间接使用，或者先将字符串川转为数组，再使用）（不会改变原始数组）。

```JavaScript
const arr = [3,4,4,5,4,6,5,7];
const a = arr.map(item => item*2;) // [6, 8, 8, 10, 8, 12, 10, 14]
const b = arr.map((item,index,arr) =>{ // item代表数组下的每一个对象
    								  // index代表每个元素的下标
									 // arr代表原数组
})
```

**forEach()**

跟map方法类似，遍历数组，区别是无返回值。

```JavaScript
const arr = [3,4,4,5,4,6,5,7];
arr.forEach(function(value,index,arr){console.log(value)}))
```

**for in()**

跟 map 方法类似，遍历对象或者数组。
但值得注意的是 for in 循环返回的值都是数据结构的 键值名 。遍历对象返回的对象的key值,

遍历数组返回的数组的下标(key)。

```JavaScript
// 对象
const obj = {a: 123, b: 12, c: 2 };
for (let a in obj) {
	console.log(a)
}
// a	b	c
//数组
const arr = [3,4,4,5];
for(let a in arr) {
	console.log(a)
}
// 0	1	2	3
```

**filter()**

一个过滤方法，==参数是一个函数==，所有的数组成员依次执行该函数，返回结果为 true 的成员组成一个新数组返回。（不会改变原始数组）。

```js
const arr = [3,4,4,5,4,6,5,7];
const a = arr.filter(item => item % 3 > 1);
console.log(a); // [5, 5]
```

**some()&every()**

这两个方法类似于“断言”（ assert ），用来判断数组成员是否符合某种条件。
some方法是只要==有一个==数组成员的返回值为true，则返回true，否则false；
every方法是需要==每一个==返回值为true，才能返回true，否则为false;

```js
const arr = [3,4,4,5,4,6,5,7];
console.log( arr.some( function( item, index, array ){
	console.log( 'item=' + item + ',index='+index+',array='+array );
	return item > 3;
}));
//  item=3,index=0,array=3,4,4,5,4,6,5,7
//   item=4,index=1,array=3,4,4,5,4,6,5,7
//   true
console.log( arr.every( function( item, index, array ){
	console.log( 'item=' + item + ',index='+index+',array='+array );
	return item > 3;
}));
// item=3,index=0,array=3,4,4,5,4,6,5,7
//false

```

**reduce()**

依次处理数组的每个成员，最终累计成一个值。
 a:必填，累计变量；b：必填，当前变量；x: 可选，当前位置；y:可选，原数组。

```JavaScript
// reduce(a, b, x, y)
// 简单用法
const arr = [3,4,4,5,4,6,5,7];
const a = arr.reduce((pre, cur) => {return pre+cur})
// 逗号写法
const a = arr.reduce((pre, cur) => (sum= pre+cur, sum))
console.log（a） // 38  第一次执行：pre为第一个	成员3，cur为第二个成员为4
//高级用法（举个数组去重和数组扁平化栗子）
const b = arr.reduce((pre, cur) => {
	if(!pre.includes(cur)) {
		return pre.concat(cur)
    } else {
		return pre
    }
}, [])
// [3, 4, 5, 6, 7]
const arrs = [[2,3,2], [3,4,5]]
const c = arr.reduce((pre, cur) => {
	return pre.concat(cur)
}, [])
// [2, 3, 2, 3, 4, 5]
```

**reduceRight()**

与 reduce 方法使用方式相同，区别在于 reduceRight 方法从右到左执行（例子略过）。

```js
let numbers = [65, 44, 12, 4];
 
function getSum(total, num) {
    return total + num;
}
function myFunction(item) {
    document.getElementById("demo").innerHTML = numbers.reduceRight(getSum);
}

```

**indexOf()**

返回给定元素在数组中的第一次出现的位置，如果没有则返回-1(同样适用于字符串)。

```js
//数组
const arr = [3,4,4,5,4,6,5,7];
console.log(arr.indexOf(4)) // 1
console.log(arr.indexOf('4'))  // -1
//字符串
conststring = 'asdfghj';
console.log(string.indexOf('a')) // 0

```

**flatten()**

简写为flat（），接收一个数组，无论这个数组里嵌套了多少个数组，flatten最后都会把其变成一个一维数组(扁平化)。

```js
const arr = [[1,2,3],[4,5,[6,7]]];
const a = arr.flatten(3);
console.log(a); // [1, 2, 3, 4, 5, 6, 7]
```



#### 3.JSON对象

**JSON.parse()**

用于把字符串转化为对象

```js
const str = '{"name": "phoebe", "age": 20}';
const obj = JSON.parse(str)  // {name: "phoebe", age: 20}（object类型）
```

**JSON.stringify()**

用于把对象转为字符串

```js
const obj = {"name": "Tins", "age": 22};
const str = JSON.stringify(obj)  // {"name":"Tins","age":22}(string类型)
```



### 3.异步操作



### 4.BOM

#### 4.1  window 对象

> BOM的核心是window对象，表示浏览器的实例。window 对象在浏览器中有两重身份，一个是 ECMAScript 中的 Global 对象，另一个就是浏览器窗口的 JavaScript 接口。这意味着网页中定义的所有 对象、变量和函数都以 window 作为其 Global 对象，都可以访问其上定义的 parseInt()等全局方法

#### 4.1.1 Global 作用域

> 因为window 对象被复用为ECMAScript的Global对象，所以通过var 声明的所有全局变量和函数都会变成window对象的属性和方法。

#### 4.1.2 窗口关系

> top 对象始终指向最上层窗口（即浏览器窗口本身），而parent对象则始终指向当前窗口的父窗口。
>
> 当前窗口为最外层时，则parent等于top（都等于window）

#### 4.1.3 窗口位置与像素比

```js
// moveTo()接收要移动到的新位置的绝对坐标 x 和 y
// 把窗口移动到左上角
window.moveTo(0,0)

// moveTo()接收要移动到的新位置的绝对坐标 x 和 y
// 把窗口向下移动100像素
window.moveBy(0，100)
```

#### 4.2 location 对象

> 提供了当前窗口加载文档的信息，以及通常的导航功能。
>
> location既是window属性，也是document的属性，即 window.location和document.location指向同一个对象。

https://jingyan.baidu.com/article/597a06431ed6c3312a52434d.html

### 5.DOM

> DOM时JavaScript操作网页的接口，全称为“文档对象模型”，
>
> 作用是将网页转为一个JavaScript对象，从而可以用脚本执行各种操作（比如增删内容）

#### 1.节点

> dom 的最小组成单位叫节点(node)。文档的树形结构（DOM 树），就是由各种不同类型的节点组成。

节点的类型有七种。

- `Document`:整个文档树的顶层节点
- `DocumentType`：`doctype`标签（比如`<!DOCTYPE html>`）
- `Element`：网页的各种HTML标签（比如`<body>`、`<a>`等）
- `Attr`：网页元素的属性（比如`class="right"`）
- `Text`：标签之间或标签包含的文本
- `Comment`：注释
- `DocumentFragment`：文档片段

#### 2.节点树

浏览器原生提供`document`节点，代表整个文档

除了根节点，其他节点都有三种层级关系

- 父节点关系(parentNode)：直接的那个上级节点
- 子节点关系(childNodes)：直接的下级节点
- 同级节点关系(sibling)：拥有同一个父节点的节点



#### Node接口

**属性**

**1.Node.prototype.nodeType**

`nodeType`属性返回一个整数值，表示节点的类型。

```js
document.nodeType // 9
//文档节点的类型值为9
```

**2.Node.prototype.nodeName**

`nodeName`属性返回节点的名称

```js
// HTML 代码如下
// <div id="d1">hello world</div>
var div = document.getElementById('d1');
div.nodeName // "DIV"
```

**3.Node.prototype.nodeValue**

`nodeValue`属性返回一个字符串，表示当前节点本身的文本值，该属性可读写

只有文本节点(text)、注释节点（comment）和属性节点（attr）有文本值

```js
// HTML 代码如下
// <div id="d1">hello world</div>
var div = document.getElementById('d1');
div.nodeValue // null
div.firstChild.nodeValue // "hello world"
//div是元素节点，nodeValue属性返回null。div.firstChild是文本节点，所以可以返回文本值。
```

**4.Node.prototype.textContent**

`textContent`属性返回当前节点和它的所有后代节点的文本内容

`textContent`属性自动忽略当前节点内部的 HTML标签，返回所有文本内容

```js
// HTML 代码为
// <div id="divA">This is <span>some</span> text</div>

document.getElementById('divA').textContent
// This is some text
```

**5.Node.prototype.baseURI**

`baseURI`属性返回一个字符串，表示当前网页的绝对路径。浏览器根据这个属性，计算网页的相对路径的URL。该属性为只读

```js
document.baseURI
```

该属性的值一般由当前网址的URL(即window.location属性)决定



**6.Node.prototype.ownerDocument**

`Node.ownerDocument`属性返回当前节点所在的顶层文档对象，即`document`对象。

**7.Node.prototype.nextSibling**

`Node.nextSibling`属性返回紧跟在当前节点后面的第一个同级节点。如果当前节点后面没有同级节点，则返回`null`。

```js
// HTML 代码如下
// <div id="d1">hello</div>
// <div id="d2">world</div>
var d1 = document.getElementById('d1');
var d2 = document.getElementById('d2');

d1.nextSibling === d2 // true
```

`nextSibling`属性可以用来遍历所有子节点。

```js
//遍历div1节点的所有子节点
var el = document.getElementById('div1').firstChild;

while (el !== null) {
  console.log(el.nodeName);
  el = el.nextSibling;
}
```

**8.Node.prototype.previousSibling**

`previousSibling`属性返回当前节点前面的、距离最近的一个同级节点。如果当前节点前面没有同级节点，则返回`null`。

```js
// HTML 代码如下
// <div id="d1">hello</div><div id="d2">world</div>
var d1 = document.getElementById('d1');
var d2 = document.getElementById('d2');

d2.previousSibling === d1 // true
//d2.previousSibling就是d2前面的同级节点d1。
```

**9.Node.prototype.parentNode**

`parentNode`属性返回当前节点的父节点。对于一个节点来说，它的父节点只可能是三种类型：元素节点(element)、文档节点（document）和文档片段节点（documentfragment）

```js
if (node.parentNode) {
  node.parentNode.removeChild(node);
}
//通过node.parentNode属性将node节点从文档里面移除。
```

**10.Node.prototype.parentElement**

`parentElement`属性返回当前节点的父元素节点。如果当前节点没有父节点，或者父节点类型不是元素节点，则返回`null`。

```js
if (node.parentElement) {
  node.parentElement.style.color = 'red';
}
```

**11.Node.prototype.firstChild,Node.prototype.lastChild**

`firstChild`属性返回当前节点的第一个子节点，如果当前节点没有子节点，则返回`null`

```js
// HTML 代码如下
// <p id="p1"><span>First span</span></p>
var p1 = document.getElementById('p1');
p1.firstChild.nodeName // "SPAN"
//p元素的第一个子节点是span元素。
```

`lastChild`属性返回当前节点的最后一个子节点，如果当前节点没有子节点，则返回`null`。用法与`firstChild`属性相同。

**12.Node.prototype.childNodes**

`childNodes`属性返回一个类似数组的对象（`NodeList`集合），成员包括当前节点的所有子节点。

```js
var children = document.querySelector('ul').childNodes;
//children就是ul元素的所有子节点
```

使用该属性，可以遍历某个节点的所有子节点。

```js
var div = document.getElementById('div1');
var children = div.childNodes;

for (var i = 0; i < children.length; i++) {
  // ...
}
```

**13.Node.prototype.isConnected**

`isConnected`属性返回一个布尔值，表示当前节点是否在文档之中



**方法**

**1.Node.prototype.appendChild()**

`appendChild()`方法接受一个节点对象作为参数，将其作为最后一个子节点，插入当前节点。该方法的返回值就是插入文档的子节点

```js
var p = document.createElement('p');
document.body.appendChild(p);
//上面代码新建一个<p>节点，将其插入document.body的尾部。
```

**2.Node.prototype.hasChildNodes()**

`hasChildNodes`方法返回一个布尔值，表示当前节点是否有子节点

```js
var foo = document.getElementById('foo');

if (foo.hasChildNodes()) {
  foo.removeChild(foo.childNodes[0]);
}
//上面代码表示，如果foo节点有子节点，就移除第一个子节点。
```

注意，子节点包括所有类型的节点，并不仅仅是元素节点。哪怕节点只包含一个空格，`hasChildNodes`方法也会返回`true`

判断一个节点有没有子节点，有许多种方法，

- node.hasChildNodes()
- node.firstChild !== null
- node.childNodes && node.childNodes.length > 0

`hasChildNodes`方法结合`firstChild`属性和`nextSibling`属性，可以遍历当前节点的所有后代节点。

```js
function DOMComb(parent, callback) {
  if (parent.hasChildNodes()) {
    for (var node = parent.firstChild; node; node = node.nextSibling) {
      DOMComb(node, callback);
    }
  }
  callback(parent);
}

// 用法
DOMComb(document.body, console.log)
```

**3.Node.prototype.cloneNode()**

`cloneNode`方法用于克隆一个节点。它接受一个布尔值作为参数，表示是否同时克隆子节点。它的返回值是一个克隆出来的新节点。

### 6.事件

> DOM的事件操作（监听和触发），都定义在`EventTarget`接口。

该接口主要提供三个实例方法

- `addEventListener`:绑定事件的监听函数
- `removeEventListener`:移除事件的监听函数
- `dispatchEvent`：触发事件

#### 1.EventTarget.addEventListener()

`EventTarget.addEventListener()`用于在==当前节点或对象上==，定义一个特定事件的监听函数。一旦这个事件发生，就会执行监听函数。该方法没有返回值。

```js
target.addEventListener(type, listener[, useCapture]);
```

- type：事件名称，大小写敏感
- listener：监听函数。事件发生时，会调用该监听函数
- useCapture：布尔值，表示监听函数是否在捕获阶段（capture）触发，默认为flase

```js
function hello() {
  console.log('Hello world');
}

var button = document.getElementById('btn');
button.addEventListener('click', hello, false);
//上面代码中，button节点的addEventListener方法绑定click事件的监听函数hello，该函数只在冒泡阶段触发。
```

