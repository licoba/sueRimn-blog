# 元素居中

### 一、元素分类

首先要知道元素分三种：内联元素（行内元素）、块级元素、内联块级元素。

#### 1、内联（行内）元素

* 可与其他元素占一行
* 高、行高、内外边距不可更改
* 宽度为内容所占宽度，不可更改
* 容纳文本或其他行内元素

#### 2、块级元素

* 独占一行
* 高、行高、内外边距可更改
* 不设置宽度的话宽度默认为容器的100%
* 可容纳行内元素和块级元素

#### 3、内联块级元素

* 可与其他元素在一行
* 高、宽、行高以及上下边距可更改

常见内联（行内）元素：

* a - 锚点
* b - 粗体
* br - 换行
* cite - 引用
* code - 计算机代码
* em - 强调
* i - 斜体
* img - 图片
* input - 输入框
* label - 表格标签
* span - 常用内联容器，定义文本内区块
* strike - 中划线
* strong - 粗体强调
* sub - 下标
* sup - 上标
* textarea - 多行文本输入框

常见块级元素：

* address - 联系方式
* audio - 音频
* vedio - 视频
* article - 文章
* blockquote - 块引用
* div - 文档分区
* form - 表单
* section - 页面区段
* h1 - 大标题
* h2 - 副标题
* h3 - 3级标题
* h4 - 4级标题
* h5 - 5级标题
* h6 - 6级标题
* hr - 水平分隔线
* header - 页头
* menu - 菜单列表
* ol - 排序列表
* ul - 非排序列表
* p - 段落
* table - 表格

常见的内联块级元素有：

* img - 图片
* input - 输入

比如：

```text
img{display: inline-block;}
```

### 二、水平居中

#### 1、内联元素水平居中

**（1）父级为块级元素的内联元素使用text-align: center**

```text
p{
  background-color: cyan;
  text-align: center;
}
<p>我是内联文本</p>
```

可以看到块级元素`p`是默认宽度是整个容器的100%，并且其容纳的文本是内联元素，使用`text-align: center`让其水平居中。

`text-align: center`仅对内联元素有效，包括内联块级元素。比如对`inline, inline-block, inline-table, inline-flex`的元素同样有效，因为`inline-block`值将内部`div`显示为嵌入式元素和块，因此外部`div`中的属性`text-align`将内部`div`居中。

#### 2、块级元素水平居中

**（1）单个块使用margin: 0 auto**

有时需要居中的不是文本，而是整个块。我们希望左右边距相等，方法是将边距设置为“auto”。

这通常用于**固定宽度**的块，如果块本身是灵活的，它将占用所有宽度，当然，居中效果可能就不明显了，因为它占了整个宽度。

无论正在居中的块级别元素或父元素的宽度是多少，这都是有效的。

```text
.block {
    width: 200px;
    margin: 0 auto;
    background-color: aquamarine;
}
<div class="block">我是一个块，设宽度可见居中效果，不设宽度占满整个可用宽度</div>
```

**（2）父级为块级元素的块级元素水平居中**

**1）使用table布局加左右边距自适应**

因为块级元素设为表格布局之后，将块级变为了内部单元格

```text
.child-3 {
    display: table;
    margin: 0 auto;
}

<div class="child">块级元素(display:table) + (margin:0 auto)</div>
```

**2）多个块级元素在一行居中**

当想让多个块级元素在一行内居中时，有两种办法：

**第一种：将块级元素变为内联块级元素：display: inline-block**

当把多个块级元素变为内联块级元素时，就可以使用内联元素居中的办法`text-align: center;`居中了：

```text
.box {
    text-align: center;
    background-color: aquamarine;
}
.inline-block {
    width: 100px;
    height: 50px;
    border: 1px #333 solid;
    display: inline-block;
}
<div class="box">
    <p class="inline-block">块级1</p>
    <p class="inline-block">块级2</p>
    <p class="inline-block">块级3</p>
</div>
```

给每个块级元素设置宽高只是为了显示效果。

**第二种：使用flex布局**

