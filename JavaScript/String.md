

# String

> **内置方法只对 string 类型管用，其他类型并不会隐式转换**

------

## 一：提取子字符串

- String.prototype.slice()

- String.prototype.substring()

- ~~String.prototype.substr()----- 废弃 -----~~

### String.prototype.slice()

> **语法：str.slice(beginIndex, endIndex)**
>
> **作用：截取str的一部分，并返回一个新字符串，且不会改动原字符串**
>
> **返回值：新字符串**

- beginIndex : 提取字符串的开始索引，默认为 0
		- 如果为负值，会被当做 beginIndex + str.length 来看待，也就是倒数第几个。例如，如果 beginIndex 为 -3，那么则看作是 str.length - 3 ，也就是倒数第 3 个
	- 如果 beginIndex >= str.length，则返回空字符串
- endIndex : 提取字符串的结束索引（可选）
		- 如果省略该参数，会一直提取到字符串末尾
	- 如果为负值，会被当做 endIndex+ str.length 来看待，也就是倒数第几个。例如，如果 endIndex为 -3，那么则看作是 str.length - 3 ，也就是倒数第 3 个
- 新字符串包括 beginIndex 但不包括 endIndex
- 如果转换成正数后， endIndex >= beginIndex ，则返回空字符串
- 如果任一参数为 NaN ，则被当作 0
- 任一参数大于 str.length ，都会被当做 str.length

```javascript
let str = 'The morning is upon us.' // str 的长度 length 是 23
let str1 = str.slice(),
    str2 = str.slice(1, 1),
    str3 = str.slice(1, 6),
    str4 = str.slice(6, 1),
    str5 = str.slice(-6),
    str6 = str.slice(1, -6),
    str7 = str.slice(1),
    str8 = str.slice(23),
    str9 = str.slice(NaN, 6);
console.log(str1); // The morning is upon us.
console.log(str2); // ""
console.log(str3); // he mo
console.log(str4); // ""
console.log(str5); // on us.
console.log(str6); // he morning is up       
console.log(str7); // he morning is upon us.
console.log(str8); // ""
console.log(str9); // The mo
```

### String.prototype.substring()

> **语法：str.substring(indexStart, indexEnd)**
>
> **作用：截取str的一部分，并返回一个新字符串，且不会改动原字符串**
>
> **返回值：新字符串**

- indexStart : 提取字符串的开始索引，默认为 0
- indexEnd : 提取字符串的结束索引 （可选）
		- 如果省略该参数，会一直提取到字符串末尾
- 任一参数小于 0 或为 NaN，都会被当做 0 
- 任一参数大于 str.length，都会被当做 str.length
- 新字符串包括 indexStart 但不包括 indexEnd
- 如果 indexStart == indexEnd ，则返回空字符串
- 如果转换后 indexStart > indexEnd ，则 indexStart 与 indexEnd 会互换位置

```javascript
let str = 'The morning is upon us.' // str 的长度 length 是 23
let str1 = str.substring(),
    str2 = str.substring(1, 1),
    str3 = str.substring(1, 6),
    str4 = str.substring(6, 1),
    str5 = str.substring(-6),
    str6 = str.substring(1, -6),
    str7 = str.substring(-1,25),
    str8 = str.substring(25, -1),
    str9 = str.substring(NaN, 6),
    str10 = str.substring(25);
console.log(str1); // The morning is upon us.
console.log(str2); // ""
console.log(str3); // he mo
console.log(str4); // he mo
console.log(str5); // The morning is upon us.
console.log(str6); // T
console.log(str7); // The morning is upon us.
console.log(str8); // The morning is upon us.
console.log(str9); // The mo
console.log(str10); // ""
```

### slice() 与 substring() 的异同

