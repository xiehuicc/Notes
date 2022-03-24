# Math

| **方法**                       | **说明**                     |
| ------------------------------ | ---------------------------- |
| **Math.max(n1, n2, ... , nn)** | **返回一组数里的最大值**     |
| **Math.min(n1, n2, ... , nn)** | **返回一组数里的最小值**     |
| **Math.ceil(num)**             | **向上取整**                 |
| **Math.floor(num)**            | **向下取整**                 |
| **Math.round(num)**            | **四舍五入取整**             |
| **Math.random()**              | **获取 [0, 1) 之间的随机数** |
| **Math.abs()**                 | **取绝对值**                 |
| **Math.pow(base, exponent)**   | **base 的 exponent 次幂**    |
| **Math.sqrt(num)**             | **平方根**                   |
| **Math.cbrt(num)**             | **立方根**                   |

------

## 一：确定最值

- Math.max()
- Math.min()

### Math.max()

> **语法：Math.max(n1, n2, ... , nn)**
>
> **作用：返回一组数里的最大值**
>
> **返回值：最大值**

- 如果没有给参数，则返回 -Infinity
- 可接收 1 ~ 任意个 参数
- 如果参数里有不能转化为数字的，则返回 NaN

```javascript
let arr = [5, -1, 7, 3, 6, 0, 4]
console.log(Math.max(5, -1, 7, 3, 6, 0, 4)); // 7
console.log(Math.max(...arr)); // 7
console.log(Math.max()); // -Infinity
console.log(Math.max('a', 5, 7)); // NaN
```

### Math.min()

> 语法：Math.min(n1, n2, ... , nn)
>
> 作用：返回一组数里的最小值
>
> 返回值：最小值

- 如果没有给参数，则返回 Infinity
- 可接收 1 ~ 任意个 参数
- 如果参数里有不能转化为数字的，则返回 NaN

```javascript
let arr = [5, -1, 7, 3, 6, 0, 4]
console.log(Math.min(5, -1, 7, 3, 6, 0, 4)); // -1      
console.log(Math.min(...arr)); // -1      
console.log(Math.min()); // Infinity
console.log(Math.min('a', 5, 7)); // NaN
```

## 二：取整

- Math.ceil()
- Math.floor()
- Math.round()

### Math.ceil()

> **语法：Math.ceil(num)**
>
> **作用：向上取整**
>
> **返回值：大于等于这个数的最小整数**

```javascript
console.log(Math.ceil(1.9)); // 2
console.log(Math.ceil(2)); // 2
console.log(Math.ceil(2.1)); // 3
console.log(Math.ceil(2.5)); // 3
console.log(Math.ceil(2.7)); // 3
```

### Math.floor()

> **语法：Math.floor(num)**
>
> **作用：向下取整**
>
> **返回值：小于等于这个数的最小整数**

```javascript
console.log(Math.floor(1.9)); // 1
console.log(Math.floor(2)); // 2
console.log(Math.floor(2.1)); // 2
console.log(Math.floor(2.5)); // 2
console.log(Math.floor(2.7)); // 2
```

### Math.round()

> **语法：Math.round(num)**
>
> **作用：四舍五入取整**
>
> **返回值：四舍五入的整数**

```javascript
console.log(Math.round(1.9)); // 2
console.log(Math.round(2)); // 2
console.log(Math.round(2.1)); // 2
console.log(Math.round(2.5)); // 3
console.log(Math.round(2.7)); // 3
```

## 三：随机数

- Math.random()

### Math.random()

> **语法：Math.random()**
>
> **作用：获取一个 [0, 1) 范围内的随机数**
>
> **返回值：浮点数**

```javascript
// [min, max) 之间的随机数
function getRandomArbitrary (min, max) {
  return Math.random() * (max - min) + min
}
console.log(getRandomArbitrary(1, 2)); // 1.0721495223544841
```

```javascript
// [min, max) 之间的随机整数
function getRandomInt (min, max) {
  return Math.floor(Math.random() * (max - min) + min)
}
console.log(getRandomInt(1, 2)); // 1
```

```javascript
// [min, max] 之间的随机整数
function getRandomIntInclusive (min, max) {
  return Math.floor(Math.random() * (max - min + 1) + min)
}
console.log(getRandomIntInclusive(1, 2)); // 2
```

## 四：其他常用的函数

- Math.abs()
- Math.pow()
- Math.sqrt()
- Math.cbrt()

### Math.abs()

> **语法：Math.abs(num)**
>
> **作用：取绝对值**
>
> **返回值：num 的绝对值**

```javascript
console.log(Math.abs(-2.5)); // 2.5
console.log(Math.abs(2.5)); // 2.5
```

### Math.pow()

> **语法：Math.pow(base, exponent)**
>
> **作用：返回基数(base)的指数(exponent)幂**
>
> **返回值：指数**

```javascript
console.log(Math.pow(2, 3)); // 8 
console.log(Math.pow(2, 4)); // 16
```

### Math.sqrt()

> **语法：Math.sqrt(num)**
>
> **作用：平方根**
>
> **返回值：num 的平方根**

```javascript
console.log(Math.sqrt(4)); // 2
console.log(Math.sqrt(9)); // 3
```

### Math.cbrt()

> **语法：Math.cbrt(num)**
>
> **作用：立方根**
>
> **返回值：num 的立方根**

```javascript
console.log(Math.cbrt(8)); // 2
console.log(Math.cbrt(27)); // 3
```

