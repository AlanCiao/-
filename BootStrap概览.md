# Bootstrap概览

本文简要介绍 Bootstrap 相关概念和所使用的类选择器，基于 Bootstrap 3 的版本。

*目录*

[TOC]

## 概述

### 什么是Bootstrap

Bootstrap 来自 Twitter ，是前端 CSS 框架，可以方便地对网站页面进行布局，简单迅速地实现一些网页的样式和布局。同时，Bootstrap 可以实现响应式布局，即在任何尺寸的显示器上都可以得到很好的布局效果。说的简单一点，就是一些样式和布局的模板，通过使用一些已经定义好的类选择器来轻松实现良好的布局效果，而不必在自动设计大量的 CSS 属性。

### 使用

使用 Bootstrap 需要在 HTML 页面中引入相应的文件，格式如下：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <!-- 启用响应式布局 -->
    <!-- content 中的后两项是针对移动设备，禁止网页的缩放，视需求使用 -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
    <title>Home</title>
    
    <!-- 引入 Bootstrap 样式 -->
    <link type="text/css" rel="stylesheet" href="css/bootstrap.min.css" />
    <!-- 引入 Bootstrap 的 js 文件，它依赖 jQuery，所以同样必须引入 -->
    <script src="js/jquery-2.2.2.min.js" ></script>
    <script src="js/bootstrap.min.js" ></script>
  </head>
</html>
```

另外，针对 IE 浏览器可能出现的不兼容问题，还需要引入几个辅助脚本，如下所示：

```html
<!-- HTML5 shim and Respond.js IE8 support of HTML5 elements and media queries -->
<!--[if lt IE 9]>
<script src="../../docs-assets/js/html5shiv.js"></script>
<script src="../../docs-assets/js/respond.min.js"></script>
<![endif]-->
```

Bootstrap 的 CDN ，由MaxCDN 提供：

```html
<!-- Latest compiled and minified CSS -->
<link rel="stylesheet" href="//netdna.bootstrapcdn.com/bootstrap/3.0.0/css/bootstrap.min.css">

<!-- Optional theme -->
<link rel="stylesheet" href="//netdna.bootstrapcdn.com/bootstrap/3.0.0/css/bootstrap-theme.min.css">

<!-- Latest compiled and minified JavaScript -->
<script src="//netdna.bootstrapcdn.com/bootstrap/3.0.0/js/bootstrap.min.js"></script>
```

### 定制

这里还要特别提到定制的问题，因为 Bootstrap 是一个 CSS 开发框架，它提供了比较详尽的功能，在此基础上使用者可以根据需要来修改其中的样式来满足自己的需求，可以进行轻度定制和深度定制，方法类似。首先找到需要定制的样式类，然后将该类复制到自己的文件中，先覆盖默认样式，然后添加自己所要实现的样式。这样做的好处是可以引入 Bootstrap 的样式结构，而不需要自己去创建过多额外的类选择器。另外，通过修改 LESS 文件也可以深度定制样式，但在升级上会有困难，在这里不作说明。

在 Bootstrap 的网站上还可以选择需要的功能和样式进行生成，去除不需要的样式代码有利于节省带宽。定制的文件都会生成压缩和未压缩两个版本，未压缩版本是为了可以清楚地知道需要修改的地方在哪里。

### 响应式布局

所谓响应式布局就是根据所使用的设备来呈现不同的布局效果，新的 Bootstrap 设计目的是 **移动设备友好** 的，即优先考虑移动设备上的布局效果，将该方面融入核心代码中，其次才是台式设备。

#### 响应式使用工具

这些类可以控制在不同设备上元素是否显示，目前只支持块元元素，使用时要在需要应用的地方添加 `.visible-on` 类。

| Class                            | 描述           |
| -------------------------------- | ------------ |
| `.visible-xs` `.hidden-xs`       | 对额外的小设备可见/隐藏 |
| `.visible-sm` `.hidden-sm`       | 对小型设备可见/隐藏   |
| `.visible-md` `.hidden-md`       | 对中型设备可见/隐藏   |
| `.visible-lg` `.hidden-lg`       | 对大型设备可见/隐藏   |
| `.visible-print` `.hidden-print` | 可打印/不可打印     |

### 栅格系统（Grid System）

#### 基本使用

在标签上应用 `.container` 类选择器可以实现居中的效果，所有宽度都设置成百分比的样式，可以随着窗口大小的变化而缩放。 Bootstrap 将页面平均分为最多 12 列，大小也随着窗口的大小而改变，可以通过 `.col-md-*` 来进行栅格布局，如果屏幕宽度变小，栅格就会纵向堆叠在一起。如果针对其他类型的屏幕，可以使用 `.col-sm-*` 和 `.col-xs-*` 来划分较小的栅格。这些栅格会适应更小的屏幕，不会堆叠在一起。总列数加起来一定是 12 ，如果超过就会折行显示。

`col-md-offset-*` 会将栅格向右偏移 `*` 个列的距离。如果要使用栅格布局，那么可以用 `.row` 将这些内容包裹起来，它带有一个固定的内边距。

若要进行栅格的嵌套布局，需要使用 `.row` 将栅格包裹起来，并使用新的 `.col-md-*` 来划分新的栅格列，这些列的总和需要等于 12 。

`.col-md-push-*` 将栅格向右移动， `.col-md-pull-*` 将栅格向左移动，通过这两个类选择器可以将栅格的位置进行手动排列。

#### LESS 语义化栅格布局

用 LESS 语法中的变量和 Mixins 来生成栅格 class 。使用到的变量如下：

```less
@grid-columns:				12;
@grid-gutter-width:			30px;
@grid-float-breakpoint:		768px;
```

下面列出一段 Mixins 的代码，具体细节可以参考 [LESS]() 方面的内容。

```less
.make-row(@gutter: @grid-gutter-width) {
  // Then clear the floated columns
  .clearfix();

  @media (min-width: @screen-small) {
    margin-left:  (@gutter / -2);
    margin-right: (@gutter / -2);
  }

  // Negative margin nested rows out to align the content of columns
  .row {
    margin-left:  (@gutter / -2);
    margin-right: (@gutter / -2);
  }
}

