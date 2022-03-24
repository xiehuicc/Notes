# JSON

------

## 一：JSON是什么？

- JavaScript Object Notation( JavaScript 对象表示法)
- 是一种轻量级的文本数据交换格式
- 是 JavaScript 语法的子集

## 二：JSON对象 与 JSON字符串

> - JSON 对象：{"key1": value1, "key2": value2}
> - JSON 字符串： '{"key1": value1, "key2": value2}'

## 三：JSON 对象

> 1. JSON对象可以包含多个 key / value (键值对)
> 2. key必须是字符串
> 3. value 可以是合法的JSON数据类型 (string, number , object, array, boolean, null)
> 4. 可以通过 . 和 [] 的方式访问 JSON 对象，但 . 后面的会默认为是字符串，而 [] 里边儿的可以为 变量 或者 字符串
> 5. 最好都使用 JsonObj['attribution'] 的方式获取属性的值

- 把键名赋值给另外一个变量，然后通过 . 方式去获取值，这种方式是行不通的,他只会把它当成一个字符串，所以返回的是 undefined
- 用 for in 这样的循环的时候， . 也就不好使了

```javascript
let JsonObj = {"name": "许七安", "age": 18, sex: "female"}
let peopleName = 'name'
console.log(JsonObj.name); // 许七安
console.log(JsonObj['age']); // 18
```

```javascript
console.log(JsonObj.peopleName); // undefined
console.log(JsonObj[peopleName]); // 许七安
```

```javascript
for (item in JsonObj) {
  console.log(JsonObj.item);
}
// undefined
// undefined
// undefined
```

```javascript
for (item in JsonObj) {
  console.log(JsonObj[item]);
}
// 许七安
// 18
// female
```

## 三：JSON.parse()

> 语法：JSON.parse(text, reviver)
>
> 作用：将 JSON 字符串转化为 JSON对象(JavaScript 对象)
>
> 返回值：JSON对象

- text ：必需，一个有效的 JSON 字符串
- reviver(key, value) ：可选，一个转换结果的函数

```javascript
let text = '{ "name":"Runoob", "initDate":"2013-12-14", "site":"www.runoob.com"}';
let obj = JSON.parse(text, function (key, value) {
    if (key == "initDate") {
        return new Date(value);
    } else {
        return value;
}});
console.log(obj);
// {
//   name: 'Runoob',
//   initDate: 2013-12-14T00:00:00.000Z,
//   site: 'www.runoob.com'
// }
```



## 四： JSON.stringify()

> 语法：JSON.stringify(value, replacer, space)
>
> 作用：将 JSON 对象转化为 JSON 字符串
>
> 返回值：JSON 字符串

- value ：必需，要转换的 JSON 对象
- replace: 可选，用于转化结果的 函数 或 数组
	- 函数：JSON.stringify 将调用该函数，并传入每个成员的键和值。使用返回值而不是原始值。如果此函数返回 undefined，则排除成员。根对象的键是一个空字符串：""。
	- 数组：仅转换该数组中具有键值的成员。成员的转换顺序与键在数组中的顺序一样。当 value 参数也为数组时，将忽略 replacer 数组。
- space ：可选，文本添加缩进、空格和换行符，如果 space 是一个数字，则返回值文本在每个级别缩进指定数目的空格，如果 space 大于 10，则文本缩进 10 个空格。space 也可以使用非数字，如：\t。
- JSON 不能存储 Date() 对象，会转化为 字符串
- JSON遇见 function 会删除其 key 和 value







环境变量：

用户：![image-20210408102910185](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210408102910185.png)![image-20210408102908479](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210408102908479.png)

系统：同上