>#### 相同点
>
>1. **都返回新字符串**
>2. **新字符串包括 beginIndex ，但不包括 endIndex**
>3. **beginIndex 默认为 0 , endIndex 不给则截取至末尾**
>4. **如果 beginIndex == endIndex，则返回空字符串**
>5. **如果 index == NaN ，则被看作 0**
>6. **如果 index > str.length ，则被看作 str.length**

> #### 不同点
> 1. **当参数index为负值的时候，处理方式不同**
		- **slice()是看作 str.length + index ，也就是倒数第几个**
		- **substring() 是看作 0**
> 3. **当 beginIndex > endIndex 时，处理方式不同**
		- **slice() 是直接返回空字符串**
		- **substring()则是将它们的位置互换**
> 

## 二：字符串是否包含子字符串

- String.prototype.includes()
- String.prototype.startsWith()
- String.prototype.endsWith()

### String.prototype.includes()

> **语法：str.includes(searchString, position)**
>
> **作用：判断一个字符串是否包含在另外一个字符串里**
>
> **返回值：true or false**

- searchString : 要在此字符串中搜索的字符串
	- 如果不给此参数，默认为字符串 'undefined'
- position : 开始搜索的位置索引，默认为 0 （可选）
		- 如果 position 为 负值或为 NaN，则被看作 0

```javascript
let str = 'To be, or not to be, that is the question.';  // str.length = 42
let str1 = 'undefined'
console.log(str.includes()); // false
console.log(str1.includes()); // true
console.log(str.includes('To be', -6)); // true
console.log(str.includes('To be', NaN)); // true
console.log(str.includes('To be')); // true
console.log(str.includes('To be', 1)); // false
```

### String.prototype.startsWith()

>**语法：str.startsWith(searchString, position)**
>
>**作用：判断一个字符串是否以另外一个字符串开头**
>
>**返回值：true or false**

- searchString : 要在此字符串中搜索的字符串
	- 如果不给此参数，默认为字符串 'undefined'
- position : 开始搜索的位置索引，默认为 0 （可选）
		- 如果 position 为 负值或为 NaN，则被看作 0

```javascript
let str = 'To be, or not to be, that is the question.'; // str.length = 42
let str1 = 'undefined'
console.log(str.startsWith()); // false
console.log(str1.startsWith()); // true
console.log(str.startsWith('To be', -6)); // true
console.log(str.startsWith('To be', NaN)); // true
console.log(str.startsWith('To be')); // true
console.log(str.startsWith('To be', 1)); // false
console.log(str.startsWith('to be')); // false
console.log(str.startsWith('to be', 14)); // true
```

### String.prototype.endsWith()

> **语法：str.endsWith(searchString, length)**
>
> **作用：判断一个字符串是否以另外一个字符串开头**
>
> **返回值：true or false**

- searchString : 要在此字符串中搜索的字符串
	- 如果不给此参数，默认为字符串 'undefined'
- length : 作为 str 的长度，默认值为 str.length
		- 设置后也应该是设置后的 str.length = 设置的数，如果是算下标就得加一
	- 如果 length 为 负值或为 NaN，则被看作 0

```javascript
let str = 'To be, or not to be, that is the question.'; // str.length = 42
let str1 = 'undefined'
console.log(str.endsWith()); // false
console.log(str1.endsWith()); // true
console.log(str.endsWith('question.', -6)); // false
console.log(str.endsWith('question.', NaN)); // false
console.log(str.endsWith('question.')); // true
console.log(str.endsWith('question.', str.length - 1)); // false
console.log(str.endsWith('to be')); // false
console.log(str.endsWith('to be', 19)); // true
```

### includes()、startsWith() 与 endsWith() 的异同

>#### 相同点
>
>1. **都是返回 true or false**
>2. **searchString 如果未传，默认为 'undefined'**
>3. **position 和 length 如果为 负值 或 NaN，则会被看作 0**

> #### 不同点
>
> 1. **includes() 与 startsWith() 极其相似，只不过 startsWith() 只查找开头，而 includes() 会查找至字符串末尾**
> 2. **endsWith() 与另外两个函数的 第二个参数 的传入不同，includes() 和 startsWith()的第二个参数是开始查找的索引值 (position)，而 endsWith() 传入的是 length (str.length)**

