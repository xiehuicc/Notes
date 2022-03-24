### 0.开发环境配置

#### 1.babel

> ES6提供了许多新特性，但并不是所有的浏览器都能够完美支持。
>
> 其中[Babel](https://babeljs.io/)是一个广泛使用的转码器，可以将ES6代码转为ES5代码，从而在现有环境执行。

#### 2.webpack

>  Webpack的工作方式是：把你的项目当做一个整体，通过一个给定的主文件（如：index.js），Webpack将从这个文件开始找到你的项目的所有依赖文件，使用loaders处理它们，最后打包为一个（或多个）浏览器可识别的JavaScript文件。

### 1.面向对象编程

#### 1）面向过程编程POP(process-oriented programming)

> 面向过程就是分析出解决问题所需的步骤，然后用函数把这些步骤一步一步实现，使用的时候再一个一个的一次调用就可以了

==面向过程：==

- 优点：性能比面向对象高，适合跟硬件联系很机密的东西，例如单片机就是采用面向过程编程
- 缺点：没有面向对象易维护，易复用，易扩展

#### 2)面向对象标称OOP(Object oriented programming)

> 面向对象 是把事务分解成一个个对象，然后由对象之间分工合作

举个例子：将大象装进冰箱

先找出对象，并写出这些对象的功能

1.大象对象

- 进去

2.冰箱对象

- 打开
- 关闭

3.使用大象和冰箱的功能

**面对对象是以对象功能来划分问题的，而不是步骤**

==面向对象编程==

优点：

>再面向对象程序开发的思想中，每一个对象都是功能中心，具有明确分工
>
>面向对象编程具有灵活，代码可复用，容易维护和开发的优点，更适合多人合作的大型项目，可以设计出低耦合的系统，是系统更加灵活，易于维护

缺点：性能比面向过程低

==面向对象的特性==

- 封装性
- 继承性
- 多态性



### 2.类和对象

#### 1）对象由属性和方法组成

- 属性：事务的特征，再对象中用属性来表示
- 方法：事务的行为，再对象中用方法来表示

#### 2）类class

> 类抽像了对象的公共部分，它泛指某一大类
>
> 对象特指某一个，通过类实例化一个具体的对象

#### 3）创建类

```javascript
<script>
	 class Star {
         //类的共同属性放到constructor里面
         constructor(name,age) {
             this.name = name;
             this.age = age;
         }
         //类里面创建方法
         sing(){
             console.log("我唱歌")
         }
     }   
	var ldh = new Star('刘德华',18)
    console.log(ldh)
	 ldh.sing("冰雨")
</script>
```

#### 4)类的继承

==super关键字==用于访问和调用对象父类的函数，可以调用父类的构造函数，也可以调用==父类的普通函数==

```javascript
class Father {
    constructor(x,y){
        this.x = x
        this.y = y
    }
    sum(){
        console.log(this.x + this.y)
    }
    say(){
		return '我是爸吧'
    }
}
class Son extends Father {
    constructor(x,y) {
        //调用了父类中的构造函数
        super(x,y)
        //super.say() 就是调用父类中的普通函数say()
    }
}
var son = new Son(1,2)
son.sum()
```

如果字类的构造函数中由this的话，子类在构造函数中使用super，必须放到this前面(必须先调用父类的构造函数，再使用字类构造方法)

```javascript
class Son extends Father {
    super(x,y)
    this.x = x
    this.y = y
}
```



#### 5）三个注意点

1. 在ES6中类没有变量提升，所以必须先定义类，才能通过类实例化对象
2. 类里面的共有属性和方法一定要加this使用
3. 类里面的this指向问题
4. **constructor里面的this指向实例化对象，方法里面的this指向这个方法的调用者**（谁调用的就指向谁）

### 3. let 和const命令

### 4.变量的解构赋值

#### 1）数组的解构赋值

```javascript
 let [a,b,c] = [1,2,3]
```

#### 2）对象的解构赋值

```javascript
let {foo,bar} = {foo:'aaa',bar:'bbb'}
foo // "aaa"
bar //"bbb"
```

> 对象的解构赋值和数组有一个重要的不同：
>
> 数组的元素是按次序排列的，变量的取值由它的位置决定的；
>
> 而对象的属性没有次序，变量必须与属性同名，才能取到正确的值

```javascript
let {bar,foo} = {foo:'aaa',bar:'bbb'}
foo // "aaa"
bar //"bbb"
```

对象的解构赋值其实是下面形式的简写

```javascript
let {foo:foo,bar:bar} = {foo;'aaa',bar:'bbb'}
```

其中第一个`foo`是`匹配模式`，模式不会被赋值；第二个foo是`变量`

#### 3)字符串的解构赋值

```javascript
const [a,b,c,d,e] = 'hello'; 
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"
```

类似数组的对象和都有一个`length`属性，因此还可以对这个属性解构赋值

```javascript
let {length :len} = 'hello'
len // 5
```

#### 4)数组和布尔值的解构赋值

解构赋值时，如果等号右边是数值和布尔值，则会先转为对象

```javascript
let {toString: s} = 123;
s === Number.prototype.toString // true

let {toString: s} = true;
s === Boolean.prototype.toString // true
```

#### 5)函数参数的解构赋值

```javascript
function add([x, y]){
  return x + y;
}

add([1, 2]); // 3
```

```javascript
[[1, 2], [3, 4]].map(([a, b]) => a + b);
// [ 3, 7 ]
```

#### 6)圆括号问题

建议不要再模式中放置圆括号

#### 7)用途

- 交换变量值
- 从函数返回多个值
- 函数参数的定义
- 提取json数据
- 函数参数的默认值
- 遍历Map解构

```javascript
const map = new Map();
map.set('first', 'hello');
map.set('second', 'world');

for (let [key, value] of map) {
  console.log(key + " is " + value);
}
// first is hello
// second is world
```

如果只想获取键名，或者只想获取键值，可以写成下面这样

```javascript
// 获取键名
for (let [key] of map) {
  // ...
}

// 获取键值
for (let [,value] of map) {
  // ...
}
```

#### 7）将数组转化为对象

```js
const points = [
  [4,5],
  [10,1],
  [0,40]
];
//期望得到的数据格式如下，如何实现？
// [
//   {x:4,y:5},
//   {x:10,y:1},
//   {x:0,y:40}
// ]
let newPoints = points.map(pair => {
  const [x,y] = pair;
  return {x,y}
})
//还可以通过以下办法，更为简便
let newPoints = points.map(([x,y]) => {
  return {x,y}
})
console.log(newPoints);

```

**混合解构**

```js
const people = [
  {name:"Henry",age:20},
  {name:"Bucky",age:25},
  {name:"Emily",age:30}
];
//es5 写法 
var age = people[0].age;
console.log(age);
//es6 解构
const [a] = people; // 解构数组中的第一个项
console.log(a);//第一次解构数组 {name:"Henry",age:20}
cosnt [a,,c] = people
console.log(a,c) // {name:"Henry",age:20}  {name:"Emily",age:30}
const [{age}] = people;//再一次解构对象
console.log(age);//20

```



#### 8)输入模块的指定方法

```javascript
const {SourceMapConsumer,SourceNode} = require("source-map")
```

### 5.字符串的新增方法

- 模板字符串``(反引号)，它可以当作普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。**模板字符串嵌入变量和函数**，需要将变量名写在==${}==之中

```js
let name = "Henry";
function makeUppercase(word){
  return word.toUpperCase();
}
let template = 
  `
  <h1>${makeUppercase('Hello')}, ${name}!</h1>//可以存放函数和变量
  <p>感谢大家收看我们的视频, ES6为我们提供了很多遍历好用的方法和语法!</p>
  <ul>
    <li>1</li>
    <li>2</li>
  </ul>
  `;
document.getElementById('template').innerHTML = template;
```



再举个例子，工作中常用到ElementUI库，在自定义一个弹出框时，使用模板字符串就很方便:

```js
   await this.$alert(
          `<p><strong>确认是否升级${
            this.lectureName
          }</strong><br>(若已存在讲义套件，升级后请重新生成)</p>`,
          {
            dangerouslyUseHTMLString: true
          }
        )
```

------



### 6.正则表达式

### 7.数值的扩展

#### 1）二进制和八进制的表示法

二进制：0b(或0B)   

八进制：0o(或0O)

```javascript
0b111110111 === 503 // true
0o767 === 503 // true
```

#### 2)Number对象的方法

- Number.isFinite()：检查数值是否为有限

- Number.isNaN()：检查一个值是否为NaN

- Number.parseInt()：转化为int型
- Number.parseFloat()：转化为字符串
- Number.isInteger()：判断一个数值是否为整数

### 8.函数的扩展

#### 1.箭头函数的使用和this指向

传统函数：

> ```
> var aaa = function (V) {
> 	return v
> }
> ```

箭头函数：

> ```javascript
> var aaa = v => v
> ```

如果箭头函数需要参数

```javascript
var f = (num1,num2) => num1 +num2
//等同于
var sum = function (num1,num2) {
    return num1 +num2
}
```



**什么时候使用箭头函数**

> 当我们准备把一个函数作为参数传到另外一个函数时

**箭头函数的this指向问题**

> 箭头函数this查找时，向外层作用域中，一层一层查找this，直到有this定义为止。

##### 使用注意点

（1）函数体内的`this`对象，就是定义时所在的对象，而不是使用时所在的对象。

（2）不可以当作构造函数，也就是说，不可以使用`new`命令，否则会抛出一个错误。

（3）不可以使用`arguments`对象，该对象在函数体内不存在。如果要用，可以用 rest 参数代替。

（4）不可以使用`yield`命令，因此箭头函数不能用作 Generator 函数。

上面四点中，第一点尤其值得注意。`this`对象的指向是可变的，但是在箭头函数中，它是固定的。

### 9.数组的扩展

#### 1）含义

> 扩展运算符(spread)是 `... `它好比 rest 参数（剩余参数）的逆运算，
>
> rest参数允许你把多个独立的参数合并到一个数组中；
>
> 而扩展运算符则允许将一个数组分割，并将各个项作为分离的参数传给函数。
>
> 该运算符组要用于函数调用

```javascript
function push (arry,...items) {
    array.push(...items)
}
function add (x,y) {
    return x + y
}
const number = [4,38]
add(...number) // 42
```

上面代码中，`array.push(...items)`和`add(...numbers)`这两行，都是函数的调用，它们都使用了扩展运算符。该运算符将一个数组，变为参数序列。

#### 2)替代函数apply方法

> 由于扩展运算符可以展开数组，所以不在需要apply方法，将数组转化为函数的参数了

```javascript
// ES5 的写法
function f(x, y, z) {
  // ...
}
var args = [0, 1, 2];
f.apply(null, args);

// ES6的写法
function f(x, y, z) {
  // ...
}
let args = [0, 1, 2];
f(...args);
```

#### 3)扩展运算符的应用

- 复刻数组

  数组是复合的数据类型，直接复制的话，只是复制了指向底层数据结构的指针，而不是克隆一个全新的数组。

  ```javascript
  const a1 = [1, 2];
  const a2 = a1;
  
  a2[0] = 2;
  a1 // [2, 2]
  //上面代码中，a2并不是a1的克隆，而是指向同一份数据的另一个指针。修改a2，会直接导致a1的变化。
  ```

  扩展运算符提供了复制数组的写法

  ```javascript
  const a1 = [1,2]
  //写法一
  const a2 = [...a1]
  //写法二
  const [...a2] = a1
  ```

- 合并数组

  ```javascript
  const arr1 = ['a', 'b'];
  const arr2 = ['c'];
  const arr3 = ['d', 'e'];
  
  // ES5 的合并数组
  arr1.concat(arr2, arr3);
  // [ 'a', 'b', 'c', 'd', 'e' ]
  
  // ES6 的合并数组
  [...arr1, ...arr2, ...arr3]
  // [ 'a', 'b', 'c', 'd', 'e' ]
  ```

  

- 与解构赋值结合

  扩展运算符可以与解构赋值结合起来，用于生成数组

  ```javascript
  // ES5
  a = list[0], rest = list.slice(1)
  // ES6
  [a, ...rest] = list
  ```

- 字符串

  扩展运算符还可以将字符串转为真正的数组

  ```javascript
  [...'hello']
  // [ "h", "e", "l", "l", "o" ]
  ```

  实现了Iterator接口对象 **不明白**

  任何定义了遍历器(Iterator)接口的对象，都可以用扩展运算符转为真正的数组

- Map和Set结构，Generator 函数

  > 扩展运算符内部调用的是数据结构的Iterator接口，因此只需要具有Iterator接口的对象，都可以使用扩展运算符，比如Map结构

  ```javascript
  let map = new Map([
    [1, 'one'],
    [2, 'two'],
    [3, 'three'],
  ]);
  
  let arr = [...map.keys()]; // [1, 2, 3]
  ```

  Generator 函数运行后，返回一个遍历器对象，因此也可以使用扩展运算符。

  ```javascript
  //变量go是一个 Generator 函数，执行后返回的是一个遍历器对象
  const go = function*(){
    yield 1;
    yield 2;
    yield 3;
  };
  
  [...go()] // [1, 2, 3]
  ```

  如果对没有 Iterator 接口的对象，使用扩展运算符，将会报错。

  ```javascript
  const obj = {a: 1, b: 2};
  let arr = [...obj]; // TypeError: Cannot spread non-iterable object
  ```

#### 4）Array.from()

将伪数组对象或可遍历对象转换为真数组

> **如果一个对象的所有键名都是正整数或零，并且有length属性**，那么这个对象就很像数组，称为伪数组。典型的伪数组有**函数的arguments对象，以及大多数 DOM 元素集，还有字符串。**

```javascript
...
<button>测试1</button>
<br>
<button>测试2</button>
<br>
<button>测试3</button>
<br>
<script type="text/javascript">
let btns = document.getElementsByTagName("button")
console.log("btns",btns);//得到一个伪数组
btns.forEach(item=>console.log(item)) Uncaught TypeError: btns.forEach is not a function
</script>
```

针对伪数组，没有数组一般方法，直接遍历便会出错,ES6新增Array.from()方法来提供一种明确清晰的方式以解决这方面的需求。

```js
Array.from(btns).forEach(item=>console.log(item))将伪数组转换为数组
```



#### 5)Array.of()

>  用于将一组值，转换为数组

当调用 new Array( )构造器时，根据传入参数的类型与数量的不同，实际上会导致一些不同的结果， 例如：

```js
//使用单个数值参数来调用Array构造器时，数组的长度属性会被设置为该参数
let items = new Array(2) ;
console.log(items.length) ; // 2
console.log(items[0]) ; // undefined
console.log(items[1]) ;
```



```js
//使用多个参数(无论是否为数值类型)来调用,这些参数也会成为目标数组的项
let items = new Array(1, 2) ;
console.log(items.length) ; // 2
console.log(items[0]) ; // 1
console.log(items[1]) ; // 2
```

数组的这种行为既混乱又有风险，因为有时可能不会留意所传参数的类型。

> ES6 引入了Array.of( )方法来解决这个问题。该方法的作用非常类似Array构造器，但在使用单个数值参数的时候并不会导致特殊结果。
>
> **Array.of( )方法总会创建一个包含所有传入参数的数组，而不管参数的数量与类型**：

```js
let items = Array.of(1, 2);
console.log(items.length); // 2
console.log(items[0]); // 1
console.log(items[1]); // 2
items = Array.of(2);
console.log(items.length); // 1
console.log(items[0]); // 2
```

==Array.of基本上可以用来替代Array()或newArray()==，并且不存在由于参数不同而导致的重载，而且他们的行为非常统一。

------



#### 6）数组实例的 copyWithin()

#### 7）数组实例的 find() 和 findIndex()

- 数组实例的`find`方法，用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为`true`的成员，然后返回该成员。如果没有符合条件的成员，则返回`undefined`。
- 数组实例的`findIndex`方法的用法与`find`方法非常类似，返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回`-1`。

```js
[1, 4, -5, 10].find((n) => n < 0) // -5
```

```js
[1, 5, 10, 15].findIndex(function(value, index, arr) {
  return value > 9;
}) // 2
```



#### 8）数组实例的 fill() 

`fill`方法使用给定值，填充一个数组。

#### 9)数组实例的 entries()，keys() 和 values()

#### 10)数组实例的 includes()

> `Array.prototype.includes`方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的`includes`方法类似
>
> 该方法的第二个参数表示搜索的起始位置，默认为0。如果第二个参数为负数，则表示倒数的位置，如果这时它大于数组长度（比如第二个参数为-4，但数组长度为3），则会重置为从0开始。

```js
[1, 2, 3].includes(2)   // true
[1, 2, 3].includes(3, -1); // true
[1, 2, 3, 5, 1].includes(1, 2); // true
```



没有该方法之前，我们通常使用数组的indexOf方法，检查是否包含某个值。

indexOf方法有两个缺点

- **一是不够语义化，它的含义是找到参数值的第一个出现位置，所以要去比较是否不等于-1，表达起来不够直观。**
- **二是，它内部使用严格相等运算符（===）进行判断，这会导致对NaN的误判**。

```js
[NaN].indexOf(NaN) // -1
[NaN].includes(NaN) // true
```



#### 11)数组实例的 flat()，flatMap()

#### 12)数组实例entries(),keys()和value()

> ES6 提供entries()，keys()和values(),用于遍历数组。它们都返回一个遍历器对象，可以用for...of循环进行遍历
>
> 唯一的区别是keys()是对键名的遍历、values()是对键值的遍历，entries()是对键值对的遍历。

```js
for (let index of ['a', 'b'].keys()) {
  console.log(index);
}
// 0
// 1

for (let elem of ['a', 'b'].values()) {
  console.log(elem);
}
// 'a'
// 'b'

for (let [index, elem] of ['a', 'b'].entries()) {
  console.log(index, elem);
}
// 0 "a"
// 1 "b"

```



------



### 10.对象的扩展

#### 1）属性的简写

```javascript
onst o = {
  method() {
    return "Hello!";
  }
};

// 等同于

const o = {
  method: function() {
    return "Hello!";
  }
};
```

**注意，简写的对象方法不能用作构造函数，会报错。**

```javascript
onst obj = {
  f() {
    this.foo = 'bar';
  }
};

new obj.f() // 报错
```

#### 2）属性名表达式

#### 3）方法的name属性

> 函数的`name`属性,返回函数名，对象方法也是函数，因此也有`name` 属性

#### 4）属性的可枚举性和遍历

****

**可枚举性**

**遍历**

- for...in

  `for...in`循环遍历对象自身的和继承的可枚举属性（不含 Symbol 属性）

- Object.keys(obj)

  `Object.keys`返回一个数组，包括对象自身的（不含继承的）所有可枚举属性（不含 Symbol 属性）的键名。

- Object.getOwnPropertyNames(obj)

- Object.getOwnPropertySymbols(obj)

- Reflect.ownKeys(obj)

#### 5)super关键字



#### 6）对象的扩展符

**解构赋值**

> 对象的解构赋值用于从一个对象取值，相当于将目标对象自身的所有可遍历的（enumerable）、但尚未被读取的属性，分配到指定的对象上面。所有的键和它们的值，都会拷贝到新对象上面。

```javascript
let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
x // 1
y // 2
z // { a: 3, b: 4 }
```

解构赋值必须是最后一个参数，否则会报错。

```javascript
let { ...x, y, z } = someObject; // 句法错误
let { x, ...y, ...z } = someObject; // 句法错误
```

注意，解构赋值的拷贝是==浅拷贝==，即如果一个键的值是复合类型的值（数组、对象、函数）、那么解构赋值拷贝的是这个值的==引用==（如果修改该值，则父本的值也改变了），而不是这个值的副本。

```javascript
let obj = { a: { b: 1 } };
let { ...x } = obj;
obj.a.b = 2;
x.a.b // 2
```

解构赋值的一个用处，是扩展某个函数的参数，引入其他操作。

### 11.Symbol

#### 1.概述

> ES5的对象属性都是字符串，这容易造成属性名冲突。比如，你使用了一个他人提供的对象，但又想为这个对象添加新的方法（mixin 模式），新方法的名字就有可能与现有方法产生冲突。

> ES6引入一种新的原始数据类型Symbol，表示独一无二的值

```javascript
let s = Symbol();

typeof s
// "symbol"
```

注意：`Symbol`函数前不能用`new`命令，否则会报错，这是因为生成的`Symbol`是一个原始类型的值，不是对象，所以不能添加属性。

> 如果Symbol的参数是一个对象，就会调用该对象的`toString`方法，将其转为字符串，然后生成一个Symbol值

```javascript
const obj = {
  toString() {
    return 'abc';
  }
};
const sym = Symbol(obj);
sym // Symbol(abc)
```



#### 2.Symbol.prototype.description

> 创建Symbol的时候，可以添加一个描述

```javascript
const sym = Symbol('foo');

sym.description // "foo"
```

#### 3.作为属性名的Symbol

> 由于每一个Symbol值都是不相等的，意味着	Symbol值可以作为标识符，用于对象的属性名，就能保证不会出现同名的属性。



```javascript
let mySymbol = Symbol();

// 第一种写法
let a = {};
a[mySymbol] = 'Hello!';

// 第二种写法
let a = {
  [mySymbol]: 'Hello!'
};

// 第三种写法
let a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello!' });

// 以上写法都得到同样结果
a[mySymbol] // "Hello!"
```





### 12.Set 和 Map 数据结构

#### 1）Set

> ES6提供了新的数据结构Set。它类似于数组，但是成员的值都是==唯一==的，没有重复的值。
>
> `Set`本身是一个构造函数，用来生成Set数据结构

```javascript
const s = new Set();

[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));

for (let i of s) {
  console.log(i);
}
// 2 3 5 4	
//上面代码通过add()方法向Set结构加入成员，结果表明Set结构不会添加重复的值
```

**Set 实例的属性和方法**

Set 结构的实例有以下属性。

- `Set.prototype.constructor`：构造函数，默认就是`Set`函数。
- `Set.prototype.size`：返回`Set`实例的成员总数。

Set 实例的方法分为两大类：操作方法（用于操作数据）和遍历方法（用于遍历成员）。

- `Set.prototype.add(value)`：添加某个值，返回 Set 结构本身。
- `Set.prototype.delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功。
- `Set.prototype.has(value)`：返回一个布尔值，表示该值是否为`Set`的成员。
- `Set.prototype.clear()`：清除所有成员，没有返回值。

**遍历操作**

Set 结构的实例有四个遍历方法，可以用于遍历成员。

- `Set.prototype.keys()`：返回键名的遍历器

- `Set.prototype.values()`：返回键值的遍历器

- `Set.prototype.entries()`：返回键值对的遍历器

- `Set.prototype.forEach()`：使用回调函数遍历每个成员

  - `keys()`，`values()`,`entries()`方法返回的都是遍历器对象

    ```javascript
    let set = new Set(['red', 'green', 'blue']);
    
    for (let item of set.keys()) {
      console.log(item);
    }
    // red
    // green
    // blue
    
    for (let item of set.values()) {
      console.log(item);
    }
    // red
    // green
    // blue
    
    for (let item of set.entries()) {
      console.log(item);
    }
    // ["red", "red"]
    // ["green", "green"]
    // ["blue", "blue"]
    ```

  - `forEach()`用于对某个成员执行某种操作，没有返回值

    ```javascript
    let set = new Set([1, 4, 9]);
    set.forEach((value, key) => console.log(key + ' : ' + value))
    // 1 : 1
    // 4 : 4
    // 9 : 9
    ```

  - 遍历的应用

    扩展运算符(`...`)内部使用`for...of`循环，所以也用于Set结构

    ```javascript
    let set = new Set(['red', 'green', 'blue']);
    let arr = [...set];
    // ['red', 'green', 'blue']
    ```

    数组的`map`和`filter`方法也可以间接用于Set了

    ```javascript
    let set = new Set([1, 2, 3]);
    set = new Set([...set].map(x => x * 2));
    // 返回Set结构：{2, 4, 6}
    
    let set = new Set([1, 2, 3, 4, 5]);
    set = new Set([...set].filter(x => (x % 2) == 0));
    // 返回Set结构：{2, 4}
    ```

#### 2）WeakSet

> WeakSet 结构与Set类似，也是不重复的值的集合。
>
> 但是，与Set有两个区别
>
> 1.WeakSet的成员只能是对象，而不能是其他类型的值
>
> 2.WeakSet 中的对象都是弱引用，即垃圾回收机制不考虑 WeakSet 对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 WeakSet 之中

#### 3）Map

**含义和基本用法**

> JavaScript的对象，本质上是键值对的集合，但是传统上只能用字符串当作键。这给它的使用带来了很大限制
>
> 为了解决这个问题，ES6提供了Map数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值(包括对象)都可以当作键。
>
> 也就是说，Object结构提供了“字符串—值”的对应，Map数据结构提供了“值—值”的对应

```javascript
const map = new Map([
  ['name', '张三'],
  ['title', 'Author']
]);

map.size // 2
map.has('name') // true
map.get('name') // "张三"
map.has('title') // true
map.get('title') // "Author"
//上面代码在新建Map实例时，就指定了两个键name和title
```

**实例的属性和操作方法**

- **size属性** ：返回Map结构的成员总数
- **Map.prototype.set(key, value)**
- **Map.prototype.get(key)**:如果找不到key，返回undefined
- **Map.prototype.has(key)**：返回一个布尔值，表示某个键是否在当前Map对象中
- **Map.prototype.delete(key)**
- **Map/prototype.claear()**：清除所有成员

**遍历方法**

- `Map.prototype.keys()`：返回键名的遍历器。
- `Map.prototype.values()`：返回键值的遍历器。
- `Map.prototype.entries()`：返回所有成员的遍历器。
- `Map.prototype.forEach()`：遍历 Map 的所有成员。

需要特别注意的是：Map的遍历顺序就是插入顺序

```javascript
const map = new Map([
  ['F', 'no'],
  ['T',  'yes'],
]);

for (let key of map.keys()) {
  console.log(key);
}
// "F"
// "T"

for (let value of map.values()) {
  console.log(value);
}
// "no"
// "yes"

for (let item of map.entries()) {
  console.log(item[0], item[1]);
}
// "F" "no"
// "T" "yes"

// 或者
for (let [key, value] of map.entries()) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"

// 等同于使用map.entries()
for (let [key, value] of map) {
  console.log(key, value);
}
// "F" "no"
// "T" "yes"
```

Map 结构转为数组结构，比较快速的方法是使用扩展运算符（`...`）。

```javascript
const map = new Map([
  [1, 'one'],
  [2, 'two'],
  [3, 'three'],
]);

[...map.keys()]
// [1, 2, 3]

[...map.values()]
// ['one', 'two', 'three']

[...map.entries()]
// [[1,'one'], [2, 'two'], [3, 'three']]

[...map]
// [[1,'one'], [2, 'two'], [3, 'three']]
```

结合数组的`map`方法、`filter`方法，可以实现Map的遍历和过滤（Map本身没有这两个方法）

Map的`forEach`方法，与数组的`forEach`方法类似，也可以实现遍历

```javascript
map.forEach(function(value, key, map) {
  console.log("Key: %s, Value: %s", key, value);
});
```

**Map与其他数据结构的互相转化**





#### 4）WeakMap

> `WeakMap`结构与`Map`结构类似，也是用于生成键值对的集合。



### 13.Proxy

#### 1)概述

> Proxy（代理） 用于修改某些操作的默认行为，等同于在语言层面做出修改，所以属于一种“元编程”，即对编程语言进行编程。
>
> Proxy可以理解成，在目标对象之前，架设一层“拦截”，外界对该对象的访问，都必须通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。

```javascript
var obj = new Proxy({}, {
  get: function (target, propKey, receiver) {
    console.log(`getting ${propKey}!`);
    return Reflect.get(target, propKey, receiver);
  },
  set: function (target, propKey, value, receiver) {
    console.log(`setting ${propKey}!`);
    return Reflect.set(target, propKey, value, receiver);
  }
});
//上面代码对一个空对象架设了一层拦截，重定义了属性的读取(get)和设置(set)行为。对设置了拦截行为的对象obj，去读写它的属性，就会得到下面的结果。
obj.count = 1
//  setting count!
++obj.count
//  getting count!
//  setting count!
//  2
```

> 上面代码说明，Proxy实际上重载了点运算符，即用自己的定义覆盖了语言的原始定义
>
> ES6原生提供Proxy构造函数，用来生成Proxy实例

#### 2）语法

```javascript
let proxy = new proxy(target,handler);
```

- target 参数表示索要拦截的目标对象
- handler参数也是一个对象，用来定制拦截行为

拦截行为

- get(target,propKey,receiver) 拦截对象属性的读取
- set(target,propKey,receiver)  拦截对象的属性设置
- has(target,propKey)  拦截propKey in proxy的操作，返回一个布尔值
- deleteProperty(target,propKey)  拦截delete proxy[propKey]的操作，返回一个布尔值
- ....



### 14.Reflect

#### 1）概述

> `Reflect`（反应）对象与`Proxy`对象一样，也是ES6为了操作对象而提供的新API

`Reflect`设计目的

- 将`Object`对象的一些明显属于语言内部的方法（比如`Object.defineProperty`)，放到`Reflect`对象上。现阶段，某些方法同时在`Object`和`Reflect`对象上部署，未来的新方法将之部署在`Reflect`对象上。也就是说从`Reflect`对象上可以拿到语言内部的方法
- 修改某些`Object`方法的返回结果，让其变得更合理。比如，`Object.defineProperty(obj, name, desc)`在无法定义属性时，会抛出一个错误，而`Reflect.defineProperty(obj, name, desc)`则会返回`false`。

```javascript
// 老写法
try {
  Object.defineProperty(target, property, attributes);
  // success
} catch (e) {
  // failure
}

// 新写法
if (Reflect.defineProperty(target, property, attributes)) {
  // success
} else {
  // failure
}
```

- 让`Object`操作都编程函数行为。某些`Object`操作是命令式，比如`name in obj `和`delete obj[name]`，而`Reflect.has(obj,name)`和`Reflect.deleteProperty(obj,name)`让它们变成函数行为

  ```javascript
  // 老写法
  'assign' in Object // true
  
  // 新写法
  Reflect.has(Object, 'assign') // true
  ```

- `Reflect`对象的方法与`proxy`对象的方法一一对应，只要`Proxy`对象的方法，就能在`Reflect`对象上找到对应的方法。这就让`Proxy`对象可以方便的调用对应的`Reflect`方法，完成默认行为。

  ```javascript
  Proxy(target, {
    set: function(target, name, value, receiver) {
      var success = Reflect.set(target, name, value, receiver);
      if (success) {
        console.log('property ' + name + ' on ' + target + ' set to ' + value);
      }
      return success;
    }
  });
  ```

#### 2）静态方法

- **Reflect.get(target,name,receiver)**  方法查找并返回`target`对象的`name` 属性，没有则返回`undefined`

- **Reflecet.set(target,name,value,receive)** 方法设置`target`对象的`name`属性等于`value`

- **Reflect.has(obj,name)** 方法对应`name in obj`里面的`in`运算符。

  ```javascript
  var myObject = {
    foo: 1,
  };
  
  // 旧写法
  'foo' in myObject // true
  
  // 新写法
  Reflect.has(myObject, 'foo') // true
  //如果Reflect.has()方法的第一个参数不是对象，会报错
  ```

- **Reflect.deleteProperty(obj,name)**  方法等同于`delete obj[name]`用于删除对象的属性

  ```javascript
  const myObj = { foo: 'bar' };
  
  // 旧写法
  delete myObj.foo;
  
  // 新写法
  Reflect.deleteProperty(myObj, 'foo');
  ```

### 15.Promise

#### 1)介绍

> Promise 是异步编程的一种解决方案。
>
> 所谓`Promise`，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise是一个对象，从它可以获取异步操作的消息。
>
> 常见的应用场景：网络请求。
>
> 我们封装一个网络请求的函数，因为不能立即拿到结果，所以往往我们会传入里一个函数，在数据请求成功时，将数据通过传入的函数回调回去。
>
> 但是，网络请求非常复杂时，会出现回调地狱。即指太多的异步操作需要一步一步执行，或者一个函数里面有太多的异步操作，这时候就会产生大量的嵌套和回调。

#### 2）定时器的异步事件

```javascript
 new Promise((reslove,reject) => {
      // 1.第一次网络请求代码
      setTimeout(() => {
        reslove()
      },1000)
    }).then(() => {
      // 第一次拿到结果的处理代码
      console.log("hello world");

      return new Promise ((reslove,reject) => {
        //第二次网络请求的请求代码
        setTimeout(() => {
          reslove()
        },1000)
      }).then(() => {
        // 第二次拿到结果的处理代码
        console.log("hello VUE");

        return new Promise ((reslove,reject) => {
          //第三次网络请求的代码
          setTimeout(() => {
            reslove()
          },1000)
        }).then(() => {
          //第三次拿到结果的处理代码
          console.log("hello javascript");
        })
      })
    })
```

#### 3)Promsie三种状态

- pending:等待状态，比如正在进行网络请求，或者定时器没有事件
- fulfill：满足状态，当我们主动回调了resolve时，就是处于该状态，并且回调.then()
- reject：拒绝状态，当我们主动回调了reject时，就处于该状态，并且会回调.catch()

```javascript
  new Promise ((reslove,reject) => {
      setTimeout(() => {
        // reslove('hello world')
        reject("errmessage")
      },1000)
      // then((函数1,函数2)) 括号里可以传入两个函数,一个是成功时的,一个是失败时的
    }).then(data =>{
      console.log(data);
    },err =>{
      console.log(err);
    })
}
```

**Promise的使用流程**

1. new Promise一个实例，而且要 return（是否真的需要return promise？？）
2. new Promise 时要传入函数，函数有resolve reject 两个参数
3. 成功时执行 resolve，失败时执行reject
4. then 监听结果

#### 4)promise的链式调用

**1.普通写法**

```javascript
  new Promise((resolve,reject) => {
      setTimeout(() => {
        resolve("aaa")
      },1000)
    }).then(res => {
      //处理自己10行代码
      console.log(res,"第一层的10行代码处理");
      return new Promise ((resolve) =>{
        resolve(res+'111')
      })
    }).then(res =>{
      console.log(res,"第二层的10行代码处理");
      return new Promise ((resolve) =>{
        resolve(res+'222')
      })
    }).then(res =>{
      console.log(res,"第三层的10行代码处理");
    })
