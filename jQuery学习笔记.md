# jQuery学习笔记

*目录*

[TOC]

## 概述

### 什么是jQuery

jQuery 是一个 JavaScript 框架，它对一些常用功能进行封装，提供简便的操作方式，实现所谓的“用更少的代码，做更多的事情。”

### 使用

jQuery 是 JavaScript 库，因此可以在 HTML 文件中用 `<script>` 标签引入。

```html
<head>
  <script src="js/jquery-2.2.2.min.js"></script>
</head>
```

或者从 CDN （内容分发网络）[^1]上直接引用：

```html
<!-- 百度 CDN -->
<script src="http://libs.baidu.com/jquery/1.10.2/jquery.min.js"></script>

<!-- 新浪 CDN -->
<script src="http://lib.sinaapp.com/js/jquery/2.0.2/jquery-2.0.2.min.js"></script>

<!-- Google CDN -->
<script src="http://ajax.googleapis.com/ajax/libs/jquery/1.10.2/jquery.min.js"></script>

<!-- 微软 CDN -->
<script src="http://ajax.htmlnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js"></script>
```

*\* 注意：引用文件名中的数字为 jQuery 的版本号，可根据需要选择。*

### 基础语法

#### 语法格式

jQuery 的主要语法就是使用选择器选择 HTML 元素，并对其进行操作，基础语法格式如下：

> $(selector).action()

`$` 表示 jQuery 操作，selector 是 jQuery 用来选择 HTML 元素的，同时也可以选择对象（如 document），它结合了 CSS 和 XPath 选择器的语法， action() 则是要对元素进行的操作。

当 `$` 存在冲突时（网页上其他脚本或程序也使用到了 `$` 符号）， jQuery 允许使用别名来代替 `$` 符号，或者直接使用 `jQuery` 作为标识符。

```javascript
var jq = $.NoConflict();
jq(doucment).ready();
```

甚至可以把 `$` 符号作为参数传给 ready 中的函数，这样在内部就可以继续使用而不会产生冲突了。

#### 文档就绪函数

在文档全部加载之后，才会运行 ready 函数中的内容，这相当于注册 window 的 `onload` 事件，但是 ready 函数可以重复传入不同的函数并依次调用，而 `onload` 事件只能注册一个函数。代码如下：

```javascript
$(document).ready(function () {
  // action...
})

// 可以简写该方式为
$(function () {
  // action...
})
```

#### 选择器

类似于 CSS 选择器，jQuery 也使用选择器来选取 HTML 元素。需要对哪一个元素进行操作，就先选择该元素。 jQuery 强大的选择器可以方便精确地选择到你想要的任何一个元素，以便对 HTML 文档进行操作。它简化和增强了 JavaScript 选取元素的功能，不再需要使用繁琐的 `getElementByID()` 等方式来实现元素的选取。