// Generate the medium columns
.make-md-column(@columns; @gutter: @grid-gutter-width) {
  position: relative;
  // Prevent columns from collapsing when empty
  min-height: 1px;
  // Inner gutter via padding
  padding-left:  (@gutter / 2);
  padding-right: (@gutter / 2);

  // Calculate width based on number of columns available
  @media (min-width: @screen-medium) {
    float: left;
    width: percentage((@columns / @grid-columns));
  }
}

// Generate the medium column offsets
.make-md-column-offset(@columns) {
  @media (min-width: @screen-medium) {
    margin-left: percentage((@columns / @grid-columns));
  }
}
```

## CSS样式

### 概述

Bootstrap 通过 CSS 实现 HTML 的样式，通过使用提供的类选择器和标签选择器，完成所需的样式。

### 文字

#### 标签样式

Bootstrap 使用含有明确含义的标签来提供文本样式，这些标签都是 HTML 自身的标签，给这些标签加上 CSS 样式，便可以呈现出 Bootstrap 给出的指定样式。以下是一些常用的标签效果：

`<h1>` 到 `<h6>` ：加了样式的标题标签；

`<small>` ：使用在文字上，使文字相对于上下文略微变小（父字体的85%），并且颜色变浅；

`<p>` ：字体 14px ，行高设为 1.428 ，并且设置了等于行高 1/2 的底部外边距；

`<strong>` ：强调文字，对应于加粗样式；

`<em>` ：强调文字，对应于斜体样式；

`<abbr>` ：缩写词，需要带有 `title` 属性，否则没有效果。文字下面会出现浅色的虚下划线，并且鼠标悬停时为带有问号的样式；

`<address>` ：用来显示地址样式，该标签为块标签，里面的文本需要使用 `<br>` 标签换行；

`<blockquote>` ：引用样式，块元素，引用的文字会有缩进，并且段落左侧出现灰色竖线，内部可以使用 `<cite>` 和 `<small>` 标签；

`<code>` ：行内（内联）代码标签，会用特殊颜色显示代码，带有背景色；

`<pre>` ：块元素，同样是原样显示代码[^1]，代码块会放在带有边框和背景的矩形内。

#### 类选择器

Bootstrap提供了一系列的类选择器来实现文字效果

`.lead` ：应用在段落上，可以得到变大变粗，行高更高的文本；

`.initialism` ：应用在 `<abbr>` 标签上，可以使缩写词更小更紧凑；

`.pull-right` ：应用在 `<blockquote>` 标签中，用来使引用右对齐；

`.pre-scrollable` ：应用在 `<pre>` 标签上，设置最大高度为 350px 并显示滚动条；

`.text-left` `.text-center` `text-right` ：控制文字内容的对其方式；

以下类选择器用于将文字应用于特定的场合，提供不同的显示效果，字体相对会变小一些，主要是颜色上的差别。

`.text-muted` 浅灰色

`.text-primary` 蓝色

`.text-success` 绿色

`.text-info` 浅蓝色

`.text-warning` 黄色

`.text-danger` 红色

### 列表

列表的用法同HTML是一样的，如 `<ol>` 、 `<ul>` ，但是可以给标签添加类属性来应用 Bootstrap 的列表样式。常用的类选择器如下：

`.list-unstyled` ：一般使用在无序列表 `<ul>` 中，取消列表样式；

`.list-inline`  ：一般使用在无序列表中，使列表横向排列；

`.dl-horizontal`  ：使用在定义列表 `<dl>` 中，实现词条和解释在同一行上显示；

`.text-overflow` ：对于水平显示的定义列表，实现截断过长文字的效果。

### 表格

大部分的类选择器都需要加在 `<table>` 标签上面，用来实现表格的不同样式。要使用 Bootstrap 样式，首先需要加上 `.table` 类属性，实现基本表格样式，而更多的样式是分离出来单独使用的。其他的表格标签，如 `<caption>` 也会应用 Bootstrap 样式。常用样式类选择器如下：

`.table-striped` ：表格体隔行变色；

`.table-bordered` ：给表格加上边框；

`.table-hover` ：鼠标悬停行变色；

`.table-condensed` ：减小行高，使表格更加紧凑。

此外，在 `<tr>` 或 `<td>` 上加上 `.active` （灰色）、 `.success` （绿色）、`.warning` （黄色）、`.danger` （红色）类属性，可以实现特定颜色的显示。

如果要实现表格的响应式布局，可以用带有 `.table-responsive` 类属性的 `<div>` 将表格包裹起来，代码如下：

```html
<div class="table-responsive">
  <table class="table">
    ...
  </table>
</div> 
```

### 表单

#### 基础表单

表单的样式相对比较复杂，存在多种样式和多种 input 组件的组合形式，下面分类整理。

要使用 Bootstrap 表单样式，建议给 `<form>` 标签指定一个 `role` 属性，以增加可访问性，如下所示。

```html
<form role="form">
  ...
</form>
```

默认的表单是纵向排列的，可以在 `<form>` 使用类选择器 `.form-inline` 使表单横向排列。另外，使用 `.sr-only` 可以隐藏行内表单的标签，实际上，这个类是使元素对所有设备隐藏，除了屏幕阅读器。建议为所有表单使用 `<label>` 标签，这样在屏幕阅读器上才能被正确识别。

#### 水平表单

水平表单是将表单项中的 `<label>` 和 `<input>` 并列显示在一行的格式。当需要使用水平表单时，需要在 `<form>` 标签中加入类属性 `.form-horizontal` ，然后把需要水平显示的一个表单项用 `<div class="form-group">` 包裹起来（一般的表单最好也包裹起来以达到最好的效果），并把标签和表单项放在网格中，最后在 `<label>` 标签[^2]中添加 `.label-control` 类属性。这里面的类选择器 `.form-group` 就是用来标注一个表单项的，它必须存在，才能实现所需要的布局效果[^3]。

下面详细列出每个类选择器的作用：

`.form-group` ：将表单项作为一个整体进行操作，主要用于格式的对齐效果；

`.form-horizontal` ：这个类选择器主要是用于实现表单项水平排列和对齐的格式。这个类的作用是将 `.form-group` 类的行为变为 `.row` 类的行为，因此表单项内容还需要放在栅格布局中，以达到最好的效果；

`.control-label` ：用来对齐标签，调整格式；

`.form-control` ：用来改变输入文本框或者选择框等的样式，宽度被被设置为 100% ，更好的对齐到网格。

示例：

```html
<form class="col-md-12 form-horizontal">
  <div class="form-group">
    <label for="name" class="control-label col-md-2">username: </label>
    <div class="col-md-5">
      <input type="text" id="name" class="form-control"/>
    </div>
  </div>
  
  <div class="form-group">
    <label for="pwd" class="col-md-2 control-label">password: </label>
    <div class="col-md-5">
      <input type="password" id="pwd" class="form-control" />
    </div>
  </div>