```

**2.new Promise(resolve => resolve(结果))简写**

```javascript
 new Promise((resolve,reject) => {
      setTimeout(() => {
        resolve("aaa")
      },1000)
    }).then(res => {
      //处理自己10行代码
      console.log(res,"第一层的10行代码处理");
      return Promise.resolve(res+'111')
    }).then(res =>{
      console.log(res,"第二层的10行代码处理");
      return Promise.resolve(res+'222')
    }).then(res =>{
      console.log(res,"第三层的10行代码处理");
    })
```

**3.省略掉 promise.resolve简写**

```javascript
 new Promise((resolve,reject) => {
      setTimeout(() => {
        resolve("aaa")
      },1000)
    }).then(res => {
      //处理自己10行代码
      console.log(res,"第一层的10行代码处理");
      return res+'111'
    }).then(res =>{
      console.log(res,"第二层的10行代码处理");
      return res+'222'
    }).then(res =>{
      console.log(res,"第三层的10行代码处理");
    })
```

**4.请求失败时的简写处理**

```javascript
 new Promise((resolve,reject) => {
      setTimeout(() => {
        resolve("aaa")
      },1000)
    }).then(res => {
      //处理自己10行代码
      console.log(res,"第一层的10行代码处理");
      // return Promise.resolve(res+'111')
      // 请求失败时:
      return Promise.reject("errmessage")
      //或者用throw抛出异常也可以
      // throw 'errmessage'
    }).then(res =>{
      console.log(res,"第二层的10行代码处理");
      return Promise.resolve(res+'222')
    }).then(res =>{
      console.log(res,"第三层的10行代码处理");
    }).catch(err =>{
      console.log(err);
    })
