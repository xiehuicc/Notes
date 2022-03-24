# Date

> **Date对象基于 Unix Time Stamp，即自 ==1970年1月1日==(UTC)起经过的毫秒数**
>
> **==月份==是从 0 开始的(==0 ~ 11==)，一月是 0，十二月是 11**
>
> **==礼拜几==的范围是 ==0 ~ 6==，周日是 0，周三是 3**

###  

### 1. 构造函数的用法

> `Date`实例有一个独特的地方。其他对象求值的时候，都是默认调用`.valueOf()`方法，但是`Date`实例求值的时候，**默认调用的是`toString()`方法**。

```js
var today = new Date();

today
// "Tue Dec 01 2015 09:34:43 GMT+0800 (CST)"

// 等同于
today.toString()
// "Tue Dec 01 2015 09:34:43 GMT+0800 (CST)"
```

作为构造函数时，`Date`对象可以接受多种格式的参数，返回一个该参数对应的时间实例。

```js
// 参数为日期字符串
new Date('January 6, 2013');
// Sun Jan 06 2013 00:00:00 GMT+0800 (CST)

// 参数为多个整数，
// 代表年、月、日、小时、分钟、秒、毫秒
new Date(2013, 0, 1, 0, 0, 0, 0)
// Tue Jan 01 2013 00:00:00 GMT+0800 (CST)
```

- 参数可以是负整数，代表1970年元旦之前的时间。

- 只要是能被`Date.parse()`方法解析的字符串，都可以当作参数

  ```js
  new Date('2013-2-15')
  new Date('2013/2/15')
  new Date('02/15/2013')
  new Date('2013-FEB-15')
  new Date('FEB, 15, 2013')
  new Date('FEB 15, 2013')
  new Date('February, 15, 2013')
  new Date('February 15, 2013')
  new Date('15 Feb 2013')
  new Date('15, February, 2013')
  // Fri Feb 15 2013 00:00:00 GMT+0800 (CST)
  ```

- 参数为年、月、日等多个整数时，年和月是不能省略的，其他参数都可以省略的。至少需要两个参数，年和月。只是用一个参数`Date`则将会解释为毫秒数

  - 年：使用四位数年份，比如 2000。 如何写成两位数或个位数，则加上**1900**
  - 月： 0表示一月。。。
  - 日： 1-31
  - 小时：0-23
  - 分钟：0-59
  - 秒：0-59
  - 毫秒：0-999



### 2. 静态方法



#### 1）Date.now()

> `Date.now`方法返回当前时间距离时间零点（1970年1月1日 00:00:00 UTC）的毫秒数，相当于 Unix 时间戳乘以1000

```js
Date.now() // 1364026285194
```

#### 2)Date.parse()

> `Date.parse`方法用来解析日期字符串，返回该时间距离时间零点（1970年1月1日 00:00:00）的毫秒数

日期字符串应该符合 RFC 2822 和 ISO 8061 这两个标准，即`YYYY-MM-DDTHH:mm:ss.sssZ`格式，其中最后的`Z`表示时区.但是，其他格式也可以被解析

```js
Date.parse('Aug 9, 1995')
Date.parse('January 26, 2011 13:51:50')
Date.parse('Mon, 25 Dec 1995 13:30:00 GMT')
Date.parse('Mon, 25 Dec 1995 13:30:00 +0430')
Date.parse('2011-10-10')
Date.parse('2011-10-10T14:48:00')

// 解析失败，返回NaN
Date.parse('xxx') // NaN
```



#### 3)Date.UTC()

> `Date.UTC`方法接受年、月、日等变量作为参数，返回该时间距离时间零点（1970年1月1日 00:00:00 UTC）的毫秒数
>
> 该方法的参数用法与Date构造函数一致，区别在于：`Date.UTC()`方法的参数，会被解释为UTC（**世界标准时间**），`Date`构造函数的参数会被解释为当前时区的时间。

```js
// 格式
Date.UTC(year, month[, date[, hrs[, min[, sec[, ms]]]]])
// 用法
Date.UTC(2011, 0, 1, 2, 3, 4, 567)
// 1293847384567

```



### 3. 实例方法

> 除了ValueOf 和 toString，可以分为一下三类
>
> - to 类： 从Date 对象返回一个字符串，表示指定的时间
> - get 类：获取 Date 对象的日期和时间
> - set类：设置 Date 对象的日期和时间



#### 一、to类方法

#### 1）Date.toString()

​		返回一个完整的日期字符串

#### 2）Date.toUTCString()

​		返回对应的UTC时间，也就是比北京时间晚8小时

```js
var d = new Date(2013, 0, 1);

d.toUTCString()
// "Mon, 31 Dec 2012 16:00:00 GMT"
```