```text
.box {
    display: flex;
    justify-content: center;
}
.box-child {
    width: 100px;
    height: 50px;
    text-align: center; // 是为了把块级元素中的内联文本居中
    border: 1px #333 solid;
}

<div class="box">
    <p class="box-child">块级1</p>
    <p class="box-child">块级2</p>
    <p class="box-child">块级3</p>
</div>
```

**3）多个块级元素在一列居中**

这个也属于块级中的块级水平居中。

```text
.box p{
    margin: 0 auto;
}
.box-child {
    width: 100px;
    height: 50px;
    text-align: center; // 是为了把块级元素中的内联文本居中
    border: 1px #333 solid;
}
<div class="box">
    <p class="box-child">块级1</p>
    <p class="box-child">块级2</p>
    <p class="box-child">块级3</p>
</div>
```

**（3）定位块级元素：父相子绝 + transform**

父元素使用相对定位，子元素使用绝对定位。

使用了相对定位，就有指定的`top`，`bottom`，`left`，和`right`。子元素的对象使用绝对定位是以设置相对定位的父元素为基础偏移的，如果没有设置相对定位的父元素，就以文档为基础进行偏移。

由于绝对定位是脱离文档流的，最好尽量避免使用。

在对使用绝对定位的元素居中时，如果该元素有固定大小，在偏移后，仅需使用边距进行补偿即可。步骤如下：

* 需居中元素设置`left: 50%`，这使该元素的左边缘与父元素`50%`的线对齐。
* 添加一个负边距，大小为元素宽度的一半，使得该元素的一半往左移动，中心点与父元素`50%`线对齐。

使用`transform`时，需要兼容浏览器。

```text
.parent {
    position: relative;
}
/* 注意兼容 */
.child-4-1 {
    position: absolute;
    left: 50%;
    -webkit-transform: translateX(-50%);
    -moz-transform: translateX(-50%);
    -ms-transform: translateX(-50%);
    -o-transform: translateX(-50%);
    transform: translateX(-50%)
}
<div class="parent">
	<div class="child">定位块级元素(父相子绝)</div>
</div>
```

#### 3、浮动元素水平居中

浮动`float`是`CSS`中最常用的布局技术，当使用`float: left`设置元素样式时，之后元素将重新排版。文档流是指内容的顺序以及元素之间如何排列。如果被浮动的元素没有设置宽度，它将折叠为内容的宽度。如果之后的元素比剩余的空间更小，它将向右移动。请记住，设置为`display: block`的元素需要有一个固定宽度，否则它将独占一行。

可以通过在样式表中为元素提供一个`clear`属性来防止元素重新排版。比如`clear: both`、`clear: left`和`clear: right`，还可以使用`clear: none`来覆盖默认行为。

因为被浮动的子元素会导致父元素崩溃，所以你会很想同时创建新元素来添加`clear:`防止这种行为。虽然这是可行的，但我们希望保持标记的语义性，因此应该只使用`CSS`来实现。通过使用伪元素`::after`可以创建一个清除浮动`clearfix`：

```text
.container::after {
    clear: both;
    content: '';
    display: table;
}
```

浮动适合大型容器，不太适用于文本元素，因为它很难对齐。使用`display: inline-block`更适合，请参考上方的内联元素水平居中。

想要是浮动的元素居中怎么办呢，只有向左向右浮动，没有向中间浮动，这需要一个中间线，所以设置给浮动元素设置一个父元素，以父元素的中间线为居中标准即可。

**（1）浮动元素指定宽：位相 + 负宽一半margin**

如果浮动元素需要指定宽度，则在浮动的时候需要以宽度的一半值为负边距补偿。

```text
.child {
    position: relative;
    float: left;
    width: 250px;
    left: 50%;
    margin-left: -125px;
}
<div class="parent">
    <div class="child">
    	指定宽度：子位相左浮，负一半宽度margin
    </div>
</div>
```

**（2）浮动元素不指定宽：父子位相左浮，左右各50%**