```

#### 5）Promise的all方法的使用

> 用于将多个Promise实例，包装成一个新的Promise实例

```javascript
 Promise.all([
      new Promise((resolve,reject) => {
        setTimeout(() => {
          resolve("result1")
        },2000)  
      }),
      new Promise((resolve,reject) => {
        setTimeout(() => {
          resolve('result2')
        },2000)
      })
    ]).then(results =>{
      console.log(results);
    })
```

#### 6）Promise.prototype.then()

> Promise 实例具有`then`方法，也就是说，`then`方法是定义在原型对象上的。它的作用为了Promise实例添加状态改变时的回调函数。
>
> `then`方法的第一个参数是`resolved`状态的回调函数，第二个参数是`rejected`状态的回调函数，它们都是可选的。
>
> `then`方法返回的是一个新的`Promise`实例（注意，不是原来的Promise实例）因此可以采用链式写法，即then方法后面再调用另一个then方法

```javascript
getJSON("/posts.json").then(function(json) {
  return json.post;
}).then(function(post) {
  // ...
});
```

#### 7）Promise.prototype.catch()

> Promise.prototype.catch()方法是 用于指定发生错误时的回调函数

#### 应用

**加载图片**

> 我们可以将图片的加载写成一个Promise。一旦加载完成，Promise的状态就会发生变化

```javascript
const preloadImage = function (path) {
  return new Promise(function (resolve, reject) {
    const image = new Image();
    image.onload  = resolve;
    image.onerror = reject;
    image.src = path;
  });
};
```

**Generator 函数与 Promise的结合**

> 使用Generator 函数管理流程，遇到异步操作的时候，通常返回一个Promise对象

```javascript
function getFoo () {
  return new Promise(function (resolve, reject){
    resolve('foo');
  });
}

