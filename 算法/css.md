# flex

首先`flex`属性是`flex-grow`，`flex-shrink`和`flex-basis`的缩写。

**1个值**

如果flex的属性值只有一个值，则：

- 如果是数值，例如`flex: 1`，则这个`1`表示`flex-grow`，此时`flex-shrink`和`flex-basis`都使用默认值，分别是`1`和`auto`。
- 如果是长度值，例如`flex: 100px`，则这个`100px`显然指`flex-basis`，因为3个缩写CSS属性中只有`flex-basis`的属性值是长度值。此时`flex-grow`和`flex-shrink`都使用默认值，分别是`0`和`1`。

**2个值**

如果flex的属性值有两个值，则第1个值一定指`flex-grow`

- 如果第2个值是数值，例如`flex: 1 2`，则这个`2`表示`flex-shrink`，此时`flex-basis`使用默认值`auto`。
- 如果第2个值是长度值，例如`flex: 1 100px`，则这个`100px`指`flex-basis`，此时`flex-shrink`都使用默认值`0`。

**3个值**

如果flex的属性值有三个值，则这3个值分别表示`flex-grow`，`flex-shrink`和`flex-basis`。这个顺序该如何记忆，时间久了岂不是很容易忘记。这个告诉大家我的邪恶记忆法，叫做“能大能小的鸡鸡”。grow是增加变大意思，shrink是收缩变小的意思，而basis是基（鸡）础基（鸡）准的意思，于是连起来就是“能大能小的鸡鸡”。

**initial**

初始值。`flex:initial`等同于设置`"flex: 0 1 auto"`。可以理解为flex属性的默认值。任意打开一个页面，我们打开控制台执行下面的代码：`window.getComputedStyle(document.body).flex; // 结果就是"flex: 0 1 auto"`也就是`flex`属性默认值是`"0 1 auto"`，等同于`initial`关键字的计算值。

**auto**

`flex:auto`等同于设置`"flex: 1 1 auto"`。

**none**

`flex:none`等同于设置`"flex: 0 0 auto"`。

### 作用在flex容器上的CSS属性

#### 1. flex-direction

