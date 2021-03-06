### 1. 定位

> **文档流介绍**
>
> 简单说就是元素按照其在HTML中的位置顺序决定排布的过程。HTML的布局机制就是用文档流模型的，即块元素（block）独占一行，内联元素（`inline`）不独占一行
>
> 一般使用`margin`用来隔开元素与元素的间距；`padding`是用来隔开元素与内容的间隔，让内容（文字）与（包裹）元素之间有一段“距离”。
>
> 只要不是float和绝对定位方式布局的，都是在文档流里面

**`postition`属性介绍**

- `static`, 默认值。位置设置为static的元素，他始终会处于文档流给予的位置

- `inherit`，规定应该从父元素继承position属性的值。但是任何版本的Internet Explorer （包括 IE8）都不支持属性值 “inherit”。

- `fixed`，生成绝对位的元素。默认情况下，不为元素预留空间，而是通过**指定元素相对于屏幕视口（viewport）的位置**来指定元素位置元素的位置。通过 `left`,`top`,`right`,`bottom`进行规定。不论窗口滚动与否，元素都会留在那个位置。但当祖先元素具有`transform`属性且不为none时，就会**相对于祖先元素**指定坐标进行定位。

- `absoult`,  生成绝对定位元素。**相对于距该元素最近已定位的祖先元素**进行定位。此元素的位置可通过`left`,`top`,`right`,`bottom`属性来规定

- `releative`,  生成相对定位元素。 **相对于该元素在文档中的初始位置进行定位**。通过`left`,`top`,`right`,`bottom`属性来设置此元素相对于自身位置的偏移

  

 #### 1）相对定位

> **relative** 生成相对定位的元素，相对于其正常位置进行定位。

相对定位完成的过程如下：

- 按默认方式（static）生成一个元素（并且元素像浮层一样浮动起来）
- 相对于以前的位置移动，偏移前的位置保留不动
- **相对定位的参照物是它本身。**



#### 2）绝对定位

> 与相对于定位的一大不同就是，把一个元素设置为绝对定位，那么这个元素将会脱离文档流
>
> 其他元素会认为这个元素不存在文档流中，而填充它原来的位置。
>
> 绝对定位元素根据它的参照物移动自己的位置，而参照物则根据它的祖先元素的定位设置来确定
>
> 所谓根据它祖先元素的定位设置来确定简单理解为：相对于该元素最近的已定位的祖先元素，如果没有一个祖先元素设置定位，那么参照物是body层。