const g = function* () {
  try {
    const foo = yield getFoo();
    console.log(foo);
  } catch (e) {
    console.log(e);
  }
};

function run (generator) {
  const it = generator();

  function go(result) {
    if (result.done) return result.value;
    return result.value.then(function (value) {
      return go(it.next(value));
    }, function (error) {
      return go(it.throw(error));
    });
  }

  go(it.next());
}

run(g);
// foo

//上面代码的Generator函数g之中，有一个异步操作getFoo，它返回的就是一个Promise对象。函数run用来处理这个Promise对象，并调用下一个next方法。
```





### 16.Iterator 和 for ...of 循环

#### 1）Iterator(遍历器)的概念

> 需要一种统一的接口机制，来处理所有不同的数据结构
>
> 遍历器(Iterator) 就是这样一种机制。他是一种接口，为各种不同的数据结构提供统一访问机制。任何数据结构只要部署Iterator接口，就可以完成遍历操作
>
> Iterator的遍历过程：创建一个指针对象，不断调用指针对象的`next`方法，每次调用`next`方法，都会返回数据结构的当前成员信息，具体来说就是返回一个包含`value`和`done`两个属性的对象。

**Iterator的作用**

- 为各种数据结构，提供一个统一的，简单的访问接口
- 使得数据结构成员能够按某种次序排列
- 创造了一种新的遍历命令`for...of`循环，Iterator接口主要提供`for...of`消费

#### 2）默认Iterator接口

> Iterator 接口的目的，就是为所有数据结构，提供一种统一的访问机制，即`for...of`循环。
>
> 一个数据结构只要具有`Symbol.iterator`属性，就可以认为是可遍历的

原生具备Iterator接口的数据结构

- Array
- Map
- Set
- String
- TypedArray
- 函数的arguments对象
- NodeList对象

#### 3)for ... of循环

### 17.Generator 函数的语法

#### 1)基本概念

> Generator 函数又多种理解角度。
>
> 语法上：可以理解成，Generator 函数时一个状态机，封装了多尔内部状态。执行Generator函数会返回一个遍历器对象。
>
> 形式上：1.function 关键字与函数名之间有一个 *号；2. 函数体内部使用`yield`表达式，定义不同的内部状态。

```javascript
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';
}

