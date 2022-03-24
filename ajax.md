### 1.1简介

> Ajax 全程为Asynchronous JavaScript and XML ，就是异步的js和xml
>
> 通过AJAX可以在浏览器中向服务器发送异步请求，最大的优势：无刷新获取数据



### 1.2XML简介

> XML可扩展标记语言
>
> XML被设计用来传输数据和存储数据
>
> XML和HTML类似，不同的是HTML都是预定义标签，而XML中没有预定义标签，全是自定义标签，用来表示一些数据。

```
//比如有一个学生数据
name="孙悟空"；age="18";gender="男";

用XML表示
<student>
	<name>孙悟空</name>
	<age>18</age>
	<gender>男<gender>
</student>
```

现在已经被JSON格式取代了

```
{
	name:"",
	age:"",
	gender:""
}
```



### 1.3AJAX的特点

#### 1.3.1AJAX的优点

- 可以无刷新页面而与服务器端进行通信
- 允许你根据用户事件来更新部分页面内容

#### 1.3.2AJAX的缺点

- 没有浏览历史，不能回退
- 存在跨域问题（同源）
- seo不友好 （爬虫爬不到数据）