`flex-direction`用来控制子项整体布局方向，是从左往右还是从右往左，是从上往下还是从下往上。和CSS的[direction属性](https://www.zhangxinxu.com/wordpress/2016/03/css-direction-introduction-apply/)相比就是多了个`flex`。

语法如下：

```
flex-direction: row | row-reverse | column | column-reverse;
```

#### 2. flex-wrap

`flex-wrap`用来控制子项整体单行显示还是换行显示，如果换行，则下面一行是否反方向显示。这个属性比较好记忆，在CSS世界中，只要看到单词wrap一定是与换行显示相关的，`word-wrap`属性或者`white-space:nowrap`或者`pre-wrap`之类。

语法如下：

```
flex-wrap: nowrap | wrap | wrap-reverse;
```

#### 3. flex-flow

`flex-flow`属性是`flex-direction`和`flex-wrap`的缩写，表示flex布局的flow流动特性，语法如下：

```
flex-flow: <‘flex-direction’> || <‘flex-wrap’>
```

#### 4. justify-content

`justify-content`属性决定**了水平方向子项的对齐和分布方式**。CSS `text-align`有个属性值为`justify`，可实现[两端对齐](https://www.zhangxinxu.com/wordpress/2015/08/chinese-english-same-padding-text-justify/)，所以，当我们想要控制flex元素的水平对齐方式的时候，记住`justify`这个单词，`justify-content`属性也就记住了。

`justify-content`可以看成是`text-align`的远房亲戚，不过前者控制flex元素的水平对齐外加分布，后者控制内联元素的水平对齐。

语法如下：

```
justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly;
```

其中：

- flex-start

  默认值。逻辑CSS属性值，与文档流方向相关。默认表现为左对齐。

- flex-end

  逻辑CSS属性值，与文档流方向相关。默认表现为右对齐。

- center

  表现为居中对齐。

- space-between

  表现为两端对齐。between是中间的意思，意思是多余的空白间距只在元素中间区域分配。

  ![space-between分布效果示意](https://image.zhangxinxu.com/image/blog/201810/space-between.svg)

- space-around

  around是环绕的意思，意思是每个flex子项两侧都环绕互不干扰的等宽的空白间距，最终视觉上边缘两侧的空白只有中间空白宽度一半。使用抽象图形示意如下：![space-around分布效果示意](https://image.zhangxinxu.com/image/blog/201810/space-around.svg)

- space-evenly

  evenly是匀称、平等的意思。也就是视觉上，每个flex子项两侧空白间距完全相等。使用抽象图形示意如下：![space-evenly分布效果示意](https://image.zhangxinxu.com/image/blog/201810/space-evenly.svg)

#### 5. align-items

`align-items`中的`items`指的就是flex子项们，因此`align-items`指的就是flex子项们相对于flex容器在**垂直方向上的对齐方式**，大家是一起顶部对齐呢，底部对齐呢，还是拉伸对齐呢，类似这样。

语法如下：

```
align-items: stretch | flex-start | flex-end | center | baseline;
```

### 作用在flex子项上的CSS属性

#### 6. align-self

`align-self`指控制单独某一个flex子项的垂直对齐方式，写在flex容器上的这个`align-items`属性，后面是items，有个s，表示子项们，是全体；这里是self，单独一个个体。其他区别不大，语法几乎一样：

```
align-self: auto | flex-start | flex-end | center | baseline | stretch;
```



# grid

### 作用在grid容器上的CSS属性

#### 1. grid-template-columns和grid-template-rows

这两个CSS属性用来对田地进行基本的划分，columns是列的意思，表示竖直方向的划分；rows是行的意思，表示水平方向的划分。

看一个简单例子：

```
.container {
    grid-template-columns: 80px auto 100px;
    grid-template-rows: 25% 100px auto 60px;
}
```

`grid-template-columns`后面3个值，表示分为了3列，从左往右每列的尺寸分别是`80px`，auto（自动）和`100px`；
`grid-template-rows`属性值含4个值，表示分为了4行，从上往下每行的高度分别是`25%`，`100px`，auto（自动）和`60px`。

![基本尺寸划分示意图](https://image.zhangxinxu.com/image/blog/201811/2018-11-04_210143.png)

**（1）repeat()**

有时候，重复写同样的值非常麻烦，尤其网格很多时。这时，可以使用`repeat()`函数，简化重复的值。上面的代码用`repeat()`改写如下。

> ```css
> .container {
>   display: grid;
>   grid-template-columns: repeat(3, 33.33%);
>   grid-template-rows: repeat(3, 33.33%);
> }
> ```

`repeat()`接受两个参数，第一个参数是重复的次数（上例是3），第二个参数是所要重复的值。

`repeat()`重复某种模式也是可以的。

> ```css
> grid-template-columns: repeat(2, 100px 20px 80px);
> ```

#### 2. grid-template-areas

网格布局允许指定"区域"（area），一个区域由单个或多个单元格组成。`grid-template-areas`属性用于定义区域。

> ```css
> .container {
>   display: grid;
>   grid-template-columns: 100px 100px 100px;
>   grid-template-rows: 100px 100px 100px;
>   grid-template-areas: 'a b c'
>                        'd e f'
>                        'g h i';
> }
> ```

上面代码先划分出9个单元格，然后将其定名为`a`到`i`的九个区域，分别对应这九个单元格。

如果某些区域不需要利用，则使用"点"（`.`）表示。

> ```css
> grid-template-areas: 'a . c'
>                      'd . f'
>                      'g . i';
> ```

上面代码中，中间一列为点，表示没有用到该单元格，或者该单元格不属于任何区域。

#### 3.grid-gap 属性

`grid-row-gap`属性设置行与行的间隔（行间距），`grid-column-gap`属性设置列与列的间隔（列间距）。

> ```css
> .container {
>   grid-row-gap: 20px;
>   grid-column-gap: 20px;
> }
> ```

上面一段 CSS 代码等同于下面的代码。

> ```css
> .container {
>   grid-gap: 20px 20px;
> }
> ```

如果`grid-gap`省略了第二个值，浏览器认为第二个值等于第一个值。

#### 4.justify-items 属性， align-items 属性， place-items 属性（设置**单元格内容**）

`justify-items`属性设置**单元格内容**的水平位置（左中右），`align-items`属性设置单元格内容的垂直位置（上中下）。

> ```css
> .container {
>   justify-items: start | end | center | stretch;
>   align-items: start | end | center | stretch;
> }
> ```

这两个属性的写法完全相同，都可以取下面这些值。

> - start：对齐单元格的起始边缘。
> - end：对齐单元格的结束边缘。
> - center：单元格内部居中。
> - stretch：拉伸，占满单元格的整个宽度（默认值）。

`place-items`属性是`align-items`属性和`justify-items`属性的合并简写形式。

> ```css
> place-items: <align-items> <justify-items>;
> ```

#### 5.justify-content 属性， align-content 属性， place-content 属性

`justify-content`属性是整个内容区域在容器里面的水平位置（左中右），`align-content`属性是**整个内容区域的垂直位置**（上中下）。

> ```css
> .container {
>   justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
>   align-content: start | end | center | stretch | space-around | space-between | space-evenly;  
> }
> ```

这两个属性的写法完全相同，都可以取下面这些值。（下面的图都以`justify-content`属性为例，`align-content`属性的图完全一样，只是将水平方向改成垂直方向。）

`place-content`属性是`align-content`属性和`justify-content`属性的合并简写形式。

> ```css
> place-content: <align-content> <justify-content>
> ```

如果省略第二个值，浏览器就会假定第二个值等于第一个值。

### 作用在grid子项上的CSS属性

#### 1.grid-column-start 属性， grid-column-end 属性， grid-row-start 属性， grid-row-end 属性

项目的位置是可以指定的，具体方法就是指定项目的四个边框，分别定位在哪根网格线。

> - `grid-column-start`属性：左边框所在的垂直网格线
> - `grid-column-end`属性：右边框所在的垂直网格线
> - `grid-row-start`属性：上边框所在的水平网格线
> - `grid-row-end`属性：下边框所在的水平网格线

> ```css
> .item-1 {
>   grid-column-start: 2;
>   grid-column-end: 4;
> }
> ```

![img](https://www.wangbase.com/blogimg/asset/201903/bg2019032526.png)

#### 2.grid-column 属性， grid-row 属性

`grid-column`属性是`grid-column-start`和`grid-column-end`的合并简写形式，`grid-row`属性是`grid-row-start`属性和`grid-row-end`的合并简写形式。

> ```css
> .item {
>   grid-column:  / ;
>   grid-row:  / ;
> }
> ```

下面是一个例子。

> ```css
> .item-1 {
>   grid-column: 1 / 3;
>   grid-row: 1 / 2;
> }
> /* 等同于 */
> .item-1 {
>   grid-column-start: 1;
>   grid-column-end: 3;
>   grid-row-start: 1;
>   grid-row-end: 2;
> }
> ```

上面代码中，项目`item-1`占据第一行，从第一根列线到第三根列线。

这两个属性之中，也可以使用`span`关键字，表示跨越多少个网格。

> ```css
> .item-1 {
>   background: #b03532;
>   grid-column: 1 / 3;
>   grid-row: 1 / 3;
> }
> /* 等同于 */
> .item-1 {
>   background: #b03532;
>   grid-column: 1 / span 2;
>   grid-row: 1 / span 2;
> }
> ```

[上面代码](https://jsbin.com/volugow/edit?html,css,output)中，项目`item-1`占据的区域，包括第一行 + 第二行、第一列 + 第二列。

![img](https://www.wangbase.com/blogimg/asset/201903/bg2019032529.png)

斜杠以及后面的部分可以省略，默认跨越一个网格。

> ```css
> .item-1 {
>   grid-column: 1;
>   grid-row: 1;
> }
> ```

#### 3 grid-area 属性

`grid-area`属性指定项目放在哪一个区域。

> ```css
> .item-1 {
>   grid-area: e;
> }
> ```

#### 4.justify-self 属性， align-self 属性， place-self 属性

`justify-self`属性设置单元格内容的水平位置（左中右），跟`justify-items`属性的用法完全一致，但只作用于单个项目。

`align-self`属性设置单元格内容的垂直位置（上中下），跟`align-items`属性的用法完全一致，也是只作用于单个项目。

> ```css
> .item {
>   justify-self: start | end | center | stretch;
>   align-self: start | end | center | stretch;
> }
> ```

这两个属性都可以取下面四个值。

> - start：对齐单元格的起始边缘。
> - end：对齐单元格的结束边缘。
> - center：单元格内部居中。
> - stretch：拉伸，占满单元格的整个宽度（默认值）。

下面是`justify-self: start`的例子。

> ```css
> .item-1  {
>   justify-self: start;
> }
> ```

![img](https://www.wangbase.com/blogimg/asset/201903/bg2019032532.png)

`place-self`属性是`align-self`属性和`justify-self`属性的合并简写形式。

> ```css
> place-self: <align-self> <justify-self>;
> ```

下面是一个例子。

> ```css
> place-self: center center;
> ```

# CSS 解决img底部空白间隙

行内元素默认的垂直对齐方式为baseline造成的字体下方会有间隙

inline默认的垂直对齐方式vertical-align默认值是baseline(基线对齐)

**第一种方法：**修改img行内元素的垂直居中方式，让它不在以基线对齐。

```css
img {
    vertical-align: bottom;
}
```

**第二种方法：**修改行高，使行高变小，这样基线下方的位置基本可以忽略。

```css
div {
    line-height: 0px;
}
```

**第三种方法：**修改img行内元素的字体大小，基线的下方间隙是部分字体超过基线下方而产生的，如果把父元素的`font-size`变的超小，基线的下方距离将忽略不计。

```css
div {
    font-size: 0px;
}
```

**第四种方法：**直接让img变成块级元素，不在受行内基线的影响。

```css
img {
    display: block;
}

/* 浮动也可以让元素变成块级 */
img {
    float：left;
}

/* 只要能变成块级的属性都可以 */
...
```

# 选择器

> 选择器简介

- 用于匹配`HTML`元素
- 分类和权重
- 解析方式和性能 
  - 浏览器的解析方式是从右往左反着，然后再往前验证,主要出于性能的考虑，更快速的匹配到指定元素(左边范围太广了，比如`<div>`可能能找到几百个)
- 值得关注的选择器

> 选择器分类

- 元素选择器                	 `a{}`
- 伪元素选择器            `::before{}`                //真实存在的容器
- 类选择器                	    `.link{}`
- 属性选择器            		`[type=radio]{}`
- 伪类选择器            		`:hover{}`       //元素的状态
- `ID`选择器              		`#id{}`
- 组合选择器            		`[type=checkbox]` + `label{}`
- 否定选择器            		`:not(.link){}`
- 通用选择器            		`*{}`

> 选择器权重

- ID选择器器            		     +100
- 类   属性   伪类器      	 +10
- 元素 伪元素器        	     +1
- 其它选择器器         	     +0

> 计算一个不进位的数字

```
#id` `.link` `a[href]
```

计算：

- `#id`         		+100
- `.link`       	+10
- `a`                     +1
- `[href]`      +0

结果：111

```
#id .link.active
#id       +100
.link 	  +10
.active   +10
结果：120
复制代码
```

> 不进位

```
只要有id选择器，就是最大，类选择器再多也不会进位，只能在自己位上
```

百位    十位    个位

> 其他选择器权重

- `!important`优先级最高
- 元素属性优先级高   //`元素的属性` > `外部样式表 (style标签，外部样式表)`
- 相同权重后写的生效

# BFC

BFC(Block formatting context)直译为"块级格式化上下文"。它是一个独立的渲染区域，

**BFC布局规则** BFC 就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。

- BFC这个元素的垂直方向的边距会发生重叠，垂直方向的距离由`margin`决定，取最大值
- BFC 的区域不会与浮动盒子重叠（`清除浮动原理`）。
- 计算 BFC 的高度时，浮动元素也参与计算。
- 根据 BFC 的定义，两个元素只有在同一 BFC 内，才有可能发生垂直外边距的重叠

**哪些元素会生成 BFC**

- 根元素
- float 属性不为 none
- position 为 absolute 或 fixed
- display 为 inline-block， table-cell， table-caption， flex， inline-flex
- overflow 不为 visible




# link和@import的区别？

```swift
（1）link属于XHTML标签，而@import是CSS提供的。
（2）页面被加载时，link会同时被加载，而@import引用的CSS会等到页面被加载完再加载。
（3）import只在IE 5以上才能识别，而link是XHTML标签，无兼容问题。
（4）link方式的样式权重高于@import的权重。
（5）使用dom控制样式时的差别。当使用javascript控制dom去改变样式的时候，只能使用link标签，因为@import不是dom可以控制的。
```

# px，em，rem 的区别

**px** 像素(Pixel)。绝对单位。像素 px 是`相对于显示器屏幕分辨率`而言的，是一个虚拟长度单位，是计算 机系统的数字化图像长度单位，如果 px 要换算成物理长度，需要指定精度 DPI。

**em** 是相对长度单位，`相对于当前对象内文本的字体尺寸`。如当前对行内文本的字体尺寸未被人为设置， 则相对于浏览器的默认字体尺寸。它会继承父级元素的字体大小，因此并不是一个固定的值。

**rem** 是 CSS3 新增的一个相对单位(root em，根 em)，使用 rem 为元素设定字体大小时，仍然是相对大小， 但`相对的只是 HTML 根元素`。



# margin塌陷和重叠现象

当父div没有border和padding时候，父子div之间的margin会合并，产生margin塌陷。
解决方法：
（1）为父盒子设置border，为外层添加border后父子盒子就不是真正意义上的贴合。
（2）为父盒子添加overflow：hidden；
（3）为父盒子设定padding值。
（4）给父子元素之间加入一个空白div

# **display:inline-block 什么时候会显示间隙？**

1. 有空格时候会有间隙 解决：移除空格
2. margin正值的时候 解决：margin使用负值
3. 使用font-size时候 解决：font-size:0、letter-spacing、word-spacing

# 元素隐藏方法总结：

如果希望元素不可见、不占据空间、资源会加载、DOM 可访问： `display: none`；

如果希望元素不可见、不能点击、但占据空间、资源会加载，可以使用： `visibility: hidden`；

如果希望元素不可见、可以点击、占据空间，可以使用： `opacity: 0`；

如果希望元素不可见、不能点击、占据空间，可以使用： `position: relative; z-index: -1;`；

如果希望元素不可见、不能点击、不占据空间，可以使用： `position: absolute ; z-index: -1;`；

可相对与已被定位的父元素进行定位，逐级向上直到找到为止。

position: absolute;绝对定位：绝对定位是相对于元素最近的已定位的祖先元素（即是设置了绝对定位或者相对定位的祖先元素）。如果元素没有已定位的祖先元素，那么它的位置则是相对于最初的包含块（body）