var hw = helloWorldGenerator();
//函数有三个状态：hello、world和return
// 调用Generator函数后，函数并不执行，返回的不是结果，而是一个指向内部状态的指针对象
```

遍历器的`next`方法

> 使得指针移向下一个状态，直到遇到yield表达式或(return语句)，`yield`表示暂停执行的标记，而`next`方法可以恢复执行

```javascript
hw.next()
// { value: 'hello', done: false }

hw.next()
// { value: 'world', done: false }

hw.next()
// { value: 'ending', done: true }

hw.next()
// { value: undefined, done: true }
```

#### 2）yield表达式

如果该函数没有return语句，则调用next返回的对象`value`属性为`undefined`	

`yield`表达式与`return`语句区别

> 相似之处在于，都能返回紧跟在语句后面的那个表达式的值
>
> 区别在于每次遇到`yield`，函数暂停执行，下一次再从该位置继续向后执行，而`return`语句不具备位置记忆的功能。一个函数里面，只能执行一次（或者说一个）`return`语句，但是可以执行多次（或者说多个）`yield`表达式。

#### 3)与Iterator接口关系

> 由于Generator 函数就是遍历器生成函数，因此可以把Generator 赋值给对象的`symbol.iterator`属性，从而使得该对象具有Iterator接口

```javascript
var myIterable = {};
myIterable[Symbol.iterator] = function* () {
  yield 1;
  yield 2;
  yield 3;
};

