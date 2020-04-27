## 1. 语义化标签

html标签是我们开发前端界面的骨架，但是以前我们使用html标签布局使用的都是div标签，div标签的语义非常的不清晰，因此html5为了规范这一块，给出了一系列的标签：

- `<hrader></header>`  头部区域标签，块级标签
- `<footer></footer>`  底部区域标签，块级标签
- `<nav></nav>` 导航区域标签，块级标签
- `<time></time>` 时间区域标签，内联标签
- `<article></article>` 文章段落标签，块级标签
- `<aside></aside>` 侧边栏区域标签，块级标签
- `<mark></mark>` 标记记号标签，内联标签
- `<summary></summary>` 单词翻译: 摘要，h5官方文档描述：定义 details 元素的标题，块级标签
- `<detailes></detailes>` 单词翻译：细节，h5官方文档描述：定义元素的细节，块级标签
- `<section></section>` 单词翻译：部分，h5官方文档描述：定义 section，块级标签




## 2. 新表单类型

我这里描述的表单，主要指的`<input>`，`<input>`表单标签本身已经有不少类型了，但是h5为了满足开发需求，同样还新增了不少的类型：

- `<input type="email" />`  e-mail 地址的输入域
- `<input type="number" />` 数字输入域
- `<input type="url" />` URL 地址的输入域
- `<input type="range" />` range 类型显示为滑动条，默认value值是1~100的限定范围，可以通过min属性和max属性自定义范围`<input type="range" name="points" min="1" max="10" />`
- `<input type="search" />` 用于搜索域
- `<input type="color" />` 用于定义选择颜色
- `<input type="tel" />` 电话号码输入域
- `<input type="date" />` date类型为时间选择器

HTML5 新增的表单属性

- `placehoder` 属性，简短的提示在用户输入值前会显示在输入域上。即我们常见的输入框默认提示，在用户输入后消失。
- `required`  属性，是一个 boolean 属性。要求填写的输入域不能为空
- `pattern` 属性，描述了一个正则表达式用于验证`<input>` 元素的值。
- `min` 和 `max` 属性，设置元素最小值与最大值。
- `step` 属性，为输入域规定合法的数字间隔。
- `height` 和 `width` 属性，用于 image 类型的 `<input>` 标签的图像高度和宽度。
- `autofocus` 属性，是一个 boolean 属性。规定在页面加载时，域自动地获得焦点。
- `multiple` 属性 ，是一个 boolean 属性。规定`<input>` 元素中可选择多个值。



## 3. 视频和音频

视频`<video>`和音频`<audio>`，也是html提供的新的标签，它们的功能类似于`<img>`标签，`<img>`标签引用的是图片，它们引用的是视频文件和音频文件。不仅如此，html5针对视频文件和音频文件的特殊性，给 `<video>`和`<audio>`提供了非常丰富的方法,属性和事件，用于操控这俩元素。

## 4.Web 存储

如果说离线存储是对web的资源文件存储，那么web 存储就是对应用程序里的数据做存储了。web存储提供了两个存储方式:

- `localStorage`,没有时间限制的数据存储
- `sessionStorage`,就是网页还没有关闭的情况下的存储，网页窗口关闭，则数据销毁。

1. localStorage.setItem('key', 'val') // 存储数据
2. localStorage.getItem('key') // 取数据
3. localStorage.removeItem('key')   // 删除数据
4. localStorage.clear() // 删除所有数据
5. localStorage.key(index)  // 获取某个索引数据的
6. sessionStorage.setItem('key', 'val') // 存储数据
7. sessionStorage.getItem('key') // 取数据
8. sessionStorage.removeItem('key')   // 删除数据 `



## 5. WebSocket

WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。

WebSocket 使得客户端和服务器之间的数据交换变得更加简单，允许服务端主动向客户端推送数据。在 WebSocket API 中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输。

## 6. Web Workers

web worker 是运行在后台的 JavaScript，独立于其他脚本，不会影响页面的性能。您可以继续做任何愿意做的事情：点击、选取内容等等，而此时 web worker 在后台运行。 关于web worker的应用大概分为三个部分：

- 一. 创建 web worker 文件，worker文件是一个单独的js文件，写好逻辑后，通过postMessage()方法吧数据发送出去
- 二. 调用页面创建worker对象，var w = new Worker("worker文件路径").然后通过实例对象调用`onmessage`事件进行监听，并获取worker文件里返回的数据
- 三.终止web worker，当我们的web worker创建后会持续的监听它，需要中止的时候则使用实例上的方法`w.terminate()`。



# **HTMLCollection与NodeList的区别**

`HTMLCollection`和`NodeList`都是`DOM`的节点集合；但是它们两个能够包含的元素是不太一样的，`HTMLCollection`只可以包含`HTML`元素(`Element`)集合，`NodeList`可以包含任意的节点类型，就是说`NodeList`不仅可以包含`HTML`元素集合，也可以包含像文字节点，注释节点等类型的节点集合。

# 为什么getElementsByTagName比querySelectorAll方法快



因为通过`getElementsByTagName`获取到的`NodeList`是一个实时的集合，这种实时的集合，是不需要在一开始的时候就获取到所有的信息的；然而通过`querySelectorAll`方法获取到的的`NodeList`集合是一个静态的集合，这个集合相当于一个快照，就是在这个方法运行的那个时间，它所要获取的集合元素的一个快照，所以这个集合要保存大量的信息，速度自然会慢下来。

为了加深理解，我们可以这样理解：**使用getElementsByTagName方法我们得到的结果就像是一个对象的索引，而通过querySelectorAll方法我们得到的是一个对象的克隆；所以当这个对象数据量非常大的时候，显然克隆这个对象所需要花费的时间是很长的。**