</form>
```

#### 单选框和复选框

单选框和复选框布局也是类似的，但是稍微复杂一点，需要使用 `.checkbox` 和 `.radio` 来指定样式。另外，如果要使用行内样式，可以使用 `.checkbox-inline` 或 `.radio-inline` 设定格式。如果没有标签，可以不使用 `.form-group` 类来包裹单选框和复选框。示例如下：

```html
<form class="col-md-12">			
  <div class="form-group">
    <label for="check">Block checkboxes</label>
    <div class="checkbox">
      <label class="checkbox">
        <input type="checkbox" id="check"/> love
      </label>
    </div>
    <div class="checkbox">
      <label class="checkbox">
        <input type="checkbox" id="check"/> hate
      </label>
    </div>
  </div>
  
  <div class="form-group">
    <label for="check">Inline checkboxes</label>
    <div>
      <label class="checkbox-inline">
        <input type="checkbox" id="check"/> love
      </label>
      <label class="checkbox-inline">
        <input type="checkbox" id="check"/> hate
      </label>
    </div>
  </div>
</form>	
```

#### 其他样式

`.form-control-static` ：表单中的静态纯文本样式，应用在段落 `<p>` 上；

`.has-warning` `.has-error` `.has-success` ：带有特定含义的表单项，用不同颜色显示，与 `.form-group` 放在一起；

`.input-lg` `.input-sm` ：增大或减小表单项的高度；

`.help-block` ：表单提示或帮助信息样式，可以放到表单项后面的 `<span>` 中，如果一行显示不下会自动换行；

`#focusedInput` ：这是一个 ID 选择器，可以使输入文本框保持聚焦的状态。

### 按钮

#### 按钮颜色

所有带有类选择器 `.btn` 的元素都会显示为按钮样式，一般使用在按钮元素 `<button>` 上面，同时可以使用其他选择器指定更多的样式。具体样式见下面的示例：

```html
<!-- 标准的按钮 -->
<button type="button" class="btn btn-default">默认按钮</button>

<!-- 提供额外的视觉效果，标识一组按钮中的原始动作 -->
<button type="button" class="btn btn-primary">原始按钮</button>

<!-- 表示一个成功的或积极的动作 -->
<button type="button" class="btn btn-success">成功按钮</button>

<!-- 信息警告消息的上下文按钮 -->
<button type="button" class="btn btn-info">信息按钮</button>

<!-- 表示应谨慎采取的动作 -->
<button type="button" class="btn btn-warning">警告按钮</button>

<!-- 表示一个危险的或潜在的负面动作 -->
<button type="button" class="btn btn-danger">危险按钮</button>

<!-- 并不强调是一个按钮，看起来像一个链接，但同时保持按钮的行为 -->
<button type="button" class="btn btn-link">链接按钮</button>
```

#### 按钮大小

Bootstrap 提供了几个类选择器来控制按钮的大小：

| Class        | 描述              |
| ------------ | :-------------- |
| `.btn-lg`    | 大按钮             |
| `.btn-sm`    | 小按钮             |
| `.btn-xs`    | 超小按钮            |
| `.btn-block` | 块级按钮，占满父元素的全部宽度 |

#### 激活和禁用

`.active` 用来表示按钮或链接的激活状态，显示被按下的效果。当链接 `<a>` 作为按钮使用的时候，可以添加属性 `role="button"` 来指明它是一个按钮特性：

```html
<a href="#" class="btn btn-primary disable" role="button">链接</a>
```

禁用按钮可以使用标签的 `disable` 属性，而禁用链接需要使用 `.disable` 类选择器。注意，禁用链接只是体现在样式上，需要使用 JavaScript 来禁止实际的操作。鼠标悬停在禁用的按钮上会变为禁止标志。

### 图像

Bootstrap 提供了三个最常用的实现图片样式的类选择器：

`.img-rounded` ：使用圆角矩形外形；

`.img-circle` ：把图片变为圆形；

`.img-thumbnail` ：加上外边框和一些 padding 。

对于图片默认是非响应式的，如果需要实现响应式图片，则需要添加 `.img-responsive` 类选择器即可，它将图片的宽度设定为 100% ，高度自动，从而实现可缩放的自动全部填充。

### 帮助器类

这些类的样式用来完成某些特殊的辅助功能。

| Class                      | 描述                               |
| -------------------------- | -------------------------------- |
| `.close`                   | 用在 `<button>` 元素上，将其显示为一个通用的关闭符号 |
| `.caret`                   | 插入一个表示下拉菜单的向下箭头，可以用在 `<span>` 内  |
| `.pull-left` `.pull-right` | 向左/右快速浮动，不要用于导航条                 |
| `.center-block`            | 居中块元素                            |
| `.clearfix`                | 清除元素浮动，相当于 `clear: both;`        |
| `.hidden` `.show`          | 用来隐藏/显示元素，只能用于块元素                |
| `.text-hide`               | 可以将页面元素所包含的文本替换为背景图              |

## 布局组件

### 概述

这些组件是 Bootstrap 预设计好的样式，通过使用指定功能的类，就可以实现如导航栏、下拉菜单等功能组件，为开发和使用提供方便。

### 常用组件

#### Glyphicon 字体

使用 `.glyphicon` 类选择器，然后使用对应的字体类就可以显示出 Glyphicon 字体。

```html
<span class="glyphicon glyphicon-search"></span>
```