[...myIterable] // [1, 2, 3]
//Generator 函数赋值给Symbol.iterator属性，从而使得myIterable对象具有了 Iterator 接口，可以被...运算符遍历了。
```

#### 4)next 方法的参数

> `yield`表达式本身没有返回值，或者说返回值为`undefined`。`next`可以带一个参数，该参数就会被当作上个`yield`表达式的返回值

```javascript
function* f() {
  for(var i = 0; true; i++) {
    var reset = yield i;
    if(reset) { i = -1; }
  }
}
var g = f();
g.next() // { value: 0, done: false }
g.next() // { value: 1, done: false }
g.next(true) // { value: 0, done: false }
```

#### 5）for...of 循环

> for ... of 循环可以自动遍历Generator 函数运行时生成的`Iterator`对象，且此时不再需要调用`next`方法

```javascript
function* foo() {
  yield 1;
  yield 2;
  yield 3;
  yield 4;
  yield 5;
  return 6;
}
for (let v of foo()) {
  console.log(v);
}
// 1 2 3 4 5
// 使用for...of语句时不需要使用next方法。
```

#### 6) Generator.prototype.throw()

> Generator 函数返回的遍历器对象，都有一个`throw`方法，可以在函数体外抛出错误，然后在 Generator 函数体内捕获。

```javascript
var g = function* () {
  try {
    yield;
  } catch (e) {
    console.log('内部捕获', e);
  }
};