#### 3）Date.toISOString()

​		返回对应时间的ISO8601写法

```js
var d = new Date(2013, 0, 1);

d.toISOString()
// "2012-12-31T16:00:00.000Z"
```

#### 4) Date.toJson()

返回一个符合JSON格式的ISO日期字符串，与toISOString()方法返回的结果完全一样

```js
var d = new Date(2013, 0, 1);

d.toJSON()
// "2012-12-31T16:00:00.000Z"
```

#### 5）Date.toDateString()

​	返回日期和字符串（不含小时，分和秒）

```js
var d = new Date(2013, 0, 1);
d.toDateString() // "Tue Jan 01 2013"
```

#### 6）Date.toTimeString()

​	返回时间字符串（不含年月日）

#### 7）本地时间

- Date.toLocaleString()：完整的本地时间
- Date.toLocaleDateString()：本地日期（不含小时，分和秒）
- Date.toLocaleTimeString()：本地时间（不含月日）

这三个方法都有俩个可选的参数

```js
dateObj.toLocaleString([locales[, options]])
dateObj.toLocaleDateString([locales[, options]])
dateObj.toLocaleTimeString([locales[, options]])

// 这两个参数中，locales是一个指定所用语言的字符串，options是一个配置对象。下面是locales的例子，分别采用en-US和zh-CN语言设定。
var d = new Date(2013, 0, 1);

d.toLocaleString('en-US') // "1/1/2013, 12:00:00 AM"
d.toLocaleString('zh-CN') // "2013/1/1 上午12:00:00"

d.toLocaleDateString('en-US') // "1/1/2013"
d.toLocaleDateString('zh-CN') // "2013/1/1"

d.toLocaleTimeString('en-US') // "12:00:00 AM"
d.toLocaleTimeString('zh-CN') // "上午12:00:00"

```

`options`配置对象有以下属性。

- `dateStyle`：可能的值为`full`、`long`、`medium`、`short`。
- `timeStyle`：可能的值为`full`、`long`、`medium`、`short`。
- `month`：可能的值为`numeric`(数值)、`2-digit`（两位数）、`long`、`short`、`narrow`。
- `year`：可能的值为`numeric`、`2-digit`。
- `weekday`：可能的值为`long`、`short`、`narrow`。
- `day`、`hour`、`minute`、`second`：可能的值为`numeric`、`2-digit`。
- `timeZone`：可能的值为 IANA 的时区数据库。
- `timeZoneName`：可能的值为`long`、`short`。
- `hour12`：24小时周期还是12小时周期，可能的值为`true`、`false`。

```js
var d = new Date(2013, 0, 1);

d.toLocaleDateString('en-US', {
  weekday: 'long',
  year: 'numeric',
  month: 'long',
  day: 'numeric'
})
// "Tuesday, January 1, 2013"

d.toLocaleDateString('en-US', {
  day: "2-digit",
  month: "long",
  year: "2-digit"
});
// "January 01, 13"

d.toLocaleTimeString('en-US', {
  timeZone: 'UTC',
  timeZoneName: 'short'
})
// "4:00:00 PM UTC"

d.toLocaleTimeString('en-US', {
  timeZone: 'Asia/Shanghai',
  timeZoneName: 'long'
})
// "12:00:00 AM China Standard Time"

d.toLocaleTimeString('en-US', {
  hour12: false
})
// "00:00:00"

d.toLocaleTimeString('en-US', {
  hour12: true
})
// "12:00:00 AM"
```



#### 二、get类方法

- getTime()
- getDate()
- getDay()
- getFullYear()
- getMonth()
- getHours()
- getMInutes()
- getMilliseconds()
- getSeconds()
- getTimezoneOffset()：返回当前时间与UTC的时区差异，一分钟表示，返回结果考虑到了夏令时因素。



#### 四、set类方法



- `setDate(date)`：设置实例对象对应的每个月的几号（1-31），返回改变后毫秒时间戳。
- `setFullYear(year [, month, date])`：设置四位年份。
- `setHours(hour [, min, sec, ms])`：设置小时（0-23）。返回改变后毫秒时间戳
- `setMilliseconds()`：设置毫秒（0-999）。
- `setMinutes(min [, sec, ms])`：设置分钟（0-59）。
- `setMonth(month [, date])`：设置月份（0-11）。
- `setSeconds(sec [, ms])`：设置秒（0-59）。
- `setTime(milliseconds)`：设置毫秒时间戳。