## 三：字符串中定位子字符串

- String.prototype.indexOf()
- String.prototype.lastIndexOf()

### String.prototype.indexOf()

> **语法：str.indexOf(searchValue, fromIndex)**
>
> **作用：返回指定字符串第一次出现的索引值**
>
> **返回值：包含则返回第一次出现的索引值，不包含返回 -1**

- searchValue : 要被查找的字符串
		- 如果不给此参数，默认为字符串 'undefined'
	- 如果是 空字符串""，返回转化后的 fromIndex
- fromIndex : 开始查找的索引值，默认为 0
		- 如果 fromIndex < 0 ，则被看作 0
	- 如果 fromIndex 为 NaN，则为默认值 0
	- 如果 fromIndex > str.length，则被看作 str.length

```javascript
let str = 'emm,hello world,hello vue' // str.length = 25
let str1 = 'emm,undefined'
console.log(str.indexOf()); // -1
console.log(str1.indexOf()); // 4
console.log(str.indexOf('hello')); // 4 
console.log(str.indexOf('hello',1)); // 4 
console.log(str.indexOf('hello',-1)); // 4 
console.log(str.indexOf('hello',NaN)); // 4 
console.log(str.indexOf('hello',26)); // -1
console.log(str.indexOf('',-1)); // 0
console.log(str.indexOf('',NaN)); // 0
console.log(str.indexOf('',28)); // 25
```

### String.prototype.lastIndexOf()

>**语法：str.lastIndexOf(searchValue, fromIndex)**
>
>**作用：返回指定字符串最后一次出现的索引值，会从fromIndex处从后往前搜索**
>
>**返回值：包含则返回最后一次出现的索引值，不包含返回 -1**

- searchValue : 要被查找的字符串
		- 如果不给此参数，默认为字符串 'undefined'
	- 如果是 空字符串""，返回转化后的 fromIndex
- fromIndex : 开始查找的索引值，默认值为 +Infinity
		- 如果 fromIndex < 0 ，则被看作 0
	- 如果 fromIndex 为 NaN，则为默认值 +Infinity
	- 如果 fromIndex > str.length，则被看作 str.length

```javascript
let str = 'emm,hello world,hello vue' // str.length = 25
let str1 = 'emm,undefined'
console.log(str.lastIndexOf()); // -1
console.log(str1.lastIndexOf()); // 4 
console.log(str.lastIndexOf('hello')); // 16
console.log(str.lastIndexOf('hello',15)); // 4 
console.log(str.lastIndexOf('hello',-1)); // -1
console.log(str.lastIndexOf('hello',NaN)); // 16
console.log(str.lastIndexOf('hello',28));  // 16
console.log(str.lastIndexOf('',-1)); // 0
console.log(str.lastIndexOf('',NaN)); // 25
console.log(str.lastIndexOf('',28)); // 25
```

### indexOf() 与 lastIndexOf() 的异同

> #### 相同点
>
> 1. 如果不包含子字符串就都是返回 -1
> 2. 如果 searchValue 没有传，则默认 'undefined'
> 3. 如果 searchValue 为 空字符串""，则返回转化后的 fromIndex
> 4. 如果 fromIndex 为负值，就会被看作 0
> 5. 如果 fromIndex > str.length ，则会被看作 str.length

> #### 不同点
>
> 1. lastIndexOf() 对参数为 NaN 的时候有点特别，它会被看作 +Infinity
> 2. indexOf()从前往后查找，lastIndexOf() 从后往前查找

## 三：字符串的模式匹配

- String.prototype.match()
- String.prototype.search()
- String.prototype.replace()
- String.prototype.split()

### String.prototype.match()

>语法：str.match(regExp)
>
>作用：通过检索，返回一个与其正则表达式匹配的 Array 数组
>
>返回值：Array数组 或 null