var i = g();
i.next();

try {
  i.throw('a');
  i.throw('b');
} catch (e) {
  console.log('外部捕获', e);
}
// 内部捕获 a
// 外部捕获 b
```

上面代码中，遍历器对象`i`连续抛出两个错误。第一个错误被 Generator 函数体内的`catch`语句捕获。`i`第二次抛出错误，由于 Generator 函数内部的`catch`语句已经执行过了，不会再捕捉到这个错误了，所以这个错误就被抛出了 Generator 函数体，被函数体外的`catch`语句捕获。

#### 6）Generator.prototype.return()

> Generator 函数返回的遍历器对象，还有一个`return()`方法，可以返回给定的值，并且终结遍历Generator函数





### 18.Generator函数的异步应用

> 异步编程对JavaScript语言太重要，JavaScript语言的执行环境是“单线程”的，如果没有异步编程，根本没法用，非卡死不可

#### 1）异步

> 简单来说，就是一个任务不是连续完成，可以理解为任务被分成两段，先执行第一段，然后转为执行其他任务，等做好了准备，再回过头来执行第二段
>
> 比如:	有一个任务是读文件操作，任务第一段就是向操作系统发出请求，要求读文件。然后，程序执行其他任务，等到操作系统返回文件，再接着执行第二阶段(处理文件)。

#### 2）回调函数

> JavaScript对异步编程的实现，就是回调函数。所谓回调函数，就是把任务的第二段单独写在一个函数里面，等到执行这个任务时，就调用这个函数

```javascript
fs.readFile('/etc/passwd', 'utf-8', function (err, data) {
  if (err) throw err;
  console.log(data);
});
//readFile函数的第三个参数就是回调函数，等待操作系统返回了/etc/passwd这个文件后，回调函数执行
```

一个有趣的问题:为什么Node约定，回调函数的第一个参数，必须是错误对象`err`(如果没有错误就是null)

​	------原因是执行分成两段，第一段执行完成以后，任务所在的上下文环境已经结束·。在这以后抛出的错误，原来的上下文环境无法捕捉，只能当作参数，传入第二段。

#### 3）Promise

> 回调函数本身没有问题，它的问题出现在多个回调函数嵌套。因为异步操作形成强耦合，只要有一个操作需要修改，它的上层回调函数和下层回调函数都可能要修改。这中情况就称为“回调函数地狱”。

promise对象就是为了解决这个问题而提出的。他不是新的语法功能，而是一种新的写法，允许将回调函数嵌套，改为链式调用。

> ```javascript
> var readFile = require('fs-readfile-promise');
> 
> readFile(fileA)
> .then(function (data) {
>   console.log(data.toString());
> })
> .then(function () {
>   return readFile(fileB);
> })
> .then(function (data) {
>   console.log(data.toString());
> })
> .catch(function (err) {
>   console.log(err);
> });
> ```

Promise的最大的问题是代码冗余，原来的任务被Promise包装了一下，一眼看上去都是一堆`then`，原来的语义不清楚

------

#### 4）Generator函数

**协程**

> 意思是多个线程相互协作，完成异步任务。

协程有点像函数，又有点像线程。运行流程大致如下

- 第一步，协程A开始执行
- 第二步，协程A执行到一半，进入暂停，将执行权给协程B
- 第三步，（一段时间后）协程B交还执行权
- 第四步，协程A恢复执行

```javascript
//读取文件的协程写法
function* asyncJob() {
  // ...其他代码
  var f = yield readFile(fileA);
  // ...其他代码
}
//上述代码的函数asyncJob是一个协程，它的奥妙就在其中`yield`命令。他执行到此处，执行权交给其他线程。也就是说，`yield`命令是异步两个阶段的分界线
```

**协程的Generator函数实现**

> Generator 函数是协程在ES6的实现，最大的特点是可以交出函数的执行权（即暂停执行）
>
> 整个Generator就是一个封装的异步任务，后者说异步任务容器。异步操作需要暂停的地方，都用`yield`语句注明。Generator 函数的执行方法如下。

```javascript
function* gen(x) {
  var y = yield x + 2;
  return y;
}