#### 下拉菜单

使用 `.dropdown` 类[^4]作用在包裹该下拉菜单的 `<div>` 上，确定一个可用的下拉菜单整体。菜单栏使用 `<button>` 标签来实现，需要使用 `.dropdown-toggle` 来确定样式。菜单项用一个 `<ul>` 列表来实现，需要使用 `.dropdown-menu` 类来控制菜单样式和隐藏显示效果。最重要的一步，在 `<button>` 标签上使用 `data-toggle="dropdown"` 属性将按钮与列表关联起来，实现显示菜单的功能。这个组件需要相应的 JavaScript 插件。

另外几个类选择器：

`.divider` ：用在 `<li>` 上，显示分割线；

`.dropdown-header` ：用在 `<li>` 上，显示菜单标题；

`.disabled` ：用在 `<li>` 上，禁用链接；

`.pull-right` ：用在 `<ul>` 上，使文字右对齐。

下面是一个下拉菜单的完整示例：

```html
<div class="dropdown">
  <button type="button" class="btn dropdown-toggle" id="dropdownMenu1" data-toggle="dropdown">
    menu
    <span class="caret"></span>
  </button>
  <ul class="dropdown-menu" role="menu" aria-labelledby="dropdownMenu1">
    <li role="presentation" class="dropdown-header">header</li>
    <li role="presentation" >
      <a role="menuitem" tabindex="-1" href="#">item1</a>
    </li>
    <li role="presentation">
      <a role="menuitem" tabindex="-1" href="#">item2</a>
    </li>
    <li role="presentation">
      <a role="menuitem" tabindex="-1" href="#">item3</a>
    </li>
    <li role="presentation" class="divider"></li>
    <li role="presentation" class="dropdown-header">header</li>
    <li role="presentation">
      <a role="menuitem" tabindex="-1" href="#">itemdivided</a>
    </li>
  </ul>
</div>
```

*\* role 属性不加也可以完成效果，但是为了元素的访问性建议添加。*

#### 按钮组

按钮组可以将多个按钮拼合成一个没有间隔的按钮组的样式，使用类选择器 `.btn-group` 可以实现这一功能。

此外，还有其他几个常用类选择器：

`.btn-toolbar` ：将多个按钮组组合到一起，方便管理；

`.btn-group-vertical` ：垂直显示按钮组；

`.btn-group-justified` ：使按钮组占满整个父元素宽度，但是只适用于 `<a>` 元素；

`.btn-group-lg` `.btn-group-sm` `.btn-group-xs` ：整组改变按钮大小。

#### 输入框组

输入框组可以在输入框前面或者后面嵌入一个 `<span>` 元素，当做前缀或后缀合成一个整体。在包裹这些元素的 `<div>` 上使用 `.input-group` 来确定要组合的元素，在 `<span>` 上使用 `.input-group-addon` 来调整样式，同样也不要忘了用 `.form-control` 来控制 `<input>` 的样式，使所有元素都可以很好的对齐。下面是一个示例：

```html
<div class="input-group">
  <span class="input-group-addon">$</span>
  <input type="text" class="form-control">
  <span class="input-group-addon">.00</span>
</div>
```

类似的，可以使用 `.input-group-lg` `.input-group-sm` `.input-group-xs` 来改变输入框组整体的大小。

如果加入的前缀或后缀元素是按钮，必须使用 `.input-group-btn` 来确保样式的统一，包裹该按钮，该选择器会直接改变按钮的样式来使用输入框。到现在为止，我们可以得到按钮、表单项等的任意组合，只要应用相应的类选择器控制样式，就可以得到许多不同的效果。

*\* 可以看出，实际上 `.btn-group` 和 `.input-group-btn` 的效果是等同的。*

*\* 注意，一般不要直接将输入框组的类应用到表单元素上，都用 `<span>` 来包裹，然后应用类选择器。*

最后给出一个比较复杂的例子，应用到了所有以上提及的元素样式：

```html
<div class="row">
  <div class="col-lg-6">
    <div class="input-group">
      <div class="input-group-btn">
        <button type="button" class="btn btn-default dropdown-toggle" data-toggle="dropdown">
          dropdown menu 
          <span class="caret"></span>
        </button>
        <ul class="dropdown-menu">
          <li><a href="#">item1</a></li>
          <li><a href="#">item2</a></li>
          <li><a href="#">item3</a></li>
          <li class="divider"></li>
          <li><a href="#">itemdivided</a></li>
        </ul>
      </div>
      <input type="text" class="form-control">
    </div>
  </div><br />
  
  <div class="col-lg-6">
    <div class="input-group">
      <input type="text" class="form-control">
      <div class="input-group-btn">
        <button type="button" class="btn btn-default dropdown-toggle" data-toggle="dropdown">
          dropdown menu 
          <span class="caret"></span>
        </button>
        <ul class="dropdown-menu pull-right">
          <li><a href="#">item1</a></li>
          <li><a href="#">item2</a></li>
          <li><a href="#">item3</a></li>
          <li class="divider"></li>
          <li><a href="#">itemdivided</a></li>
        </ul>
      </div>
    </div>
  </div>
</div>
```

#### 导航菜单

Bootstrap 的导航菜单可以通过在 `<ul>` 上使用 `.nav` 类选择器来实现，所有的导航菜单都必须具有该类。导航菜单有两种样式，标签式和胶囊式，对应的类选择器分别为 `.nav-tabs` 和 `.nav-pills` 。

下面是基础菜单示例：

```html
<p>标签式的导航菜单</p>
<ul class="nav nav-tabs">
  <li class="active"><a href="#">Home</a></li>
  <li><a href="#">SVN</a></li>
  <li><a href="#">iOS</a></li>
  <li><a href="#">VB.Net</a></li>
  <li><a href="#">Java</a></li>
  <li><a href="#">PHP</a></li>
</ul>

<p>基本的胶囊式导航菜单</p>
<ul class="nav nav-pills">
  <li class="active"><a href="#">Home</a></li>
  <li><a href="#">SVN</a></li>
  <li><a href="#">iOS</a></li>
  <li><a href="#">VB.Net</a></li>
  <li><a href="#">Java</a></li>
  <li><a href="#">PHP</a></li>
</ul>
```

