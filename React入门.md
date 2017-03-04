### React 入门

#### 阮一峰 React 教程

- 如果要使用 React 的 JSX 语法， `<script>` 标签的 type 属性必须为 text/babel 。因为 JavaScript 不兼容 React 独有的 JSX 语法。
- 一个简单的 React 实例需要引入三个库， React 的核心库 `react.js` ， 提供 DOM 相关功能的库 `react-dom.js` 和将 JSX 语法转为 JavaScript 语法的 `browser.js` 库。

> babel src --out-dir build

该命令可以将 src 子目录的 js 文件进行语法转换，并放到 build 子目录中。

- 最简单的 React 语法

> ReactDOM.render(
>
> ​	<h1>Hello, World!</h1>,
>
> ​	document.getElementById('example')
>
> );

- 将 HTML 语言直接写在 JavaScript 语言直中，不加任何引号，就是 JSX 的语法。遇到 `<` 就按 HTML 标签解析，遇到 `{` 就按 JavaScript 语言解析。
- 如果将数组插入模板，那么数组会自动展开成一系列元素。
- 创建组件： `React.createClass(...)` 。
- render 组件属性
  - 组件可以作为 HTML 标签插入到 DOM 中，组件需要有自己的 **render** 方法，用来输出组件。返回的组件只能有一个根元素。组件上面可以加任意的属性，在组件中可以通过 this.props.attrName 来获得该属性的值。注意，原来的 HTML 属性 class 要写成 className ，而 for 属性要写成 htmlFor ，因为这两个属性是 JavaScript 的保留字。
  - 属性 this.props.children 代表组件的所有子节点对象。返回值有三种情况 undefined 、 object 、 array ，分别对应有 0 、 1 、 n 个子节点。可以用 React.Children 方法来处理子节点。
- propTypes 组件属性
  - 验证组件实例的属性是否符合要求。
  - getDefaultProps 组件属性可以设置属性的默认值。
- this.refs.[refName] 可以用来获取真实的 DOM 节点，该 DOM 节点上要添加 ref="refName" 属性。
- 互动属性 this.state
  - getInitialState 属性，初始化 state
  - this.state.[attrName] 获得状态值
  - this.setState({...}) 设置状态值
- 表单
  - 不能用 this.prop.value 直接去取值，需要通过事件获得， event.target.value 。
- 组件的生命周期
  - componentWillMount()
  - componentDidMount()
  - componentWillUpdate(object nextProps, object nextState)
  - componentDidUpdate(object prevProps, object prevState)
  - componentWillUnmount()
  - componentWillReceiveProps(object nextProps)
  - shouldComponentUpdate(object nextProps, object nextState)
- Ajax
  - ​