var g = gen(1);
g.next() // { value: 3, done: false }
g.next() // { value: undefined, done: true }
```

### 19.async函数

#### 1.含义

> async 函数就是Generator函数的语法糖

前文有一个Generator函数，一次读取两个文件

```javascript
const fs = require('fs');
const readFile = function (fileName) {
  return new Promise(function (resolve, reject) {
    fs.readFile(fileName, function(error, data) {
      if (error) return reject(error);
      resolve(data);
    });
  });
};

const gen = function* () {
  const f1 = yield readFile('/etc/fstab');
  const f2 = yield readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};
```

上面代码的函数`gen`可以写成`async`函数，就是下面这样。

```javascript
const asyncReadFile = async function () {
  const f1 = await readFile('/etc/fstab');
  const f2 = await readFile('/etc/shells');
  console.log(f1.toString());
  console.log(f2.toString());
};
```

> async 函数就是将generator函数的星号(*)替换async，将yield替换await。

#### 2.基本用法

> async 函数返回一个Promise对象，可以使用then方法添加回调函数。当函数执行时，一旦遇到await就会先返回，等到异步操作完成，再接着执行函数体内后面的语句

例子

```javascript
async function getStockPriceByName(name) {
  const symbol = await getStockSymbol(name);
  const stockPrice = await getStockPrice(symbol);
  return stockPrice;
}

getStockPriceByName('goog').then(function (result) {
  console.log(result);
});
```

### 20.rest参数(...变量名)

> ES6 引入 rest 参数（形式为...变量名），用于获取函数的多余参数，这样就不需要使用arguments对象了

**rest 参数搭配的变量是一个数组，该变量将多余的参数放入数组中。**

我们举个例子：如何实现一个求和函数？

**传统写法：**

```js
function addNumbers(a,b,c,d,e){
  var numbers = [a,b,c,d,e];
  return numbers.reduce((sum,number) => {
    return sum + number;
  },0)
 }
 console.log(addNumbers(1,2,3,4,5));//15
```

**ES6写法：**

```js
function addNumbers(...numbers){
  return numbers.reduce((sum,number) => {
    return sum + number;
  },0)
 }
 console.log(addNumbers(1,2,3,4,5));//15

```

**rest 参数还可以与箭头函数结合**

```js
const numbers = (...nums) => nums;
numbers(1, 2, 3, 4, 5)// [1,2,3,4,5]  
```

**注意：**

**①每个函数最多只能声明一个rest参数，而且 rest参数必须是最后一个参数，否则报错。**

**②rest参数不能用于对象字面量setter之中**

### 编程风格

#### 1）块级作用域

（1） let 取代var

（2）字符串：静态字符串一律使用单引号或反引号，不使用双引号。动态字符串使用反引号

（3）解构赋值：使用数组成员对变量进行赋值时，优先使用解构赋值。

（4）对象：单行定义的对象，最后一个成员不以逗号结尾。多行定义的对象，最后一个成员以逗号结尾

（5）数组：使用扩展运算符（...)拷贝数组

（6）函数：立即执行函数可以写成箭头函数形式

（7）Map：注意区分Object和Map，只有模拟现实世界的实体对象时，才使用Object。如果只是需要`key:value`的数据结构，使用Map结构。因为Map有内建的遍历机制。

（8）class:总是用class，取代 需要prototype的操作。因为Class的写法更简洁，更易理解。

（9）模块：首先，Module语法是JavaScript模板的标准写法，坚持使用这种写法。使用`import`取代`require`

（10）ESlint的使用：是一个语法规则和代码风格的检查工具，可以用来保证写出语法正确、风格统一的代码



### 21. Class基本用法

####  1）简介

```js
class Point {
	constructor(x,y) {
		this.x = x;
        this.y = y
    }
    toString() {
        return '('+ this.x + ',' + this.y + ')';
    }
}
```

`constructor()`方法是类默认的方法，通过`new()`命令生成对象实例时，自动调用该方法。一个类必须有`constructor()`方法，如果没有显式定义，一个空的`constructor()`方法会被默认添加。

**取值函数（getter）和存值函数（setter）**

> 在“类”的内部可以使用`get`和`set`关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为

```js
class MyClass {
  constructor() {
    // ...
  }
  get prop() {
    return 'getter';
  }
  set prop(value) {
    console.log('setter: '+value);
  }
}

let inst = new MyClass();

inst.prop = 123;
// setter: 123

inst.prop
// 'getter'
```



#### 2)静态方法

> 如果在一个方法前，加上`static`关键字，就表示该方法不会被实例继承，而不是直接通过类来调用，这就称为“静态方法”

```js
class Foo {
  static classMethod() {
    return 'hello';
  }
}

// 直接在Foo类上调用，而不是在Foo类的实例上调用
Foo.classMethod() // 'hello' 

var foo = new Foo();
foo.classMethod()
// TypeError: foo.classMethod is not a function
```

注意：如果静态方法包含`this`关键字，这个`this`指的是类，而不是实例

- 父类的静态方法，可以被子类继承。

- 静态方法也是可以从`super`对象上调用的。

#### 3）实例属性