- regExp : 正则表达式对象，如果传入的不是正则表达式，则会使用 new RegExp(obj) 将其隐式转化为一个 regExp
		- 如果没有使用全局标志 g ，则返回第一个匹配项，包含捕获组。[第一个匹配项, 匹配子项1, 匹配子项2, . . . , 匹配子项n, index, input]
	- 如果使用了全局标志 g ，则返回包含所有匹配项的数组，但不包含捕获组。[第一个匹配项, 第二个匹配项, . . . , 第 n 个匹配项]
	- 如果没有传入 regExp ，则返回包含空字符串的数组  [ '', index: 0, input: str, groups: undefined ]

```javascript
// 不使用 g
let str = 'Hello world, hello Vue, hello React'
let regExp = /h(el)(l)o/i
let matchArr = str.match(regExp);
console.log(matchArr);
// [
//   'Hello',
//   'el',
//   'l',
//   index: 0,
//   input: 'Hello world, hello Vue, hello React',
//   groups: undefined
// ]
```

```javascript
// 使用 g
let str = 'Hello world, hello Vue, hello React'
let regExp = /h(el)(l)o/ig
let matchArr = str.match(regExp);
console.log(matchArr);
// [ 'Hello', 'hello', 'hello' ]
```

```javascript
// match(), 匹配失败
let str1 = 'Hello world'
let regExp1 = /Hi/g
let matchArr1 = str1.match(regExp1)
console.log(matchArr1); // null
```

```javascript
// match()，不传参数
let str = 'Hello world'
let matchArr = str.match()
console.log(matchArr);
// [ '', index: 0, input: 'Hello world', groups: undefined ]
```

### String.prototype.search()

> **语法：str.search(regExp)**
>
> **作用：返回匹配项的第一个索引值**
>
> **返回值：包含则返回第一次出现的索引值，不包含返回 -1**

- regExp : 正则表达式对象，如果传入的不是正则表达式，则会使用 new RegExp(obj) 将其隐式转化为一个 regExp
		- 与是否使用全局搜索无关，即时使用了全局搜索 g ，还是返回第一次出现的索引值
	- 如果没有传入 regExp ，则返回 0

```javascript
let str = 'hello WorlD'
let regExp = /[A-Z]/g
let regExp1 = /[.]/g
console.log(str.search()); // 0
console.log(str.search(regExp)); // 6
console.log(str.search(regExp1)); // -1
```

### String.prototype.replace()

> **语法：str.replace(subStr | regExp, newSubStr | function)**
>
> **作用：用替换值(replacement)替换掉原字符串的部分或所有模式(pattern)得到一个新字符串**
>
> **返回值：新字符串**

- pattern : 匹配模式
		- subStr : 字符串
				- 如果 pattern 为字符串，仅会替换第一个匹配项
	- regExp : 正则表达式
				- 如果 pattern 为 regExp，但没有使用全局标志 g，也仅会替换第一个匹配项
		- 如果使用了全局标志 g ，则替换所有匹配项
