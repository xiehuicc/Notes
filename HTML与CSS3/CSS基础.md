

### 1.CSS简介

css网页文档：

https://developer.mozilla.org/zh-CN/docs/Web/CSS

### 2.CSS基础选择器

#### 1.标签选择器

#### 2.类选择器

语法：

```
.类名{
	属性值1：属性值1；
	...
}

结构需要class属性来调用
```

<img src="C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210108095354631.png" alt="image-20210108095354631" style="zoom: 80%;" />

类选择器口诀：样式==点==定义，结构==类==调用，一个或多个  开发最常用

#### 3.id选择器

语法：

```
#id{
	属性值1:属性值1；
	...
}
```

==注意：id属性只能在每个HTML文档中出现一次。==

口诀：样式==#==定义，结构==id==调用，只能调用一次，别人切勿使用。

#### 4.通配符选择器

语法：

```
*{
	属性值1：属性值1;
	...
}
```

### 3.CSS字体属性

#### 1.字体系列

```
p{
	font-family:"微软雅黑";
}
div{
	font-family:"Microsoft Yahei";
}
```

单引双引号都行。

#### 2.字体大小

font-size:16px;

**注意**：**标题**需要单独指定文字大小

#### 3.文字样式

font-style:nomal;（可以将斜体装换为默认字体）

nomal:默认值，标准字体样式

italic:斜体

注意：平时很少给文字加斜体，反而要给斜体标签(em,i)改为不倾斜字体。

#### 4.字体复合属性

字体属性

```
body{
	font:font-style font-weight font-size/line-height font-family;
}
```

复合属性：简单的写法，节约代码 不能更改顺序，不需要的属性可以省略，但必须保留font-size 和 font-family属性

font-weight:	加粗时700或者blod，不加粗时400，不用单位。

### 4.CSS文本属性

#### 1.color

#### 2.text-align 

用于设置元素文本内容的水平对齐方式

#### 3.text-indent

文本缩进：text-indent:2em;		(em为相对位，1个em为一个文字大小)

#### 4.text-decoration

文本修饰（取消下划线none）

#### 2.行间距

<img src="file:///C:\Users\谢辉\Documents\Tencent Files\2499248109\Image\C2C\4EA30D116947D353A2347CB8648D80FC.jpg" alt="img" style="zoom: 80%;" />



### 5.CSS引入方式

#### 1.行内样式表

#### 2.内部样式表

#### 3.外部样式表（最多使用）

注意：如果要让图片居中对齐,则需要在其父标签p中设置属性值，在img标签中设置没有用。

```
<p class="属性值"><img src="" /></p>
```

### 6.CSS语法

#### 1.Emmet语法

![image-20210108143106186](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210108143106186.png)

#### 2.复合选择器

##### 1.后代选择器

##### 2.子选择器（重要）

语法

元素1==>==元素2{样式声明}

例如：

div>p {样式声明} 		//选择div里面所有最近一级p标签元素

注意：

1.元素1是父级，元素2是子集，==最总选择的是元素2==

2.元素2必须是亲儿子，其孙子...都不归他管。

#### 3.并集选择器

语法

元素1==，==元素2

#### 4.伪类选择器

##### 1.链接伪类选择器

==重点==（开发时常用）

a{}和a:hover{}

##### 2.focus伪类选择器

焦点为光标

input:focus{

}

### 7.CSS的元素显示模式

#### 1.块元素

常见的块元素有<h1>~<h6>,<p>,<div>,<ul>,<ol>,<li>等	独占一行。

注意：

<p>标签里面不能放块级元素，特别是不能放<div>

#### 2.行内元素

典型：span <a>

行内元素特点:

1.一行可以显示多个。

2.==高，宽直接设置是无效的==。

3.默认宽度就是他内容本身的宽度。

4.行内元素只能容纳文本或其他行内内元素。

注意：

a.链接里面不能再放链接

b. 特殊情况下，链接<a>里面可以放块级元素，但是给<a>转换一下块级模式最安全。

