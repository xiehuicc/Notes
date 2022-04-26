### 1.概述

> 正则表达式（regular expression）是一种表达文本模式（及字符串）的方法。
>
> 新建正则表达式有两种方法。一是使用字面量，以斜杠表示开始和结束；二是使用`RegExp`构造函数

```js
// 第一种方法在引擎编译代码时，就会新建正则表达式
var regex = /xyz/

// 第二种方法在运行时新建正则表达式，所以前者的效率较高。所以实际应用中，基本上都采用字面量定义正则表达式。
var Regex = new RegExp('xyz')
```



### 2.实例属性

- `.ignoreCase`:返回一个布尔值，表示是否设置了`i`修饰符
- `.global`：返回一个布尔值，表示是否设置了`g`修饰符
- `.multiline`：返回一个布尔值，表示是设置了`m`修饰符
- `.flag`：返回一个字符串，包含已经设置的所有修饰符，按字母排序

上面四个属性都是只读的

```js
var r = /abc/igm
r.ignoreCase // true
...
r.flag // 'gim'
```

- `.lastIndex`：返回一个整数，表示下一次开始搜索的位置。该属性可读写，但是只在进行连续搜索时有意义
- `.source`：返回正则表达式的字符串形式（不包括反斜杠），该属性只读。

```js
var r = /abc/igm;

r.lastIndex // 0
r.source // "abc"
```



### 3.实例方法

#### `.text()`

> 正则实例对象的`text`方法返回一个布尔值，表示当前模式是否能匹配参数字符串

```js
/cat/.test('cats and dogs') // true
// 上面代码验证参数字符串之中是否包含cat，结果返回true。
```

如果正则表达式带有`g`修饰符，则每一次`test`方法都从上一次结束的位置开始向后匹配。

```js
var r = /x/g;
var s = '_x_x';

r.lastIndex // 0
// 从0 开始可以可以匹配到该字符串
r.test(s) // true

// 从上一个配到到x字符串开始匹配
r.lastIndex // 2
r.test(s) // true

// 上一个匹配到字符串位置为 4 从4开始匹配不到字符串了
r.lastIndex // 4
r.test(s) // false

// 接着又会从头开始 匹配
r.lastIndex // 0
r.test(s) // ture
```

带有`g`修饰符时，可以通过正则对象的`lastIndex`属性指定开始搜索的位置。

```js
r.lastIndex = 4;
r.test(s) // false
```

**注意**，带有`g`修饰符时，正则表达式内部会记住上一次的`lastIndex`属性，这时不应该更换所要匹配的字符串，否则会有一些难以察觉的错误.

```js
var r = /bb/g;
r.test('bb') // true
r.test('b') // false
```



#### `.exec()`

> 正则实例对象的`exec()`方法，用来返回匹配结果。如果发现匹配，**就返回一个数组**，成员是匹配成功的子字符串，否则返回`null`

```js
var s = '_x_x';
var r1 = /x/;
var r2 = /y/;

r1.exec(s) // ["x"]
r2.exec(s) // null
```

> 如果正则表示式包含圆括号（即含有“组匹配”），则返回的数组会包括多个成员。**第一个**成员是整个匹配成功的结果，**后面的成员**就是圆括号对应的匹配成功的组。

```js
var s = '_x_x';
var r = /_(x)/;

r.exec(s) // ["_x", "x"]
```

### 字符类

>字符类（class）表示有一系列字符可供选择，只要匹配其中一个就可以了。所有可供选择的字符都放在方括号内，比如`[xyz]` 表示`x`、`y`、`z`之中任选一个匹配。

```js
/[abc]/.test('hello world') // false
/[abc]/.test('apple') // true
```

#### 脱字符（^）

> 如果方括号内的第一个字符是`[^]`，则表示**除了**字符类之中的字符，其他字符都可以匹配。比如，`[^xyz]`表示除了`x`、`y`、`z`之外都可以匹配。

```js
/[^abc]/.test('bbc news') // true
/[^abc]/.test('bbc') // false
```

**如果方括号内没有其他字符，即只有`[^]`，就表示匹配一切字符，其中包括换行符。**

#### 连字符（-）

> 某些情况下，对于连续序列的字符，连字符（`-`）用来提供简写形式，表示字符的连续范围。比如，`[abc]`可以写成`[a-c]`，`[0123456789]`可以写成`[0-9]`，同理`[A-Z]`表示26个大写字母。