- replacement : 替换值
   - newSubStr : 替换字符串，可以插入一些特殊的变量名
		
   	   - | 变量名           | 代表的值                                                     |
   	     | ---------------- | ------------------------------------------------------------ |
   	     | $$               | 插入一个$                                                    |
   	     | $&               | 插入所匹配的匹配项                                           |
   	     | $`               | 插入匹配项左边儿的内容                                       |
   	     | $'               | 插入匹配项右边儿的内容                                       |
   	     | $n               | 插入第n个括号匹配的字符串，索引是从 1 开始    (pattern必须是RegExp : /()()()/) |
   	     | $<Name> (不重要) | 这里*`Name`* 是一个分组名称                                  |
   	
   - function : 回调函数
   
		   - 当匹配执行后，该函数就会执行
		  
		   - 回调函数的 返回值 作为替换字符串
		   
		   - | 变量名           | 代表的值                        |
		     | ---------------- | ------------------------------- |
		     | match            | 匹配的匹配项(相当于上面的$&)    |
		     | p1, p2, ....     | 第n个括号匹配的字符串(相当于$n) |
		     | offset           | 相当于index                     |
		     | string           | 被匹配的原字符串                |
		     | NameCaptureGroup | 命名捕获组匹配的对象            |

```javascript
const str = 'hello world, hello vue, hello react';
console.log(str.replace('hello', 'hi'));
// hi world, hello vue, hello react
console.log(str.replace(/hello/, 'hi'));
// hi world, hello vue, hello react
console.log(str.replace(/hello/g, 'hi'));
// hi world, hi vue, hi react
```

```javascript
// 交换字符串中两个单词的位置
let regExp = /(\w+)\s(\w+)/;
let str = 'Hello World';
let newStr = str.replace(regExp, '$2, $1');
console.log(newStr); // World, Hello
```

```javascript
// 如果我们想在最终的替换中，进一步转变匹配结果，我们 必须 使用一个函数
// 问题：所有出现的大写字母转换为小写，并且在匹配位置前加一个连字符

// 1. 最开始的思路，不适用行内函数来实现康康行不行---不可以的噢，宝儿
let propertyName = 'borderTop';
let regExp = /[A-Z]/g;
let newPropertyName = propertyName.replace(regExp, '$&'.toLowerCase());
console.log(newPropertyName); // borderTop // won't work
// 这是因为 '$&'.toLowerCase() 会被先解析成字符串字面量，也就是字符'$&'，而不是一个模式
// 所以使用toLowerCase()方法不会有效

// 2. 那就使用一个回调函数呗
console.log('-------------------------------2----------------------------------');
let propertyName1 = 'borderTop';
let regExp1 = /[A-Z]/g;
let newPropertyName1 = propertyName1.replace(regExp, (match) => {
  return '-' + match.toLowerCase();
})
console.log(newPropertyName1);
// 这里使用了箭头函数，是可以的，但是我们做东西，最好有封装的思想

// 3. 把这个功能封装起来
console.log('-------------------------------3----------------------------------');
function styleHyphenFormat(propertyName2) {
  function upperToHyphenLower(match) {
    return '-' + match.toLowerCase();
  }
  return propertyName2.replace(/[A-Z]/g, upperToHyphenLower);
}
let propertyName2 = 'borderTop';
console.log(styleHyphenFormat(propertyName2));
```

### String.prototype.split()

> 语法：str.split([separator1, separator2, ..., separator n], limit)
>
> 作用：利用分隔字符将字符串分割成子字符串数组，且可指定数组元素的个数
>
> 返回值：Array 数组

- str : 调用这个方法的原字符串
	- 如果 str 为 空字符串，则返回 ['""]
	- 如果 str 为空字符串，并且 separator 也为空字符串，则返回 []
- separator ：分隔字符，可以为 字符串 或者 正则表达式
		- 如果没有传入这个参数，则返回 ["整个字符串"]
	- 字符串
			- 如果为 空字符串，则返回每个字符组成的数组，['a', 'b', 'c']这样的
		- 如果分隔字符出现在字符串的开始或结尾，那么返回的数组分别以空字符开头和结尾。因此，如果 str 仅由分隔符组成，那么会返回['', '']
		- 如果是字符串数组，如['ab','cd']，那么会先通过String()类型转换为字符串，相当于String(['ab','cd'])，相当于分隔字符串为 'ab,cd'
	- 正则表达式
			- 如果是包含了捕获括号()的正则表达式，那么捕获内容也将拼接在返回的数组内
- limit ：返回数组的最多元素个数

```javascript
let str1 = '',
    str2 = 'hello',
    str3 = 'hello world, hello vue, hello react';