jQuery 的选择器总是以 `$()` 表示，选择器的语法综合了 CSS 选择器和 XPath 选择器，包括元素选择器、 ID 选择器、类选择器、属性选择器等。具体内容请参考 [CSS 选择器](#) 和 [XPath 选择器](#) 的内容。

以下列出几个选择器示例：

| 语法                         | 描述                                       |
| -------------------------- | ---------------------------------------- |
| `$("*")`                   | 选取所有元素                                   |
| `$(this)`                  | 选取当前 HTML 元素                             |
| `$("p.intro")`             | 选取 class 为 intro 的 <p> 元素                |
| `$("p:first")`             | 选取第一个 <p> 元素                             |
| `$("[href]")`              | 选取带有 href 属性的元素                          |
| `$("a[target='_blank']")`  | 选取所有 target 属性等于 “_blank” 的 <a> 元素       |
| `$("a[target!='_blank']")` | 选取所有 target 属性不等于 “_blank” 的 <a> 元素      |
| `$("[href$='.jpg']")`      | 所有带有以 “.jpg” 结尾的属性值的 href 属性             |
| `$(":button")`             | 选取所有 type="button" 的 <input> 元素和 <button> 元素 |
| `$("tr:odd")`              | 选取奇数位置的 <tr> 元素                          |

#### 事件

jQuery 给出了一系列事件函数，这些事件对应于 HTML 中的一些事件[^2]，当事件被触发时，会调用事件函数。例如鼠标单击、双击、移动、按下键盘等都属于事件， jQuery 中的事件函数会对应这些事件的名称，使用时，只要将一个匿名函数作为参数传递到事件函数中，就会在事件被触发的时候执行了。下面列出一些常见的事件函数：

`ready()` ：前面提到过的页面加载事件；

`click()` ：点击事件，对应 HTML 中的 `onclick` 事件；

`dblclick()` ：鼠标双击事件；

`mouseenter()` `mouseleave()` ：鼠标移入/移出事件；

`mouseup()` `mousedown()` ：鼠标在元素上方/下方点击事件；

`hover()` ：鼠标悬停事件；

`focus()` `blur()` ：获得或失去焦点，对应 HTML 中的 `onfocus` 和 `onblur` 事件。

*\* 思考： jQuery 实际上是一种装饰者模式，选择器用来选取 HTML 元素，然后将该对象包裹到一个 jQuery 对象中，因此，将 HTML 对象放入到 $() 符号中后就成为了一个 jQuery 对象，并可以使用相应的 jQuery 对象的方法了。*

## jQuery 基本使用[^3]

### 动画效果

#### 显示和隐藏

`show()` `hide()` 两个函数可以显示或隐藏元素，可以使用 `"fast"` `"slow"` 或毫秒值作为参数指定变化的速度。 `toggle()` 用于在显示和隐藏直接进行切换操作，下面的例子使用按钮的单击事件来显示 `toggle()` 的效果。

```javascript
$("button").click(function () {
  $("p").toggle();
})
```

#### 淡入淡出

`fadeIn()` `fadeOut()` 可以实现淡入淡出的效果，参数与显示和隐藏函数相同， `fadeToggle()` 是两者直接的切换。

`fadeTo()` 是淡化到指定的透明度，该透明度作为第二个参数传入函数，范围在 0~1 之间。

#### 滑动

`slideUp()` `slideDown()` `slideToggle()` 类似于以上的方法，不详细介绍了。 

#### 动画

`animate()` 是一个强大的可以创建自定义动画的函数，它的使用语法如下：

> $(selector).animate({params}, speed, callback);

*\* 注意，只有采用相对定位和绝对定位的元素才可以在页面上移动。*

`{params}` 是需要改变的一组参数，其中几乎可以控制所有的 CSS 属性，属性名称要使用 Camel 写法，即 `margin-left` 要写成 `marginLeft` 的形式。

属性值可以使用绝对值（最终要变化到的值）或相对值（使用 `+=` 和 `-=` ），或者可以使用预定义值，如 `'toggle'` `'show'` `'hide'` 。 

在一个元素上多次使用 `animate()` 函数，可以实现队列功能，所有的动画将会按照顺序依次执行。

#### 停止动画

`stop()` 函数用来停止正在执行的动画，通过传递不同的参数实现不同的停止效果，语法格式如下：

> $(selector).stop(stopAll, goToEnd);

传入函数的两个参数是布尔值，如果第一个参数为 `true` ，会停止队列中所有动画，否则只是停止当前动画，队列后面的动画会接着执行。第二个参数为 `true` ，会立刻完成当前的动画并停止播放动画。默认时这两个值都是 `false` 。

#### Callback

每一个函数最后都可以传入一个匿名函数作为回调函数，也就是说当动画效果执行完毕之后，就会执行这个函数。通过回调函数可以方便地实现一些需要的功能和效果。

#### Chaining

jQuery 允许函数的链式调用，因为每个方法一般都会返回元素对象本身，所以可以方便地连续调用一串函数，不需要每次都重新去获得对象，简化书写。

### HTML

下面介绍的是 jQuery 对 HTML 中 DOM 的操作方法。

#### 内容和属性

`text()` ：设置或返回元素的文本内容；

`html()` ：设置或返回元素的内容（包括 HTML 标记）；

`val()` ：设置或返回表单字段的值；

`attr()` ：用于获取和设置属性值， 1.6 以后的版本建议使用 `prop()` 函数来代替。

#### 添加

`append()` ：在结尾插入内容；

`prepend()` ：在开头插入内容；

`after()` ：在被选元素后插入内容；

`before()` ：在被选元素前插入内容；

`appendTo()` ：将内容或被选元素插入到某元素。

注意，这里插入的内容可以是任何内容， HTML 内容或文本内容，甚至是 HTML 对象。另外， jQuery 创建元素的方法如下所示：

```javascript
var element = $("<p></p>").text("context");
```

#### 删除

`empty()` ：从被选元素中删除子元素；

`remove()` ：删除被选元素及其子元素，可以接收一个选择器，作为过滤条件。

#### CSS类

`addClass()` ：为被选元素添加 `class` 样式；

`removeClass()` ：为被选元素移除 `class` 样式；

`toggleClass()` ：会在添加和移除样式直接切换；

`css()` ：获取或设置被选元素的 CSS 属性。

jQuery 还提供了多个处理尺寸的方法，可以对元素和窗口的尺寸进行方便的操作。

`width()` `height()` ：设置或返回元素的宽度和高度（只有内容，不包括所有边距）。可以应用到文档、预览器窗口上，即 `document` 和 `window` 对象上，返回它们的宽度和高度值；；

`innerWidth()` `innerHeight()` ：返回元素的宽度和高度（包括内边距）；

`outerWidth()` `outerHeight()` ：返回元素的宽度和高度（包括内边距和边框）。如果传入 `true` 作为参数，则返回包括外边距的所有大小。

### 遍历

#### 概述

遍历就是对 HTML 文档的 DOM 树的查询操作，通过祖先后代或父子关系，从一个节点对象出发，逐个寻找到目标节点对象。关于 DOM 相关的内容，可以参考 [HTML DOM]()  。

#### 祖先

向上遍历：

`parent()` ：返回被选元素的直接父元素；

`parents()` ：返回被选元素的所有祖先元素，传入选择器作为参数进行过滤；

`parentsUntil()` ：传入一个参数，返回直到该参数的所有祖先元素， until 是不包含的。

#### 后代

向下遍历：

`children()` ：返回被选元素的所有直接子元素，传入选择器作为参数进行过滤；

`find()` ：返回被选元素的所有子孙元素，传入选择器作为参数进行过滤。

#### 同胞

水平遍历：

`siblings()` ：返回被选元素的所有同胞元素，传入选择器作为参数进行过滤；

`next()` ：返回被选元素的下一个同胞元素；

`nextAll()` ：返回被选元素后面的所有同胞；

`nextUntil()` ：需要传入一个选择器作为参数，返回被选元素后面到该元素的所有同胞， until 是不包含的；

`prev()` `prevAll()` `prevUntil()` ：于上面类似，但是方向不同，选择的是前面的同胞。

#### 过滤

`first()` ：返回要选择的元素的第一个；

`last()` ：返回要选择的元素的最后一个；

`eq()` ：传入一个索引值作为参数，表示要选择第几个元素，索引从 0 开始；

`filter()` ：通过选择器作为标准进行过滤；

`not` ：通过选择器作为标准进行排除。

### AJAX

#### 简介

AJAX 是 Asynchronous JavaScript and XML 的缩写，它可以异步发送 HTTP 请求到服务器，并将服务器响应（可以是文本、 HTML、 XML 或者 JSON[^4]）通过 JavaScript 显示到页面，实现局部交互和刷新的效果。在 AJAX 和后台交换数据的时候，并不会影响当前网页其他部分的浏览。

jQuery 将这个复杂的过程封装了起来，屏蔽了不同浏览器的差异，仅仅用一行简单的代码就可以完成 AJAX 的功能。

#### 加载

`load()` 函数是一个基本但很强大的 AJAX 方法，它将从服务器加载数据，并将返回的数据放入到被选元素中。它的语法格式如下：

> $(selector).load(URL, data, callback);

参数的含义显而易见， URL 是请求的地址， data 包含请求参数键值对， callback 是回调函数，但请求完成后调用的函数。回调函数可以有三个可选参数，响应的文本内容 `responseTxt` ，响应状态 `statusTXT` 和 `xhr` ，包含一个 XMLHttpRequest 对象，该对象就是完成 AJAX 功能的对象。

*\* 感觉这个方法并不常使用，至少我目前接触到的应用还没有涉及到。*

#### GET/POST

AJAX 还可以发送 GET 和 POST 请求，使用 jQuery 的 `$.get()` 和 `$.post()` 方法来完成。 GET 方式的语法格式如下：

> $.get(URL, callback);

回调函数有两个参数，第一个表示响应内容，第二个参数表示响应状态。

POST 方式的语法格式如下：

> $.post(URL, data, callback);

data 包含请求参数（以 JSON 的形式发送），回调函数含义与 GET 相同。

## 插件







[^1]: CDN 就是各大网站存放一些常用库和 API 的网址。
[^2]: 关于 HTML 事件，可以参考 [HTML事件](#) 中的介绍。
[^3]: 这里只是简单的列出了常用的 jQuery 函数，更多详细的内容请参考 api 文档。
[^4]: 一种数据传输格式，可以先不用了解。