在这两种基础菜单上再应用其他样式，可以实现更多的效果：

`.nav-stacked` ：实现菜单的垂直排列；

`.nav-justified` ：实现菜单两端对齐样式；

`.disable` ：使用在某个 `<li>` 上，可以禁用该菜单项。

如果要实现下拉菜单，需要在 `<li>` 上使用 `.dropdown` 类选择器，然后在下面嵌套下拉菜单项。

#### 导航栏

##### 默认导航栏

先写一个默认导航栏样式。

```html
<nav class="navbar navbar-default" role="navigation">
  <div class="navbar-header">
    <a class="navbar-brand" href="#">Logo</a>
  </div>
  <div>
    <ul class="nav navbar-nav">
      <li class="active"><a href="#">File</a></li>
      <li><a href="#">Edit</a></li>
      <li class="dropdown">
        <a href="#" class="dropdown-toggle" data-toggle="dropdown">
          Tools 
          <b class="caret"></b>
        </a>
        <ul class="dropdown-menu">
          <li><a href="#">tool1</a></li>
          <li><a href="#">tool2</a></li>
          <li><a href="#">tool3</a></li>
          <li class="divider"></li>
          <li><a href="#">tool4</a></li>
          <li class="divider"></li>
          <li><a href="#">tool5</a></li>
        </ul>
      </li>
    </ul>
  </div>
</nav>
```

首先，导航栏样式要应用到 `<nav>` 标签上，使用 `.navbar` 和 `.navbar-default` 实现默认的基本样式。这里说明了一下 `role="navigation"` 是为了增强可访问性的。然后用一个 `<div>` 实现导航栏头，使用 `.navbar-header` 类来标注样式，使用 `.navbar-brand` 类来增大字体，突出显示。使用 `.nav` 和 `.navbar-nav` 来标注导航栏菜单的样式。用 `.active` 表示选中的样式，菜单内可以嵌套下拉菜单。需要注意的细节都体现在示例当中了。

##### 导航栏中的其他元素

`.navbar-form` ：用来实现导航栏中的表单样式，里面的表单使用 `.form-group` 来包裹规定样式；

`.navbar-btn` ：用来实现导航栏中表单外的按钮样式；

`.navbar-text` ：用来实现导航栏中的文字样式，一般使用在 `<p>` 标签中；

`.navbar-link` ：用来实现非导航链接样式，在 `<a>` 标签中使用；

##### 元素的对齐方式

`.navbar-left` `.navbar-right` ：导航栏中的元素向左或向右浮动；

`.navbar-fixed-top` `.navbar-fixed-bottom` `.navbar-static-top` ：分别实现导航栏固定顶端、固定底端、在顶端随页面滚动，在 `<nav>` 中使用；

`.navbar-inverse` ：导航栏反色样式。

##### 响应式导航栏

下面是一个示例：

```html
<nav class="navbar navbar-default" role="navigation">
  <div class="navbar-header">
    <button type="button" class="navbar-toggle" data-toggle="collapse" data-target="#example-navbar-collapse">
      <span class="sr-only">切换导航</span>
      <span class="icon-bar"></span>
      <span class="icon-bar"></span>
      <span class="icon-bar"></span>
    </button>
    <a class="navbar-brand" href="#">Logo</a>
  </div>
  <div class="collapse navbar-collapse" id="example-navbar-collapse">
    <ul class="nav navbar-nav">
      <li class="active"><a href="#">File</a></li>
      <li><a href="#">Edit</a></li>
      <li class="dropdown">
        <a href="#" class="dropdown-toggle" data-toggle="dropdown">
          Tools <b class="caret"></b>
        </a>
        <ul class="dropdown-menu">
          <li><a href="#">item1</a></li>
          <li><a href="#">item2</a></li>
          <li><a href="#">item3</a></li>
          <li class="divider"></li>
          <li><a href="#">itemdivided</a></li>
          <li class="divider"></li>
          <li><a href="#">itemdivided</a></li>
        </ul>
      </li>
    </ul>
  </div>
</nav>
```

要启动响应式导航栏，应当将需要折叠的菜单栏内容放到一个 `<div>` 中，然后使用 `.collapse` 和 `.navbar-collapse` 类来标记，并且需要给该内容一个 ID 标记，示例中是 `example-navbar-collapse` 。折叠后的菜单是一个按钮，放在导航栏头部中，用 `.navbar-toggle` 类标记，并且加入 `data-toggle="collapse"` 和 `data-target="#example-navbar-collapse"` 属性， `data-target` 链接到点击需要显示的内容。这个按钮只有在折叠后才会显示出来。

`.icon-bar` 是在显示在按钮上的一条粗线。具体细节见示例代码。

#### 面包屑导航

面包屑导航是一种类似于如下样式的有序列表菜单：

> Home / 2013 / 十一月

Bootstrap 简单地在 `<ol>` 上面使用 `.breadcrumb` 类选择器来实现这一的效果。下面是示例代码：

```html
<ol class="breadcrumb">
  <li><a href="#">Home</a></li>
  <li><a href="#">2013</a></li>
  <li class="active">十一月</li>
</ol>
```

#### 分页

使用在分页导航上面的样式，使用上也比较简单，在 `<ul>` 上使用 `.pagination` 来指定样式，如下面示例所示：

```html
<ul class="pagination">
  <li><a href="#">&laquo;</a></li>
  <li><a href="#">1</a></li>
  <li><a href="#">2</a></li>
  <li><a href="#">3</a></li>
  <li><a href="#">4</a></li>
  <li><a href="#">5</a></li>
  <li><a href="#">&raquo;</a></li>
</ul>
```

此外，可以使用 `.disabled` 和 `.active` 更改链接状态，使用 `.pagination-lg` 和 `.pagination-sm` 来设定大小。

另外一种形式是”翻页“，同样是一个无序列表，但是只有两个链接，上一页和下一页。使用 `.pager` 类实现样式， `.previous` 和 `.next` 设定浮动对齐的方向， `.disabled` 设定禁止状态。

#### 标签与徽章