console.log(str2.split()); // [ 'hello' ]
console.log(str2.split('hello')); // [ '', '' ]
console.log(str2.split('')); // [ 'h', 'e', 'l', 'l', 'o' ]
console.log(str1.split('')); // []
console.log(str1.split('hello')); // [ '' ]
console.log(str3.split('hello'));// [ '', ' world, ', ' vue, ', ' react' ]
console.log(str3.split('hello', 2)); // [ '', ' world, ' ]
console.log(str3.split(/h(ell)o/));
// [ '', 'ell', ' world, ', 'ell', ' vue, ', 'ell', ' react' ]
console.log(str3.split(['rld',' he']));
// [ 'hello wo', 'llo vue, hello react' ]
```

## 四：字符串大小写转换

- String.prototype.toLowerCase()
- String.prototype.toUpperCase()

### String.prototype.toLowerCase()

> **语法：str.toLowerCase()**
>
> **作用：将字符串转换为小写形式**
>
> **返回值：新字符串**

```javascript
let str = 'HELLO WORLD'
console.log(str.toLowerCase()); // hello world
```

### String.prototype.toUpperCase()

> **语法：str.toUpperCase()**
>
> **作用：将字符串转换为大写形式**
>
> **返回值：新字符串**

```javascript
let str = 'hello world'
console.log(str.toUpperCase());  // HELLO WORLD
```

## 五：去除字符串两端的空百符

- String.prototype.trim()

### String.prototype.trim()

> **语法：str.trim()**
>
> **作用：去除字符串两端的空白符**
>
> **返回值：新字符串**

```javascript
let str = '   foo  ';
console.log(str.trim()); // 'foo'
```

## 六：返回指定字符

- String.prototype.charAt()
- 数组符：[]

### String.prototype.charAt()

> 语法：str.charAt(index)
>
> 作用：返回一个指定的字符
>
> 返回值：一个字符 或 空字符

```javascript
let str = 'hello world'; // str.length = 11
console.log(str.charAt(-2)); // ''
console.log(str.charAt(2)); // l
console.log(str.charAt(20)); // ''
```

### []

> **语法：str[index]**
>
> **作用：返回一个指定的字符**
>
> **返回值：一个字符 或 unefined**

```javascript
let str = 'hello world'; // str.length = 11
console.log(str[-2]); // undefined
console.log(str[2]); // l
console.log(str[20]); // undefined
```

## 七：拼接字符串

- String.prototype.concat()
- 加法运算符 ： +

### String.prototype.concat()

> **语法：str.concat(str1, str2, ... , str n)**
>
> **作用：拼接字符串**
>
> **返回值：新字符串**

- 不建议使用，强烈建议直接使用加法运算符 +

```javascript
let str = 'hello';
let arr = [' ', 'world,', ' ', 'hello vue'];
console.log(str.concat(' world')); // hello world
console.log(str.concat(' world', ',', 'hello vue'));
// hello world,hello vue
console.log(str.concat(...arr)); // hello world, hello vue
```

### +

> **语法：str1 + str2 + ... + str n**
>
> **作用：拼接字符串**
>
> **返回值：新字符串**

```javascript
let str = 'hello';
let arr = [' ', 'world,', ' ', 'hello vue'];
let newStr1 = str + ' world';
let newStr2 = str + ' world' + ',' + ' hello vue';
console.log(newStr1); // hello world
console.log(newStr2); // hello world, hello vue
```

## 八：对象原始值

- String.prototype.valueOf()

### String.prototype.valueOf()

> **语法：str.valueOf()**
>
> **作用：返回字符对象的原始值**
>
> **返回值：原始类型字符串**

```javascript
let str = new String('Hello world');
console.log(str.valueOf()); // 'Hello world'
```

## 九：字符串形式

- String.prototype.toString()

### String.prototype.toString()

> **语法：str.toString()**
>
> **作用：返回字符串对象的字符串形式**
>
> **返回值：原始类型的字符串**

```javascript
let str = new String("Hello world");
console.log(str.toString()); // "Hello world"
```