```text
.parent {
    position: relative; // 位相
    float: left;        // 左浮
    left: 50%;		   // 左50%
}
.child-2-2--p {
    position: relative; // 位相
    float: left;        // 左浮
    right: 50%;		   // 右50%
}

<div class="parent">
    <div class="child">
    	不指定宽度：父子位相左浮，左右各50%
    </div>
</div>
```

**（3）浮动元素通用（有无定宽皆可）：flex**

```text
.parent {
    display:flex;
    justify-content:center;
}
.chlid{
    float: left;
}
<div class="parent">
  <span class="chlid">有无定宽皆可： flex</span>
</div>
```

以下是例子效果：  
[![](https://camo.githubusercontent.com/66bb068264f2874861f258ebebab82cf50713043/68747470733a2f2f7365676d656e746661756c742e636f6d2f696d672f625662796579383f773d36323526683d383436)](https://camo.githubusercontent.com/66bb068264f2874861f258ebebab82cf50713043/68747470733a2f2f7365676d656e746661756c742e636f6d2f696d672f625662796579383f773d36323526683d383436)

### 三、垂直居中

#### 1、内联元素垂直居中

**（1）单行内联元素**

**1）使用填充**

有时候，内联/文本元素可以垂直居中显示，仅仅是因为它们的上下填充相同。

```text
 .parent {
    padding-top: 10px;
    padding-bottom: 10px;
}

.child {
    background-color: cornflowerblue;
    display: flex;
    flex-direction: column;
    justify-content: center;
    height: 50px;
}
<div class="parent">
	<span class="child">1、单行行内元素使用上下填充相同实现垂直居中</span>
</div>
```

**2）使用行高**

如果没有填充可以选择时，可以使用行高

```text
.parent {
    background-color: aquamarine;
}
.child {
    height: 50px;
    line-height: 50px;
}

<div class="parent">
	<span class="child">1、单行行内元素使用行高实现垂直居中</span>
</div>
```

**（2）多行内联元素**

**1）使用dispaly:table**

多行内联元素也可以使用上下相同的填充实现居中效果，但如果这不起作用，可能文本所在的元素可以是一个表格单元格，或者是字面上的表格单元格，亦或者是用`CSS`创建的表格单元格。

```text
.parent {
    height: 100px;
    background-color: cyan;
    display: table;
}

.child {
    display: table-cell;
    vertical-align: middle;
}

<div class="parent">
	<p class="child">3、多行行内元素父级使用dispaly:table (Lorem ipsum, dolor sit amet consectetur adipisicing elit.
Vel, quos ratione, ea
accusantium voluptas )</p>
</div>
```

**2）使用flex**

记住，只有父容器有固定高度时，才可以实现。

```text
.parent {
    background-color: cornflowerblue;
    display: flex;
    flex-direction: column;
    justify-content: center;
    height: 100px;
}

<div class="parent">
	<p class="child">4、多行行内元素父级使用flex-direction:column (Lorem ipsum, dolor sit amet consectetur
adipisicing elit.
Vel, quos ratione, ea
accusantium voluptas )</p>
</div>
```

**3）使用伪元素**

```text
.parent {
    position: relative;
    background-color: thistle;
}
.parent::before {
    content: '';
    vertical-align: middle;
}
.child {
    display: inline-block;
    vertical-align: middle;
}

<div class="child-pseudo">
	<p class="child-pseudo--p">5、多行行内元素父级使用伪元素 (Lorem ipsum, dolor sit amet consectetur
adipisicing elit.
Vel, quos ratione, ea
accusantium voluptas )</p>
</div>
```

#### 2、块级元素垂直居中

**（1）定高：父相子绝 + 上50% + 负上高度一半margin**

```text
.parent {
    position: relative;
    background-color: coral;
}
.child {
    position: absolute;
    height: 25px;
    top: 50%;
    margin-top: -12.5px;
}
<div class="parent">
	<div class="child">1、父位相，子绝+指定高度+top50%+负高度一半margin</div>
</div>
```

**（2）高度未知： 三种方法**

**1）父相子绝 + 上50% + 上偏移50%**

```text
.parent {
    position: relative;
    background-color: coral;
}
.child {
    position: absolute;
    top: 50%;
    -webkit-transform: translateY(-50%);
    -moz-transform: translateY(-50%);
    -ms-transform: translateY(-50%);
    -o-transform: translateY(-50%);
    transform: translateY(-50%);
}
<div class="parent">
	<div class="child">1、父位相，子绝+指定高度+top50%+负高度一半margin</div>
</div>
```

**2）父级table布局 + 子table-cell + vertical-align: middle**

```text
.parent {
    display: table;
}
.child {
    display: table-cell;
    vertical-align: middle;
}
<div class="parent">
	<div class="child">高度未知：父级table-cell + 子vertical-align: middlemargin</div>
</div>
```

**3）父级flex + align-items:center**

```text
.parent {
    display: flex;
    align-items: center;
}
<div class="parent">
	<div class="child">高度未知：父级flex+align-items:center</div>
</div>
```

**4）父级flex+flex-direction + justify-content**

```text
.parent {
    display: flex;
    flex-direction: column;
    justify-content: center;
}
<div class="parent">
	<div class="child">高度未知：父级flex+flex-direction + justify-content</div>
</div>
```

以下是垂直居中所有效果：

[![](https://camo.githubusercontent.com/d333ce8421dd22ee4882db93c0c6de7913cbc7d6/68747470733a2f2f7365676d656e746661756c742e636f6d2f696d672f62566279657a633f773d35363226683d383033)](https://camo.githubusercontent.com/d333ce8421dd22ee4882db93c0c6de7913cbc7d6/68747470733a2f2f7365676d656e746661756c742e636f6d2f696d672f62566279657a633f773d35363226683d383033)

### 四、水平垂直居中

#### 1、定宽高 ： 父相+子绝左上50%负margin左上负一半高度

```text
.parent {
    position: relative;
}

.child {
    width: 500px;
    height: 100px;

    position: absolute;
    top: 50%;
    left: 50%;

    margin: -50px 0 0 -250px;
}
<div class="parent">
	<div class="child">父相子绝 + transform</div>
</div>
```

#### 2、宽高未知：父相子绝 + transform

```text
.parent {
    position: relative;
}

.child {
    position:absolute;
    left:50%; 
    top:50%;
    transform: translate(-50%,-50%); 
}
<div class="parent">
	<div class="child">父相子绝 + transform</div>
</div>
```

#### 3、父flex + justify-content + align-items

```text
.parent {
    display: flex;
    justify-content: center;
    align-items: center;
}
<div class="parent">
	<div class="child">父flex + justify-content + align-items</div>
</div>
```

#### 4、父grid/flex + 子magin:auto

```text
.parent {
    /* display: flex; */
    display: grid;
    height: 50px;
    background-color: darksalmon;
}
.child {
    margin: auto;
}
<div class="parent">
	<div class="child">父grid/flex + 子magin:auto</div>
</div>
```

#### 5、父table-cell布局 + vertical-align + text-align

```text
.parent {
    display: table-cell;
    vertical-align: middle;
    text-align: center;
}

<div class="parent">
	<div class="child">父table-cell布局 + vertical-align + text-align</div>
</div>
```

#### 6、父相子绝 + 上下左右0 + margin:auto

```text
.parent {
    position: relative;
}

.child {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    margin: auto;
}

<div class="parent">
	<div class="child">父相子绝 + 上下左右0</div>
</div>
```

以下是例子效果：  
[![](https://camo.githubusercontent.com/24bdf6649677c62136efcdf8c64e8077ce8fd201/68747470733a2f2f7365676d656e746661756c742e636f6d2f696d672f62566279657a663f773d35313826683d333135)](https://camo.githubusercontent.com/24bdf6649677c62136efcdf8c64e8077ce8fd201/68747470733a2f2f7365676d656e746661756c742e636f6d2f696d672f62566279657a663f773d35313826683d333135)

> 参考：
>
> [centering-css-complete-guide](https://css-tricks.com/centering-css-complete-guide/)
>
> [Positioning Elements on the Web](https://thoughtbot.com/blog/positioning)