将文本以标签的样式显示，比较简单，所使用到的类选择器如示例所示：

```html
<span class="label label-default">默认标签</span>
<span class="label label-primary">主要标签</span>
<span class="label label-success">成功标签</span>
<span class="label label-info">信息标签</span>
<span class="label label-warning">警告标签</span>
<span class="label label-danger">危险标签</span>
```

徽章类似于标签，只是边角更加圆润，下面是示例：

```html
<a href="#">Mailbox <span class="badge">50</span></a>
```

#### 超大屏幕

`.jumbotron` 用来适应超大屏幕样式，标题会变得更大，并为登录界面内容添加更多的外边距。只要用一个 `<div>` 将内容包裹起来就可以了。如果想要内容占用全部屏幕宽度且不带圆角，则将带有 `.jumbotron` 类的 `<div>` 放到 `.container` 外面。

#### 页面标题

`.page-header` 用于设置网页标题样式，会在标题的下面加上一条分隔线。

```html
<div class="page-header">
  <h1>Example page header <small>Subtext for header</small></h1>
</div>
```

#### 缩略图

网站中的照片、视频和文本等，可以简单地在图像外面包裹上一个带有 `.thumbnail` 的 `<a>` 标签，图片会显示出灰色的边框和 4px 的内边距，当鼠标悬浮时边框会高亮显示。

将 `<a>` 标签改为 `<div>` 标签，就可以在缩略图中添加任意想要添加的内容了，包括图片、文字、和按钮。

#### 警告

警告给出了网页操作的反馈消息，通过不同颜色提示操作成功或者失败等警示。通过添加 `.alert` 类来指明是警告样式，并使用四个提示相应颜色的类，代码如下：

```html
<div class="alert alert-success">成功！很好地完成了提交。</div>
<div class="alert alert-info">信息！请注意这个信息。</div>
<div class="alert alert-warning">警告！请不要提交。</div>
<div class="alert alert-danger">错误！请进行一些更改。</div>
```

将警告信息添加 `.alert-dismissable` ，就会产生一个可关闭的警告框，同时要在警告框内添加一个关闭按钮，并且按钮标签上要加上 `.data-dismiss="alert"` 属性。

```html
<div class="alert alert-warning alert-dismissable">
  <button type="button" class="close" data-dismiss="alert" aria-hidden="true">&times;</button>
  <strong>Warning!</strong> Best check yo self, you're not looking too good.
</div>
```

如果警告框中有链接，则可以使用 `.alert-link` 为链接提供与警告框匹配的颜色。

#### 进度条

用来显示进度的一个组件，可以轻松地做出进度条的动画示例，但是要使用到 CSS3 中的过渡和动画，可能存在浏览器兼容问题。

在 `<div>` 中添加 `.progress` 类选择器，在里面再添加一个 `<div>` 并使用 `.progress-bar` 类选择器和一个表示进度条宽度的 `style` 属性。详细代码示例如下：

```html
<div class="progress">
  <div class="progress-bar" role="progressbar" aria-valuenow="60" 
       aria-valuemin="0" aria-valuemax="100" style="width: 40%;">
    <span class="sr-only">40% 完成</span>
  </div>
</div>
```

同样，类 `.progress-bar-success` `.progress-bar-info` `.progress-bar-warning` `.progress-bar-danger` 可以用来控制进度条的颜色。

在最外层 `<div>` 上再加上 `.progress-striped` 类选择器可以产生条纹效果（ IE8 中不可用）。如果再添加 `.active` 类选择器，就会实现动画的进度条。最终示例代码如下：

```html
<div class="progress progress-striped active">
  <div class="progress-bar progress-bar-success" role="progressbar" 
       aria-valuenow="60" aria-valuemin="0" aria-valuemax="100" 
       style="width: 40%;">
    <span class="sr-only">40% 完成</span>
  </div>
</div>
```

`.progress` 中放置多个进度条将会实现堆叠效果。

#### 媒体对象

这个组件就是指图文混排的实现。用 `.media` 类包裹所要放置的图文混排媒体对象，然后放入图片和文本内容，具体应用到的类如下面代码所示：

```html
<div class="media">
  <a class="pull-left" href="#">
    <img class="media-object" src="..." alt="...">
  </a>
  <div class="media-body">
    <h4 class="media-heading">Media heading</h4>
    ...
  </div>
</div>
```

此外，可以在列表中使用媒体对象，例如博客和评论。只要在 `<ul>` 列表上添加 `.media-list` 类选择器，然后将媒体对象放入一个列表项中就可以了。使用方法如下面代码所示：

```html
<ul class="media-list">
  <li class="media">
    <a class="pull-left" href="#">
      <img class="media-object" src="..." alt="...">
    </a>
    <div class="media-body">
      <h4 class="media-heading">Media heading</h4>
      ...
    </div>
  </li>
</ul>
```

#### 列表组

列表组用于程序复杂和自定义的列表形式组件。在 `<ul>` 中添加 `.list-group` ，在 `<li>` 中添加 `.list-group-item` ，最终会形成一个横向的列表形式，每个列表条目由分隔线分开。列表组中可以通过 `.badge` 添加徽章。如果要使用链接列表的话，用 `<div>` 代替 `<ul>` ，而用 `<a>` 代替 `<li>` 便可以达到效果。

可以向列表项内添加任意内容，用 `.list-group-heading` 标注标题，用 `.list-group-text` 标注内容。

#### 面板

面板组件就是将一些内容放到一起，组成一个盒子。只要在 `<div>` 标签上使用 `.panel` 和 `.panel-default` 就可以实现一个面板，而面板的内容用一个 `<div>` 包裹起来，并使用 `.panel-body` 来规定样式。

在面板内容的前面可以加入标题，需要使用 `.panel-heading` ，如果标题使用 `<h1>` 到 `<h6>` 标签的话，需要在上面使用 `.panel-title` 类来控制格式。而在内容的后面可以加入注脚，使用 `.panel-footer` 类来控制样式。

面板标题可以控制颜色样式，使用 `.panel-primary` `.panel-success` `.panel-info` `.panel-warning` 和 `panel-danger` 。注意，面板注脚是不会继承该颜色的。