#### 3.行内块元素

行内元素中几个特殊标签：==<img/>,<input/>,<td>==,他们同时具有块元素和行内元素的特点。有些资料也称行内块元素。

特点：==可设置宽度和高度==

#### 4.元素显示模式转换

比如要增加链接<a>的触发范围

-转换为块元素：display:block;

-转换为行内元素：display:inline;

-转换为行内块:display:inline-block;

#### 5.小技巧 单行文字垂直居中代码

css没有提供垂直居中的代码，

解决方案：==让文字的行高等于盒子的高度==，就可以让文字在盒子内居中。

### 8.CSS的背景

#### 1.背景颜色

#### 2.背景图片

```
background-image:none或者url(url);
```

#### 3.背景平铺

```
background-repeat:repeat|no-repeat|repeat-x|repeat-y
```

分别为默认平铺，不平铺，沿着x轴(横向）平铺，沿着y轴平铺。

页面元素既可以添加背景颜色，又可以添加背景图片，不过背景图片会压着背景颜色。

#### 4.背景图片位置

![image-20210109164000756](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210109164000756.png)

如果是精确数值，则第一个代表x，第二个值为y。第二个没写则默认为center

#### 5.背景图像固定（）

语法：

background-attachment:fiixed

#### 6.背景复合写法

当使用简写属性时，没有特定的书写顺序，一般习惯约定顺序为：

==background：背景颜色 背景图片地址 背景平铺 背景图像滚动 背景图片位置；==

#### 7.背景色半透明

语法

```
background:rgba(0,0,0,0.3)
```

最后一个参数是alpha透明度，取值范围在==0~1==之间

### 9.CSS的三大特性

#### 1.层叠行

以离文本最近的样式为最总的样式。

#### 2.继承性

css中的继承，子标签会继承父标签的某些样式，如：text-、font-、line-这些元素开头的可以继承，以及color属性。

行高的继承性

```
body{

	font:12px/1.5 Microsoft Yahei;

}
```

行高可以跟单位也可以不跟单位

1.5为当前文字大小的1.5倍。

#### 3.优级行

1.选择器相同，则根据层叠行。

2.如果选择器不同，则按照选择器的权重（优先级）来。

优先级：

继承或*（0，0，0，0） <元素(标签)选择器（0，0，0，1）<类选择器、伪类选择器（0，0，1，0）<ID选择器（0，1，0，0）<行内样式 style=""（1，0，0，0）<==!important==（无穷大）

==注意：==

​	==继承的权重为0==，如果该元素没有直接选中，不管父元素权重多高，子元素得到的权重都是0。

### 10.盒子模型

#### 边框(border)

https://developer.mozilla.org/zh-CN/docs/Web/CSS/border

#### 内边距(padding)

padding :5px 	//表示上下左右都是5像素

padding : 5px //代表上下边距5像素，左右镖局10边距

padding : 5 px 10px 20px //代表是上5像素，左右内边距10，下内边距为20像素

padding :5px 10px 20px 30px 	//上5，右 10，下20，左30 ，顺时针。

如何盒子已有了高度和宽度，此时在指定内边距，会撑大盒子。

解决方案：让width/heigh减去多出来的内边距即可保证盒子和效果原图大小一致。

**padding的应用：**

![image-20210110144031993](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210110144031993.png)

不会影响盒子撑大的情况：

如果不指定width/height 的属性值，则不会撑大盒子。

#### 外边距(margin)

外边距可以让块级盒子==水平居中==，但必须满足两条件

1，盒子必须制定了宽度

2，盒子==左右的外边距==都设置为==auto==

常见三种写法：

- margin-left:auto; margin-rift:auto;
- margin:auto;
- margin:0 auto;         //常用



==**注意**==:以上方法是让块级元素水平居中，行内元素或者行内块元素水平居中，给其父元素添加text-align:center;即可。

**清除盒子与页面顶部的间隙(内外边距)**：在body中设置样式	==margin:0==和padding:0;

##### 外边距合并

##### 嵌套元素垂直外边距的塌陷 

对于两个嵌套关系（父子关系）的块元素，父元素上外边距同时子元素也有上外边距，此时父元素回塌陷较大的外边距值。

解决方案：

- 可以为父元素定义上边框。border-top:
- 可以为父元素定义上内边距。padding: 
- 可以为父元素添加 overflow:hidden；

#### 圆角边框

border-radius:length;

1、如果盒子为正方形：想要把一个盒子变为圆形，把参数值设为 	高度或者宽度的一半，或者直接写==50%==。

2、如果是一个矩形，设置为高度的一般就可以做![image-20210113113643287](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210113113643287.png)

3、可以设置不同的圆角：该属性可以跟4个不同的值，分别代表左上角，右上角，右下角，左下角（顺时针）

4、分开来写：border-top-left-radius,.....

#### 盒子阴影

box-shadow:

![image-20210113114824216](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210113114824216.png)

![image-20210113114931465](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210113114931465.png)

==注意==：

1、默认的外阴影（outset），但是不可以写这个单词 ，否则导致阴影无效。

2、盒子阴影不占用空间 ，不会影响 其他盒子排列。

#### 文字阴影

text-shadow:



### 11.浮动

#### 1.标准流（普通流/文档流）

#### 2.为什么需要浮动

典型应用：可以让多个块级元素一行内排列显示。

网页布局的第一准则；==多个块级元素纵向排列找标准流，多个块级元素横向排列找浮动==。

#### 3.浮动的特性

1.如果多个盒子都设置了浮动，则它们或按照属性值，**一行显示并且顶端对齐排列**

==注意==：浮动元素是弧形贴靠在一起的（不会有缝隙），如果父级宽度装不下这些浮动的盒子，多出的盒子会**另起一行对齐**

2.浮动元素会具有行内块元素特性。

任何元素都可以浮动，不管原先是什么模式的元素，添加浮动元素后具有==行内块元素相似的特性==

#### 4. 浮动布局注意点

1.浮动和标准流的父盒子搭配

先用标准流的父元素排列上下位置之后，之后内部子元素采取浮动排列左右位置。

2.一个元素浮动了，理论上其它兄弟元素也要浮动

一个盒子里面有多个盒子，如果其中一个盒子浮动了，那么其他兄弟也应该浮动，以防止引起问题

==浮动的盒子只会影响浮动盒子后面的的标准，不会影响前面的标准流==。

#### 5,清除浮动

语法：
选择器{clear:属性值;}

属性值：left、right、both（分别为不允许左侧有浮动，右侧，左右两侧）

##### 1、清除浮动的方法

1、额外标签法

是在浮动元素末尾添加一个空的标签，例如<div style="clear:both"></div>,或者其他标签</br>等

**注意**：==添加的元素必须是块级元素==

2、父级添加overflow

可以给父元素添加overflow属性，将其属性设置为hidden、auto或scroll。

（子不教，父之过）

3、:after 伪元素法

```
clearfix:after {

	content:" ";
	display:block;
	height:0;
	clear:both;
	visibility:hidden;

}
.clearfix{	/* IE6、7专有 */
	*zoom:1;

}
```

.优点：没有增加标签，结构更加简单

缺点：照顾低版本浏览器



4、双伪元素清除浮动

（也是给父元素添加）

```
.clearfix:before,.clearfix:after{
	content:" ";
	display:table;
	
}
.clearfix:after{
	clear:both;
}
.clearfix{
	*zoom:1;
}
```

代表网站：小米，腾讯

##### 2、总结

为什么需要清除浮动？

1.父级没高度。

2，子盒子浮动了。

3.影响下面布局了，我们就应该清除浮动了。

#### 12.PS切图

cutterman 插件



<img src="C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210114225021732.png" alt="image-20210114225021732"  />

### 12.学成在线案例

**页面整体布局思路**

1、必须确定页面的版心（可视区），我们测量得知

2、分析页面中的行模块，以及每一个模块中的列模块。其为页面布局的第一准则

3、一行中的列模块经常浮动布局，先确定每一个列的大小，之后确定列的位置，页面布局第二准则

4、制作HTML结构，我们还是遵循，现有结构，后有样式原则，结构永远最重要

5、所以，先理清楚布局结构，再写代码



**导航栏注意**：

实际开发中，我们不会直接使用==链接a==，而是用li包含链接（==li+a==）的做法。

**注意：**

1、让导航栏一行显示，给li加浮动，因为li是块级元素，需要一行显示。

2、这个nav导航栏可以不给宽度，将来可以继续添加其余文字。

3、因为导航栏里面文字不一样多，最好给链接a左右padding正开盒子，而不是指定宽度。



行内块元素之间有缝隙，可以加浮动去掉缝隙。

浮动的盒子不会有外边距合并问题（塌陷问题）

### 13.定位

定位可以让盒子自由的在某个盒子内移动位置或者固定屏幕中等 而某个位置，并且压住其他盒子。

定位=定位模式+边偏移

定位模式：盒子属于那一中定位（相对、静态、绝对、固定）

#### 1.相对定位（重要）

1、相对原来位置移动

2、占有原先位置，典型应用是给绝对定位当爹的

#### 2、绝对定位（重要）

**特点**

1、没有父级或者父（祖先）级没有定位，则以浏览器为准来移动位置

2、如果父元素有定位，则以父元素为准移动位置。

3、绝对定位==不再占有原先位置==



应用场景：子绝父相

#### 3、固定定位（重要）

页面移动，元素位置不会动

特点：

1、不占有原先位置

2、

固定定位小技巧：固定在版心右侧位置

小算法：

1、让固定定位的盒子left:50%，走

#### 4、粘性定位(sticky)了解

特点：

1、以浏览器的可视窗口为参照点移动元素（固定定位 特点）

2、粘性定位占有原先的位置（相对定位的特点）

3、必须添加top、left、right、bottom其中一个才有效

![image-20210124162253556](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210124162253556.png)

##### 5、定位叠放次序 z-index

1、数值可以是正整数、负整数或0，默认是auto，数值越大，盒子越靠上，但数字后面不能家单位。

如果属性值相同，则按书写顺序 ，后来者居上

#### 6、绝对定位的盒子居中

加了绝对定位的盒子不能通过margin:0 auto水平居中

1、left:50% 让盒子的左侧移动到父级元素的水平位置，

2、margin-left:-100px 让盒子向左移动自身宽度的一半

#### 7、定位的特殊性

1、行内元素添加绝对定位或固定定位，可以直接设置高度和宽度

2、块级元素添加绝对定位或固定定位，如果不给宽度或高度，默认大小是内容的大小



### 14、元素的显示与隐藏

本质：让一个元素在页面中显示或隐藏

##### 1、display属性

display:none ；隐藏对象

display:block; 除了转换为块级元素之外，同时还有显示元素的意思

display 隐藏元素后，不占有元素位置

##### 2、visibility可见性

visibility：hidden；

visibility：visible；

visibility隐藏元素后，继续占有原来的位置

##### 3、overflow溢出

如果有定位的盒子，慎用overflow:hidden；隐藏属性

### 15、CSS高级技巧

#### 1、精灵图

#### 2、字体图标

#### 3、CSS三角

```
div{
	width:0;
	height:0;
	line-height:0;  //为了照顾兼容性
	font-size:0;
	border:60px solid transparent;
	border-left-color:pink;
}
```



####  4、CSS用户界面样式

1、鼠标样式cursor

2、轮廓线outline

3、防止拖拽文本域resize	

4、vertical-align	(应用图片表单和文字居中对齐)

用于设置一个元素的垂直对齐方式，但是只是针对于行内元素或行内块元素有效

vertical-align:baseline|top|middle|bottom

**解决图片底部默认空白缝隙问题**

原因是行内块元素会和文字基线对齐

解决方法：

1、给图片添加vertical-align

2、把图片转换为块级元素 display:block;

#### 5、将溢出的文字省略号显示

**1、单行文本**

必须满足三条件：

1、先强制一行内显示文本

white-space:nowrap;(默认nomal是自动换行)

2、超出的部分隐藏

overflow:hidden;

3、文字用省略号替代超出的部分

text-overflow:ellopsis;

**2、多行文本**



![image-20210126151707857](C:\Users\谢辉\AppData\Roaming\Typora\typora-user-images\image-20210126151707857.png)

#### 6、常用布局技巧

2、文字围绕浮动元素显示

巧妙浮动不会压住文字的特点

3、行内块巧妙运用

### 16.flex布局

> 注意，设为flex布局后，子元素的float，clear和vertical-align属性将失效。

> 采用Flex布局的元素，称为Flex容器（flex container），它的所有子元素自动成为容器成员，称为 Flex 项目（flex item），简称"项目"。

容器的属性有6个，分别是

- flex-direction

  作用：控制主轴的方向

  flex-direction: row | row-reverse | column | column-reverse;

  默认值：row

  

![img](https://user-gold-cdn.xitu.io/2018/12/13/167a5aefae675d7a?imageslim)

- flex-wrap

  作用：默认情况下，项目都排在一条线（又称"轴线"）上。flex-wrap属性定义，如果一条轴线排不下，如何换行。

  flex-wrap: nowrap | wrap | wrap-reverse;

  默认值：nowrap

  ![img](https://user-gold-cdn.xitu.io/2018/12/13/167a5aefae8a2532?imageslim)

- flex-flow

  作用：该属性是flex-direction属性和flex-wrap属性的简写形式

  默认值：row nowrap

- justify-content

  作用：定义项目在主轴上的对齐方式

  justify-content: flex-start | flex-end | center | space-between | space-around;

  默认值：flex-start

  ![img](https://user-gold-cdn.xitu.io/2018/12/13/167a5aefaebc9f3d?imageslim)

- align-items

  作用：定义项目在交叉轴上如何对齐。

  align-items: flex-start | flex-end | center | baseline | stretch

  默认值：flex-start

  ![img](https://user-gold-cdn.xitu.io/2018/12/13/167a5aefb0580b27?imageslim)

注意：如果align-items为stretch，想看到每个flex-item铺满整个交叉轴的话，需要设置所有的flex-item的高度：height:auto，否则达不到效果。

- align-content

  属性定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用。

  align-content: flex-start | flex-end | center | space-between | space-around | stretch;

  默认值：stretch

  flex-start：与交叉轴的起点对齐。

  flex-end：与交叉轴的终点对齐。

  center：与交叉轴的中点对齐。

  space-between：与交叉轴两端对齐，轴线之间的间隔平均分布。

  space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍。

  stretch：轴线占满整个交叉轴。

  ![img](https://user-gold-cdn.xitu.io/2018/12/13/167a5aefaeca4cc4?imageslim)

### 品优购项目设计

#### 1、制作favicon图标

#### 2、网站TDK三大标签SEO优化

seo（搜索引擎优化），目的是对网站进行深度优化，从而帮助网站获取免费流量。

seo三大标签

1、title网站标题

2、description网站说明

3、keywords关键词

#### 3、快捷导航

#### 4、头部制作

**logo seo优化**

1、logo里面放一个h1标签

2、h1里放一个链接，可以发返回首页

3、为了搜索引擎收录我们，我们在链接里面要放文字（网站名称），但是文字不要显示出来。

方法一：text-indent 移动盒子外面(text-indent:-9999px) 然后，overflow:hedden，淘宝做法。

方法二：直接给font-size()；就看不到文字了，京东的做法

4、最后给链接一个title属性，这样鼠标放到logo上就可以看到提示文字