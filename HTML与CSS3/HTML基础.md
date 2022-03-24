

### 1. web标准的构成

##### 包括**结构**、**表现**和**行为**三个方面。

**结构**：对**网页元素**进行整理和分类，现阶段主要学HTML。

**表现**：用于设置网页元素的版式、颜色、大小等**外观样式**，主要指CSS

**行为**：指网页模型的定义及**交互**的编写，现阶段主要学JavaScript。

htm

### 2. HTML常用标签

2.1 加粗 ==<strong></strong>==或<b></b>

​	倾斜==<em></em>==或者<i></i>

​	删除线<del></del>或者 <s></s>

​	下划线<ins></ins>或者<u></u>

#### 2.2 **div** 和**span**

**特点**

**div**用来布局，一行只能放**一个**div。大盒子

**span**一行上可以放**多个**span。小盒子

#### 2.3 图像标签和路径（重点）

**1、图像标签**

```
<img src="图像URL" />
```

**src** 图片路径；

**alt**  ==替换文本==，图像不能显示时用文字替换；

**title** ==提示文本==，鼠标放到图像上，显示的文字；

**height**、**width**、**border**

**2、路径**

相对路径分类

同一级路径：src="img.jpg"	(直接写文件名即可)

下一级路径：src="images/img.jpg"	文件夹名/文件名

上一级路径：src="../img.jpg"	

绝对路径：

#### 2.4 超链接

1.外部链接

2.内部链接

3.空链接

4.下载链接

5.网页元素链接

**6.锚点链接：**点击链接可以快速定位到页面目标位置

​	

```
	<a href="#名字">个人简介</a>
```

页面目标位置标签，调加id属性 	

```
<h3 id="名字">个人简介</h3>
```



#### 2.5表格标签

**1.**

<thead>:用于定义表格头部thead 内部必须有tr标签。

<tbody>：用于定义表格的主体，用于放数据。

以上两个标签都是放在table中的。

2.**合并单元格**

三部曲：1.确幸跨行还是跨列

​				2.找到目标单元格，跨列合并colspan="合并数量"，跨行合并rowspan="合并数量"

​				3.删除多余单元格。

**3.无序列表**

```
<ul>
<li>列表1</li>
<li>列表2</li>
<li>列表3</li>
</ul>
```

注意：**<ul>中只能嵌套<li> 其他标签都不允许**

​			<li></li>之间相当于容器，可以容纳所有元素（可以嵌套其他标签）



4.有序列表：<ol>



**5.自定义列表**

类似结构如下图：

![image-20210107201059650](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210107201059650.png)

基本语法：

```
<dl>
    <dt>名词1</dt>
    <dd>名词1解释</dd>
    <dd>名词1解释</dd>
</dl>
```

#### 2.6表单标签

**1.表单域**

语法

```
<from action="url地址" method="get/post" name="表单域名称">
</from>
```

**2.表单控件**

<input type="属性值">

![image-20210107203104802](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210107203104802.png)

<input>其他属性

checked:给单选框和复选框设置默认值	checked="checked"

maxlength ：规定输入字段 中字符的最大长度

**3.label标签**

语法：

```
<label for="sex">男</label>
<input type="radio" name="sex" id="sex"/>
```

**核心**：<label>标签的**for** 属性应与**id**属性相同

**4.select标签**

语法：

```
<select>
	<option selected="seleted">选项1</option>
	...
</select>
```

selected="seleted"默认被选中