在面板中可以添加 `<table>` 和 `<ul>` 组，直接把表格和列表组放入面板中即可。

#### Wells

`.well` 是给 `<div>` 显示一个向下凹陷的背景，可以通过 `.well-lg` 和 `.well-sm` 调整大小，同时也会改变内边距。

## 插件

### 概述

这里是 Bootstrap 自带的 12 个 JavaScript 插件，为更好地实现某些动态效果而设计。可以方便地使用 Bootstrap API 几乎不用自己编写代码就可以触发相应的事件。在 Bootstrap 中给标签创建了 `data-*` 属性，通过这个标签属性就可以启动 JavaScript 脚本，完全不需要写任何的代码，操作会由 Bootstrap 完成。

例如在下拉菜单组件中，菜单按钮元素上面有两个特殊的属性，写法如下：

```html
<button type="button" class="btn btn-default dropdown-toggle" data-toggle="dropdown" data-target="#list">
		File
</button>
```

其中的 `data-toggle` 和 `data-target` 就是 `data-api` 属性。

下面给出关闭 `data-api` （data属性）的方法：

```javascript
$(document).off('.data-api');

// 针对某一个插件
$(document).off('.alert.data-api');
```

Bootstrap 还可以使用纯 JavaScript API 来操作元素和组件，调用 API 的方式类似于 jQuery 。



冲突 事件？



### 常用插件

#### 过渡效果

Transition.js 是针对 `transitionEnd` 事件的一个基本助手工具，也是对 CSS 过渡效果的模拟。它被其它插件用来检测当前浏览器对 CSS 过渡效果是否支持。

#### 模态框

类似于一个弹出窗口，有着更美观的样式。模块的样式如下面的示例代码所示：

```html
<!-- Modal -->
<div class="modal fade" id="myModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <button type="button" class="close" data-dismiss="modal" aria-hidden="true">&times;</button>
        <h4 class="modal-title" id="myModalLabel">Modal title</h4>
      </div>
      <div class="modal-body">
        ...
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-default" data-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary">Save changes</button>
      </div>
    </div><!-- /.modal-content -->
  </div><!-- /.modal-dialog -->
</div><!-- /.modal -->
```

##### 通过 data 属性使用

```html
<button type="button" data-toggle="modal" data-target="#myModal">Launch modal</button>
```

使用 `data-toggle` 指定触发的效果是模态框，而用 `data-target` 指向需要触发的模块。

##### 通过 JavaScript 调用

只需要一行代码：

```javascript
js$('#myModal').modal(options);
```

通过 ID `#myModal` 来选中要触发的模块，然后调用模态框， `options` 为可选参数。

选项： `data-backdrop="true"` `data-keyboard="true"` `data-show="true"` `data-remote="path"` 

方法： `.modal(options)` `.modal('toggle')` `.modal('show')` `.modal('hide')` 

Bootstrap 也提供了几个事件可供使用，如 `show.bs.modal` `shown.bs.modal` `hide.bs.modal` `hidden.bs.modal` 。

选项和事件具体内容应用时可以参考文档或示例，这里不做过多的介绍。

#### 下拉菜单

##### data 属性方式

前面的组件中已经介绍了下拉菜单，可以通过 `data` 属性方法将一个无序列表和链接或按钮变成下拉菜单。向链接或按钮中添加 `data-toggle="dropdown"` 属性和 `data-target="#ID"` 属性来指向和操作菜单列表。可以在菜单列表上添加 `aria-labelledby="#btnID"` 属性来指向菜单。

##### JavaScript 方式

打开或关闭下拉菜单：

```javascript
$('.dropdown-toggle').dropdown();
```

提供的事件为 `show.bs.dropdown` `shown.bs.dropdown` `hide.bs.dropdown` `hidden.bs.dropdown` 。

方法： `$().dropdown('toggle')` 打开或关闭下拉菜单。

#### 滚动监听

##### data 属性方式

`data-spy="scroll"` 添加到监听体上，一般就是包裹要监听内容的部分，然后加入监听的导航栏目标 `data-target="#navbarID"` 就可以完成内容滚动监听。同时，在导航栏中要加入对应内容的锚。

##### JavaScript 方式

```javascript
$('body').scrollspy({ target: '#navbarID' });
```

事件： `activate.bs.scrollspy` 

方法： `.scrollspy('refresh')` 

选项： `data-offset="10"` 

#### 标签页

##### data 属性方式

在导航菜单上列表的每一项上添加 `data-toggle="tab"` 或 `data-toggle="pill"` 就完成了。

导航菜单项的链接要指向标签内容项的 ID ，标签内容项可以添加 `.tab-pane` 规定样式，如果再添加 `.fade` 可以具有淡入特效，而第一个标签内容项还要添加 `.in` 使初始内容同时具有淡入效果。

##### JavaScript 方式

需要单独激活每一个标签项。

```javascript
$('#tabID a').click(function (e) {
  e.preventDefault();
  $(this).tab('show');
});

// 其他的方式
$('#tabID a[href="#profile"]').tab('show') // Select tab by name
$('#tabID a:first').tab('show') // Select first tab
$('#tabID a:last').tab('show') // Select last tab
$('#tabID li:eq(2) a').tab('show') // Select third tab (0-indexed)
```

方法： `$().tab` 

事件： `show.bs.tab` `shown.bs.tab` 

`event.target` 和 `event.relatedTarget` 是两个事件属性，一个指向激活的标签，另一个指向前一个激活的标签。

#### 工具提示

##### data 属性方式

添加 `data-toggle="tooltip"` 属性和 `title` 属性。

##### JavaScript 方式

```javascript
$('#example').tooltip(options);
```

选项： `data-animation="true"` `data-html="false"` `data-placement="top | bottom | left | right | auto"` `data-selector="string"` `data-title="string"` `data-trigger="hover | focus | click | manual"` `data-delay="0"` `data-container="string | false"` 

方法： `$().tooltip(options)` `.tooltip('show' | 'hide' | 'toggle' | 'destroy')` 

事件： `show.bs.tooltip` `shown.bs.tooltip` `hide.bs.tooltip` `hidden.bs.tooltip` 

#### 弹出框