```js
// 浏览器环境
let date = new Date()
console.log(date) // Thu Mar 24 2022 16:44:13 GMT+0800 (中国标准时间)
date.setHours(8,0,0)
console.log(new Date(date)) // Thu Mar 24 2022 08:00:00 GMT+0800 (中国标准时间)

// node环境 
let date = new Date()
console.log(date) // 2022-03-24T08:44:13.183Z
date.setHours(8,0,0)
console.log(new Date(date)) // 2022-03-24T00:00:00.183Z


```



### 4. 常用方法

#### 1.获取当前时间

>**获取当前时间：new Date()**
>
>**浏览器返回的是 GMT+0800 (中国标准时间)**
>
>**node环境返回的是 UTC 时间，相当于浏览器环境用了toJSON()**
>
>**它们相差了8小时，但是运用获取年月日什么的方法是正常的**

```html
<!-- 浏览器 -->
<script>
  let date = new Date()
  console.log(date); // Thu Dec 17 2020 14:05:13 GMT+0800 (中国标准时间)
  console.log(date.toJSON()); // 2020-12-17T06:05:38.719Z
  console.log(date.toString()); // Thu Dec 17 2020 14:05:13 GMT+0800 (中国标准时间)
  console.log(date.getHours()); // 14
  console.log(date.getUTCHours()); // 6
</script>
```

```javascript
// node环境
let date = new Date()
console.log(date); // 2020-12-17T06:32:19.133Z
console.log(date.toJSON()); // 2020-12-17T06:32:19.133Z
console.log(date.toString()); // Thu Dec 17 2020 14:32:19 GMT+0800 (GMT+08:00)
console.log(date.getHours()); // 14
console.log(date.getUTCHours()); // 6
```

## 二：获取时间戳、年、月、日、小时、分钟、秒、毫秒、礼拜几的基本方法

> **时间戳：date.Obj.getTime()**
>
> **时间戳的静态方法：Date.now()**
>
> **利用+运算符获取时间戳：+new Date()**
>
> **Date对象的valueOf()：dateObj.valueOf()**
>
> **年(四位数)：dateObj.getFullYear()**
>
> **月(0 ~ 11)：dateObj.getMonth()**
>
> **日(1 ~ 31)：dateObj.getDate()**
>
> **小时(0 ~ 23)：dateObj.getHours()**
>
> **分钟(0 ~ 59)：dateObj.getMinutes()**
>
> **秒(0 ~ 59)：dateObj.getSeconds()**
>
> **毫秒(0 ~ 999)：dateObj.getMilliSeconds()**
>
> **周几((礼拜天)0 ~ 6)：dateObj.getDay()**

```html
<script>
  let date = new Date()
  // Date对象
  console.log(date); // Thu Dec 17 2020 14:19:38 GMT+0800 (中国标准时间)
  // 时间戳
  console.log(Date.now()); // 1608185978486
  console.log(+new Date()); // 1608185978486
  console.log(date.getTime()); // 1608185978486
  console.log(date.valueOf()); // 1608185978486
  // 年
  console.log(date.getFullYear()); // 2020
  // 月
  console.log(date.getMonth()); // 11
  // 日
  console.log(date.getDate()); // 17
  // 小时
  console.log(date.getHours()); // 14
  // 分钟
  console.log(date.getMinutes()); // 19
  // 秒
  console.log(date.getSeconds()); // 38
  // 毫秒
  console.log(date.getMilliseconds()); // 486
  // 礼拜几
  console.log(date.getDay()); // 4
</script>
```

## 三：获取本地时间和转化为字符串形式

- toLocaleDateString() ：获取当前 日期  ->  2020/12/17
- toLocaleTimeString() ：获取当前 时间  ->  下午2:41:40
- toLocaleString() ：获取当前 日期与时间  ->  2020/12/17 下午2:41:40
- toString()：GMT+0800中国标准时间 ->Thu Dec 17 2020 14:41:40 GMT+0800 (中国标准时间)
- toJSON() ：UTC时间 相差八小时

```html
<script>
  let date = new Date()
  console.log(date); // Thu Dec 17 2020 14:41:40 GMT+0800 (中国标准时间)
  // 获取当前 日期
  console.log(date.toLocaleDateString()); // 2020/12/17
  // 获取当前 时间
  console.log(date.toLocaleTimeString()); // 下午2:41:40
  // 获取当前 日期+时间
  console.log(date.toLocaleString()); // 2020/12/17 下午2:41:40
  // GMT+0800 中国标准时间
  console.log(date.toString()); // Thu Dec 17 2020 14:41:40 GMT+0800 (中国标准时间)
  // UTC 时间
  console.log(date.toJSON()); // 2020-12-17T06:41:40.058Z
</script>
```



## 四：构造函数（四种）

- new Date()
- new Date(value)
- new Date(dateString)
- new Date(year, monthIndex, day, hours, minutes, seconds, milliseconds)