弹出框插件是基于工具提示插件的。

##### data 属性方式

`data-toggle="popover"` 用来添加弹出框。

##### JavaScript 方式

```javascript
js$('#ID').popover(options);
```

选项： `data-content="string"` 其他的与工具提示选项相同。

方法： `$().popover(options)` `.popover('show' | ...)` 

事件： `show.bs.popover` `shown.bs.popover` `hide.bs.popover` `hidden.bs.popover` 

#### 警告框

##### data属性方式

为警告框添加 `data-dismiss="alert"` 属性，便可以启动警告框关闭功能。

##### JavaScript方式

```javascript
js$(".alert").alert();
```

事件： `close.bs.alert` `closed.bs.alert` 

方法： `$().alert([.fade | .in])` `.alert('close')` 

#### 按钮

通过添加不同的 data 属性可以显示按钮不同的样式：

`data-loading-text="loading ..."` 显示正在加载样式；

`data-toggle="button"` 实现切换状态的按钮；

`data-toggle="buttons"` 向按钮组添加，将 checkbox 变成可多选的按钮组，将 radio 变成可单选的按钮组。

JavaScript 方式则简单地用 `$('.btn-group').button()` 就可以实现了。

方法： `$().button('loading' | 'toggle' | 'reset' | string)` 

#### 折叠

##### data 属性方式

向触发折叠的元素添加 `data-toggle="collapse"` 和 `data-target="#ID"` 属性就可以了。折叠的目标需要添加 `.collapse` 指定样式， `.in` 表示默认展开。

用 `data-parent="#ID"` 给一组折叠元素设置控制器。

##### JavaScript 方式

手动触发：

```javascript
$(".collapse").collapse();
```

方法： `.collapse(options)` `.collapse('toggle' | 'show' | 'hide')` 

事件： `show.bs.collapse` `shown.bs.collapse` `hide.bs.collapse` `hidden.bs.collapse` 

##### 示例

```html
<div class="panel-group" id="accordion">
  <div class="panel panel-default">
    <div class="panel-heading">
      <h4 class="panel-title">
        <a data-toggle="collapse" data-toggle="collapse" data-parent="#accordion" href="#collapseOne">
          Collapsible Group Item #1
        </a>
      </h4>
    </div>
    <div id="collapseOne" class="panel-collapse collapse in">
      <div class="panel-body">
        paragraph 1
      </div>
    </div>
  </div>
  <div class="panel panel-default">
    <div class="panel-heading">
      <h4 class="panel-title">
        <a data-toggle="collapse" data-toggle="collapse" data-parent="#accordion" href="#collapseTwo">
          Collapsible Group Item #2
        </a>
      </h4>
    </div>
    <div id="collapseTwo" class="panel-collapse collapse">
      <div class="panel-body">
        paragraph 2
      </div>
    </div>
  </div>
  <div class="panel panel-default">
    <div class="panel-heading">
      <h4 class="panel-title">
        <a data-toggle="collapse" data-toggle="collapse" data-parent="#accordion" href="#collapseThree">
          Collapsible Group Item #3
        </a>
      </h4>
    </div>
    <div id="collapseThree" class="panel-collapse collapse">
      <div class="panel-body">
        paragraph 3
      </div>
    </div>
  </div>
</div>
```



#### 轮播

##### data 属性方式

先给出示例：

```html
<div id="carousel-example-generic" class="carousel slide">
  <!-- Indicators -->
  <ol class="carousel-indicators">
    <li data-target="#carousel-example-generic" data-slide-to="0" class="active"></li>
    <li data-target="#carousel-example-generic" data-slide-to="1"></li>
    <li data-target="#carousel-example-generic" data-slide-to="2"></li>
  </ol>

  <!-- Wrapper for slides -->
  <div class="carousel-inner">
    <div class="item active">
      <img src="..." alt="...">
      <div class="carousel-caption">
        ...
      </div>
    </div>
    ...
  </div>

  <!-- Controls -->
  <a class="left carousel-control" href="#carousel-example-generic" data-slide="prev">
    <span class="glyphicon glyphicon-chevron-left"></span>
  </a>
  <a class="right carousel-control" href="#carousel-example-generic" data-slide="next">
    <span class="glyphicon glyphicon-chevron-right"></span>
  </a>
</div>
```

第一部分是图片下方显示的小圆点，第二部分是轮播的图片，用 `.item` 包裹起来，里面可以放图片和图片的标题，第三部分是两侧的切换控制链接，用来切换图片。 `.carousel-caption` 里面可以放入任何东西。

##### JavaScript 方式

手动启动轮播组件：

```javascript
$('.carousel').carousel();
```

选项： `data-interval="5000"` `data-pause="hover"` `data-wrap="true"` 

方法： `.carousel(options)` `.carousel('cycle' | 'pause' | number | 'prev' | 'next')` 

事件： `slide.bs.carousel` `slid.bs.carousel` 

#### 附加导航 Affix.js

##### data 属性方式

使用属性 `data-spy="affix"` 和 data 选项来实现。

##### JavaScript 方式

```javascript
$('#affixID').affix({
  offset: {
    top: 100,
    bottom: function () {
      return (this.bottom = $('.bs-footer').outerHeight(true));
    }
  }
});
```

选项： `data-offset-top="10"` `data-offset-bottom="5"` 



[^1]: `<code>` 和 `<pre>` 中的文本如果出现特殊符号，应替换为预定义符号。例如：大于号 `>` 需要写成 `&gt;` 的形式。
[^2]: `<label>` 标签和 `<input>` 会自动占满整行，所以需要手动设定其宽度，或者放到Bootstrap网格布局中。
[^3]: 一个 `.form-group` 就代表表单中的一行，也可以把多个表单项放到一个组中，它们会被看成一个表单项组。该类用来获得最佳的对其样式，单选框、复选框和按钮可以不使用该类。
[^4]: 这个类选择器也可以换成 `.btn-group` 类选择器，可以实现相同的效果，最终就是把按钮和列表包裹在一起，完成下拉菜单的样式。另外，上拉菜单可以添加 `.dropup` 类。实际上， `dropdown` 是实现了 `position: relative;` 的属性。





