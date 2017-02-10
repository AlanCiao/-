# Vue.js 2 入门

#### 使用：

用 `<script>` 引入文件或 CDN。

用 NPM 安装： `npm install vue` 

有两种构建方式，独立构建和运行构建。它们的区别在于前者包含**模板编译器**而后者不包含。

- 独立构建包含模板编译器并支持 `template` 选项。 **它也依赖于浏览器的接口的存在，所以你不能使用它来为服务器端渲染。**
- 运行时构建不包含模板编译器，因此不支持 `template` 选项，只能用 `render` 选项，但即使使用运行时构建，在单文件组件中也依然可以写模板，因为单文件组件的模板会在构建时预编译为 `render` 函数。运行时构建比独立构建要轻量30%，只有 16.39 Kb min+gzip大小。

默认 NPM 包导出的是 **运行时** 构建。为了使用独立构建，在 webpack 配置中添加下面的别名：

```
resolve: {
  alias: {
    'vue$': 'vue/dist/vue.js'
  }
}
```

运行时构建的是完全兼容 CSP （Google Chrome Apps）的.



命令行构建（不建议新手使用）

```
npm install --global vue-cli
vue init webpack my-project
cd my-project
npm install
npm run dev
```



Bower 安装

```
bower install vue
```



从 Github 克隆

```
git clone https://github.com/vuejs/vue.git node_modules/vue
cd node_modules/vue
npm install
npm run build
```



从网络地址引用

<script src="https://unpkg.com/vue/dist/vue.js">



#### 实例：

每一个应用都是通过构造函数 Vue 创建一个 Vue 的根实例启动的。

```javascript
var vm = new Vue({
  // options
});
```

组件构造器：

```javascript
var MyComponent = Vue.extend({
  // 扩展选项
});

// 所有的 ‘MyComponent’ 实例都将以预定义的扩展选项被创建
var myComponentInstance = new MyComponent();
```

所有的 Vue.js 组件其实都是被扩展的 Vue 实例。



属性与方法：

将对象放到 data 中，这样 vue 实例对象就可以表示这个对象了，因此也可是引用该对象的属性。但是后添加的新属性则不可以。这种方式叫做代理。

```javascript
var data = { a: 1 };
var vm = new Vue({
  data: data
});
vm.a === data.a; // -> true
// 设置属性也会影响到原始数据
vm.a = 2;
data.a; // -> 2
// ... 反之亦然
data.a = 3;
vm.a; // -> 3
```

调用自身的属性和方法：

```javascript
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})
vm.$data === data // -> true
vm.$el === document.getElementById('example') // -> true
// $watch 是一个实例方法
vm.$watch('a', function (newVal, oldVal) { //这里不能使用箭头函数。
  // 这个回调将在 vm.a 改变后调用
})
```

Vue 实例的生命周期：

```javascript
var vm = new Vue({
  data: { a: 1 },
  created: function () {
    // `this` 指向 vm 实例
    console.log('a is: ' + this.a)
  }
});
// -> "a is: 1"
```



![Vue 的生命周期图示](lifecycle.png)



#### 模板语法：

- 文本 Mustach {{ data }}

v-once 只进行一次绑定

- 纯 HTML

v-html="rawHtml" 忽略数据绑定

- 属性

v-bind: attr=""

如果属性是布尔值，当为 false 时，属性会被移除。

- 以上所有绑定中都支持 JavaScript 表达式，注意，只对表达式生效。只能访问全局变量的一个白名单，如 Math 和 Date ，不能访问用户定义的全局变量。
- 过滤器

用于文本格式化，以管道方式使用。只能用于 mustache 插值尾部。

```javascript
{{ message | capitalize }}

new Vue({
  // ...
  filters: {
    capitalize: function (value) { // 前面的值永远都是第一个参数
      if (!value) return ''
      value = value.toString()
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  }
})
```

过滤器可以串联，可以带参数。



#### 指令 Directives ：

带有 v- 前缀的特殊属性，预期的属性值是单一的 JavaScript 表达式 （v-for 除外）。

- 参数

v-bind:attr="val"  用于绑定属性

v-on:action="func"  用于绑定事件

- 修饰符 Modifiers

在绑定的属性后面加上 .modifier 表示附加一些特殊的处理。



缩写：

v-bind:  简化为  :

v-on:  简化为  @



#### 计算属性

用来实现复杂的逻辑。就是一个函数，然后声明为 computed 中的一个属性，这样通过这个属性就返回一个函数的值。

```javascript
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
  
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // a computed getter
    reversedMessage: function () {
      // `this` points to the vm instance
      return this.message.split('').reverse().join('')
    }
  }
})
```

这个属性可以用 Vue 实例直接调用， vm.attr 。计算属性会依赖其他的属性，当依赖属性不变时，计算属性是不会改变的，保持一个常量，并且计算结果一直作为缓存存储起来，再次使用时不需要再调用函数进行计算。

计算属性的 getter 和 setter

```javascript
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
// ...
```



#### 观察 Watchers

观察一个属性，当该属性变化时，执行回调函数。

```html
<div id="watch-example">
  <p>
    Ask a yes/no question:
    <input v-model="question">
  </p>
  <p>{{ answer }}</p>
</div>

<script>
var watchExampleVM = new Vue({
  el: '#watch-example',
  data: {
    question: '',
    answer: 'I cannot give you an answer until you ask a question!'
  },
  watch: {
    // 如果 question 发生改变，这个函数就会运行
    question: function (newQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.getAnswer()
    }
  },
  methods: {
    // _.debounce 是一个通过 lodash 限制操作频率的函数。
    // 在这个例子中，我们希望限制访问yesno.wtf/api的频率
    // ajax请求直到用户输入完毕才会发出
    // 学习更多关于 _.debounce function (and its cousin
    // _.throttle), 参考: https://lodash.com/docs#debounce
    getAnswer: _.debounce(
      function () {
        var vm = this
        if (this.question.indexOf('?') === -1) {
          vm.answer = 'Questions usually contain a question mark. ;-)'
          return
        }
        vm.answer = 'Thinking...'
        axios.get('https://yesno.wtf/api')
          .then(function (response) {
            vm.answer = _.capitalize(response.data.answer)
          })
          .catch(function (error) {
            vm.answer = 'Error! Could not reach the API. ' + error
          })
      },
      // 这是我们为用户停止输入等待的毫秒数
      500
    )
  }
})
</script>
```



#### class 与 style 属性的绑定

使用 v-bind ，但是属性增强了，除了字符串，还可以使用对象和数组，这些也都可以是计算属性。

1. class 绑定

   ```html
   <div v-bind:class="{ active: isActive, 'text-danger': hasError }"></div>
   // 类选择器: 布尔值

   data: {
     isActive: true,
     hasError: false
   }

   <div v-bind:class="classObject"></div>

   data: {
     classObject: {
       active: true,
       'text-danger': false
     }
   }
   ```

   数组

   ```html
   <div v-bind:class="[activeClass, errorClass]">
   data: {
     activeClass: 'active',
     errorClass: 'text-danger'
   }
     
   // 渲染为
   <div class="active text-danger"></div>
   ```

   ​

2. style 绑定

   ```html
   <div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>

   data: {
     activeColor: 'red',
     fontSize: 30
   }

   // 或者
   <div v-bind:style="styleObject"></div>

   data: {
     styleObject: {
       color: 'red',
       fontSize: '13px'
     }
   }
   ```

   直接绑定一个 JavaScript 对象，形式与 CSS 十分相似。

   ```html
   <div v-bind:style="[baseStyles, overridingStyles]">
   ```

   数组语法将多个样式对象应用到该元素上。




#### 条件渲染：

v-if  v-else

```html
<h1 v-if="ok">Yes</h1>
<h1 v-else>No</h1>
```

template v-if

```html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

v-show

类似于 v-if 的作用，但是只是简单的切换元素的 display 属性，不支持 <template> ，控制的元素会始终渲染并保持在 DOM 中。

v-if 会根据条件创建和销毁元素，而 v-show 控制的元素一开始就存在，只是控制它们不会被显示出来。v-if 有较大的切换消耗，而 v-show 有较大的初始渲染消耗。



#### 列表渲染：

v-for="(item, index) in|of items"

<template v-for>

v-for="(value, key, index) in object"  获取对象中的所有属性

v-for="n in 10"  整数迭代

可以用在组件上，但是需要用 v-bind 绑定参数。



key 类似于一个属性，需要绑定使用，即 v-bind:key="id" 。这里类似于给定一个 ID 用来给元素一个标志，用来识别。文档中的解释没有看懂。



数组更新检测

变异方法：会改变原数组，触发视图更新。

push(), pop(), shift(), unshift(), splice(), sort(), reverse()

重塑方法：不会改变原数组，而总是返回一个新数组。

filter(), concat(), slice()

数组替换时，会最大重用，是非常高效的。



直接通过索引赋值和改变数组长度时，不会触发更新，需要使用 set 和 splice 来实现相同的效果。



#### 事件处理器：

v-on 监听事件

如： v-on:click="method"

事件修饰符：

method.stop/.prevent/.capture/.self

修饰符可以串联

按键修饰符：

v-on:keyup.<keyCode>/.enter/tab/delete/esc/space/up/down/left/right

可以自定义按键修饰符别名：

```javascript
// 可以使用 v-on:keyup.f1
Vue.config.keyCodes.f1 = 112
```



#### 表单控件：

v-model 进行表单的双向绑定。写在标签里面就可以了，用来指定那个 Vue 的属性被绑定了。

补充：

```javascript
<input
  type="checkbox"
  v-model="toggle"
  v-bind:true-value="a"
  v-bind:false-value="b">
```

当 checkbox 在 true 和 false 直接切换时， toggle 则在 true-value 和 false-value 直接切换。

修饰符：

.lazy 不是即时改变，而是在触发了 change 事件后，例如 input 输入后回车或者失去焦点的时候触发。

.number 表示将 input 的输入框转为 Number 类型，如果不能转换，则使用原值。

.trim 去除首尾空格。

#### 组件：

一个重要的概念，可以用于扩展许多自定义的功能。

官方解释：

> 组件可以扩展 HTML 元素，封装可重用的代码。在较高层面上，组件是自定义元素， Vue.js 的编译器为它添加特殊功能。在有些情况下，组件也可以是原生 HTML 元素的形式，以 is 特性扩展。

自定义全局组件：

```javascript
Vue.component('tag-name', {
  // options
});
// 可以在父实例的模块中以元素 <tag-name></tag-name> 的形式使用。先注册，后初始化。
```

局部注册组件：

```javascript
var Child = {
  template: '<div>A custom component!</div>'
}

new Vue({
  // ...
  components: {
    // <my-component> 将只在父模板可用
    'my-component': Child
  }
})
```

DOM 模板：

```html
<table>
  <!-- 受到 <table> 的限制，这里不能写 <my-row></my-row> -->
  <tr is="my-row"></tr>  
</table>
```

Options ：

template ：模板

data ：需要写成函数，返回一个对象引用

##### 父子组件之间的信息传递：

为了相对解耦，父子组件直接的关系是 props down, events up 。

```javascript
Vue.component('child', {
  // 声明 props
  props: ['message'],
  // 就像 data 一样，prop 可以用在模板内
  // 同样也可以在 vm 实例中像 “this.message” 这样使用
  template: '<span>{{ message }}</span>'
})
```

tips ：字面量传递的值都是字符串，使用 v-bind 表示传递的值是一个 JavaScript 表达式。

props 是单项数据流动，子组件不能传值给父组件，也不建议修改 props 的值。传递的这个 props 是引用类型（对象和数组）。

可以指定传入的 props 的类型：

```javascript
Vue.component('example', {
  props: {
    // 基础类型检测 （`null` 意思是任何类型都可以）
    propA: Number,
    // 多种类型
    propB: [String, Number],
    // 必传且是字符串
    propC: {
      type: String,
      required: true
    },
    // 数字，有默认值
    propD: {
      type: Number,
      default: 100
    },
    // 数组／对象的默认值应当由一个工厂函数返回
    propE: {
      type: Object,
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        return value > 10
      }
    }
  }
})
```

自定义事件（子到父）：

$on(eventName) 监听事件

$emit(eventName) 触发事件

在子组件中可以用 $emit(eventName) 发射事件，然后在父组件中直接用 v-on:eventName 来监听事件。

.native 表示原生事件



#### 自定义表单组件：

当使用 v-model 在自定义组件上时，相当于同时设置了 v-bind:value="value" 和 v-on:input="method($event.target.value)" 。



内容分发 <slot> 插槽，在子组件中设置 <slot> 插槽，中间可以放入备选内容，如果没有内容插入则显示备选内容。父组件中插入内容会直接放置到 <slot> 处，如果没有 <slot> 的话，会忽略父组件中插入的内容。

可以给 <slot> 一个 name 属性，这样在父组件中可以根据 slot 属性指定元素插入的具体位置。

可以用动态组件 components 属性来动态通过判断显示或切换不同的组件元素。



##### 杂项：

用 ref="..." 指定一个索引 ID ，表示引用一个组件。用 $refs.ID 来引用

异步组件

```javascript
Vue.component('async-example', function (resolve, reject) {
  setTimeout(function () {
    resolve({
      template: '<div>I am async!</div>'
    })
  }, 1000)
})
```

组件可以递归调用



### 响应式原理

Vue 会对 data 中的属性生成 getter 和 setter ，而对后添加的属性无法再产生 getter/setter ，此时不再是响应式的。但是可以向属性中添加嵌套对象，使用 set 方法。

```javascript
Vue.set(vm.someObject, 'key', value);
// 或者使用实例方法
this.$set(this.someObject, 'key', value);
```

当元素更新之后，并不是立即进行更新和渲染，而是先排入队列然后一次清空处理队列，可以使用 vm.$nextTick(callback) 来设定更新后的回调函数。 



























# 摘要

Vue 的数据驱动是由 Vue 的一个实例对象完成的。注意， Vue 只是一个 js 框架，其类似于 jQuery ，但是有着更简便的操作。

要创建一个 Vue 对象，需要传递一个包含各种属性的对象。

```javascript
var vm = new Vue({...});
```

大括号里的参数大致包含如下内容：

```javascript
{
  el: '#app',  // 绑定 HTML 元素
  data: {  // vm 实例的直接属性值， vm.key 的值为 value
    key: value
  },
  methods { // vm 的方法
    changeMessage: function () {...}
  },
  filters: {  // 管道方法
    toInt: function ...
  },
  computed: {
    int_number: function ... // 计算属性，将函数的返回值作为一个动态属性
  },
  watch: {
    key: function(newV, oldV) {...}  //监听属性变化
  }
}
```

插值表达式： {{ ... }}

Vue 的指令写在 HTML 的标签上，像是属性一样：

- v-on:action  绑定事件到methods
- v-bind:attribute  绑定html已有属性到data
- v-if v-show v-else
- v-for  遍历
- v-text  将vm对象的属性插入到标签的内容处
- v-html  插入 rawHtml
- v-model  数据双向绑定，用在有 input 事件的标签上
- 修饰器，在指令后面以点语法表示，代表一些特殊的功能

### 组件

全局注册组件：

```javascript
Vue.component('my-component', {...});
```

大括号里面是传给该组件的参数，里面包括了模板和其他一些内容。

局部注册：

这种将组件写到 Vue 的实例里面。

```javascript
var Child = {
  template: '<div>A custom component!</div>'
};
var vm = new Vue({
  el: '#example',
  components: {
    'my-component': Child
  }
});
```

组件的参数：

```javascript
{
  template: '<table>...</table>',
  data: function () {
    return {message: 'hello'}
  }
  el: function () {
    return '#example'  // 定义作用域
  }
  props: ['parent', ...],  //接收父组件传递过来的参数
  //或者 props: {
  //       propA: Number, propB: [String, Number], propC: {type: String, required: true}, propD: {type: Number, default: 100}, propE: {type: Object, default: function () {return {...}}}, propF: {validator: function (value) {return value > 10}}, ...
  //}  type 可以是 Stirng, Number, Boolean, Function, Object, Array
}
```

父子组件：

参数往下传，事件往上传。

子组件用 this.$emit('eventName') 发射事件，父组件对该事件进行 v-on 监听。v-on:click.native 用来监听 js 原生事件。

非父子关系的组件之间靠一个新的 vm 对象来传递信息。

### Vue 实例对象的生命周期钩子

beforeCreate, created, beforeMount, mounted, beforeUpdate, updated, beforeDestroy, destroyed



## Vue 的命令行安装

全局安装 vue-cli

> npm install -g vue-cli

创建 vue 项目

> vue init webpack-simple my-project

官方提供五个模板： webpack, webpack-simple, browserify, browserify-simple, simple 。

> cd my-project
>
> npm install
>
> npm run dev / npm run build

在 webpack 中，使用 vue-loader 来对 .vue 组件进行处理。

使用 vue-router 和 Vue.js 来实现路由映射组件到正确的渲染位置。

Webpack 将项目中用到的一切静态资源都视之为模块，模块之间可以互相依赖。Webpack 对它们进行统一的管理以及打包发布。

> npm install -g webpack

参考的配置文件为当前目录下的 webpack.config.js ，但也可以自己指定配置文件。

> webpack -config webpack.custom.config.js

```javascript
module.exports = {
  entry:[
    './app/entry.js'  // 定义了打包的入口文件
  ],
  output: {  // 定义文件输出的位置
    path: __dirname + '/assets/',
    publicPath: "/assets/",
    filename: 'bundle.js'
  }
};
```



# Vuex

Vuex is a **state management pattern + library** for Vue.js applications.

Vuex 的基本思想是将多个 Vue 实例中需要重用的数据（state）和方法（action）提取出来称为全局单例，供 DOM 树中的所有实例对象共享使用。

当项目比较大的时候，用 Vuex 管理这些组件外公共 state 信息传递将会变得轻松，它提供的是一种平衡策略，对长期的管理十分有效，但需要使用额外的规则和语法。

安装：

> npm install vuex

使用模块化系统时，需要使用 Vue.use(Vuex) 显示加载。

### Store

Vuex 存放全局 state 的核心。Store 是响应式的，Store 中的 state 发生改变，使用这些 state 的组件也会随着改变，不能直接改变 Store 中的 state 需要 **committing mutations** 来帮助追踪改变。

#### 使用

Store 中有两个基本参数，定义 state 和 mutations（变异）。

```javascript
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})

store.commit('increment') // 主要目的是方便阅读
console.log(store.state.count) // -> 1
```



# Element

Element 提供方便的 Vue 组件给用户使用。

- 安装

  > npm i element-ui -D

  或者使用 CDN

- 使用

  ```
  |- src/  --------------------- 项目源代码
      |- App.vue
      |- main.js  -------------- 入口文件
  |- .babelrc  ----------------- babel 配置文件
  |- index.html  --------------- HTML 模板
  |- package.json  ------------- npm 配置文件
  |- README.md  ---------------- 项目帮助文档
  |- webpack.config.js  -------- webpack 配置文件
  ```

  这里需要的三个配置文件是用来管理依赖包的。

  引入 Element

  -    完整引入

       ```javascript
       //main.js
       import Vue from 'vue'
       import ElementUI from 'element-ui'
       import 'element-ui/lib/theme-default/index.css'
       //样式文件要单独引入
       import App from './App.vue'

       Vue.use(ElementUI);

       new Vue({
         el: '#app',
         render: h => h(App)
       })
       ```

  -    按需引入

       ```javascript
         //npm install babel-plugin-component -D
         //.babelrc
         {
           "presets": [
             ["es2015", { "modules": false }]
           ],
           "plugins": [["component", [
             {
               "libraryName": "element-ui",
               "styleLibraryName": "theme-default"
             }
           ]]]
         }

         //main.js
         import Vue from 'vue'
         import { Button, Select } from 'element-ui'
         import App from './App.vue'

         Vue.component(Button.name, Button)
         Vue.component(Select.name, Select)
         /* 或写为
       * Vue.use(Button)
       * Vue.use(Select)
       */
       ```
      new Vue({
        el: '#app',
        render: h => h(App)
      })
      ​```

- 国际化

  默认是中文，可以引入其他 Element 附带语言。

  ```javascript
  //main.js
  import ElementUI from 'element-ui';
  import lang from 'element-ui/lib/locale/lang/en';
  Vue.use(ElementUI, {lang});
  // 按需要引入
  import lang from 'element-ui/lib/locale/lang/en';
  import locale from 'element-ui/lib/locale';
  locale.use(lang); // 设置语言
  ```

  使用其他语言时，中文默认依旧是被引入的，可以使用 webpack 替换默认语言。

  ```javascript
  //webpack.config.js
  {
    plugins: [
      new webpack.NormalModuleReplacementPlugin(/element-ui[\/\\]lib[\/\\]locale[\/\\]lang[\/\\]zh-CN/, 'element-ui/lib/locale/lang/en')
    ]
  }
  ```

- 自定义主题

  > //安装工具
  >
  > npm i element-theme -g
  >
  > //安装默认主题
  >
  > npm i element-theme-default -D
  >
  > npm i https://github.com/ElementUI/theme-default -D
  >
  > > et -i [可以自定义变量文件目录]
  > >
  > > ✔ Generator variables file
  >
  > 修改 element-variables.css 相应内容
  >
  > > et  //编译主题
  > >
  > > ✔ build theme font
  > >
  > > ✔ build element theme
  >
  > 在 main.js 和 .babelrc 中引入自定义主题路径
  >
  > 完成

- 基础组件

  - 基本

    - Layout

      共可以分为 24 栏

      标签：

      `<el-row>` ： `:gutter="20"` 分栏间隔， `type="flex"` `justify="start | center | end | space-between | space-around"` 水平对齐方式， `align="top | middle | bottom"` 垂直对齐方式

       `<el-col :span="24">` ： `:offset="6"` `:push="6"` `:pull="6"` 偏移， `xs` `sm` `md` `lg` number/object

    - Color

      `Light Blue, Blue, Dark Blue, ... `

    - Typography

      ```css
      font-family: "Helvetica Neue",Helvetica,"PingFang SC","Hiragino Sans GB","Microsoft YaHei","微软雅黑",Arial,sans-serif;
      ```

      正文用 14px 字号 small

    - Icon

      `<i class="el-icon-delete"></i>` 

    - Button

      标签： `<el-button>` ： `type="default | primary | text | info | success | warning | danger"` ， `:disabled="true"` ， `:plain="true"` 空心按钮， `icon="delete"` 给按钮设置图标， `:loading="true"` ， `size="large | small | mini"` ， `:autofocus="false"` ， `native-type="button"` 原生 type 属性

      `<el-button-group>` 

  - 表单

    - Radio

      标签：

      `<el-radio>` ：`v-model="radio"` `label="checked"` `class="radio"` `disabled` 

      单选组：

      ```html
      <template>
        <el-radio-group v-model="radio2">
          <el-radio :label="3">备选项</el-radio>
          <el-radio :label="6">备选项</el-radio>
          <el-radio :label="9">备选项</el-radio>
        </el-radio-group>
      </template>
      <!-- 按钮样式 -->
      <template>
        <el-radio-group v-model="radio3">
          <el-radio-button label="上海"></el-radio-button>
          <el-radio-button label="北京"></el-radio-button>
          <el-radio-button label="广州" :disabled="true"></el-radio-button>
          <el-radio-button label="深圳"></el-radio-button>
        </el-radio-group>
      </template>
      ```

      还可以有 @change 事件， `fill` `text-color` 颜色属性

    - Checkbox

      ```html
      <el-checkbox v-model="checked" checked>备选项</el-checkbox>
      <el-checkbox v-model="checked" disabled>备选项</el-checkbox>

      <el-checkbox-group v-model="checkList">
          <el-checkbox label="复选框 A"></el-checkbox>
          <el-checkbox label="复选框 B"></el-checkbox>
          <el-checkbox label="复选框 C"></el-checkbox>
          <el-checkbox label="禁用" disabled></el-checkbox>
          <el-checkbox label="选中且禁用" disabled></el-checkbox>
        </el-checkbox-group>
      ```

      其他：`true-label` `false-label` `:indeterminate="false"` `@change` 

    - Input

      ```html
      <el-input
        placeholder="请输入内容"
        v-model="input"
        :disabled="true"
        icon="time">
      </el-input>
      <!-- 文本域 -->
      <el-input
        type="textarea"
        :autosize="{ minRows: 2, maxRows: 4}"
        placeholder="请输入内容"
        v-model="textarea">
      </el-input>
      <!-- 混合实例 -->
      <el-input placeholder="请输入内容" v-model="input5" style="width: 300px;">
          <el-select v-model="select" slot="prepend" placeholder="请选择">
            <el-option label="餐厅名" value="1"></el-option>
            <el-option label="订单号" value="2"></el-option>
            <el-option label="用户电话" value="3"></el-option>
          </el-select>
          <el-button slot="append" icon="search"></el-button>
      </el-input>

      <!-- 带输入建议 -->
      <el-autocomplete
        class="inline-input | my-autocomplete"  //这里可以设定自定义的样式
        v-model="state"
        :fetch-suggestions="querySearch"
        custom-item="my-item-zh"  //这里是给定一个自定义组件
        placeholder="请输入内容"
        :trigger-on-focus="true | false"
        @select="handleSelect"
      ></el-autocomplete>
      ...
      ```

      其他： `size="large | small | mini"` `value` `maxlength` `minlength` `rows` `autosize` `autofocus` `form` `auto-complete="off"` 

    - InputNumber

      ```html
      <el-input-number v-model="num1" @change="handleChange" :min="1" :max="10" :disabled="true" :step="1"></el-input-number>
      </template>
      ```

      其他： `controls="true"` 是否使用控制按钮

    - Select

      ```html
      <el-select v-model="value" placeholder="请选择" disabled clearable>
          <el-option
            v-for="item in options"
            :label="item.label"
            :value="item.value"
            :disabled="item.disabled">
          </el-option>
        </el-select>

      <!-- 多选 -->
      <template>
        <el-select v-model="value5" multiple placeholder="请选择">
          <el-option
            v-for="item in options"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
      </template>

      <!-- 自定义样式 -->
      <el-select v-model="value6" placeholder="请选择">
          <el-option
            v-for="item in cities"
            :label="item.label"
            :value="item.value">
            <span style="float: left">{{ item.label }}</span>
            <span style="float: right; color: #8492a6; font-size: 13px">{{ item.value }}</span>
          </el-option>
        </el-select>

      <!-- 分组 -->
       <el-select v-model="value7" placeholder="请选择">
          <el-option-group
            v-for="group in options3"
            :label="group.label">
            <el-option
              v-for="item in group.options"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-option-group>
        </el-select>

      <!-- 可搜索 -->
      <el-select v-model="value8" filterable :filter-method="function" placeholder="请选择">
          <el-option
            v-for="item in options"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>

      <!-- 远程搜索 -->
      <el-select
          v-model="value9"
          multiple
          filterable
          allow-create    //允许创建新的选项
          remote
          placeholder="请输入关键词"
          :remote-method="remoteMethod"
          :loading="loading">
          <el-option
            v-for="item in options4"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
      ```

      其他： `:multiple-limit="0"` `loading="false"` 

    - Switch

      ```html
      <el-switch
        v-model="value1"
        on-text=""
        off-text="">
      </el-switch>
      <el-switch
        v-model="value2"
        on-color="#13ce66"
        off-color="#ff4949"
        disabled>
      </el-switch>
      ```

      其他： `width` `on-icon-class` `off-icon-class` 

    - Slider

      ```html
      <template>
        <div class="block">
          <span class="demonstration">默认</span>
          <el-slider v-model="value1"></el-slider>
        </div>
        <div class="block">
          <span class="demonstration">自定义初始值</span>
          <el-slider v-model="value2"></el-slider>
        </div>
        <div class="block">
          <span class="demonstration">禁用</span>
          <el-slider v-model="value3" disabled></el-slider>
        </div>
        <div class="block">
          <span class="demonstration">不显示间断点</span>
          <el-slider
            v-model="value4"
            :step="10">
          </el-slider>
        </div>
        <div class="block">
          <span class="demonstration">显示间断点</span>
          <el-slider
            v-model="value5"
            :step="10"
            show-stops>
          </el-slider>
        </div>
        <div class="block">
          <el-slider
            v-model="value6"
            show-input>
          </el-slider>
        </div>
      </template>
      ```

      其他： `min` `max` 

    - TimePicker

    - DatePicker

    - DateTimePicker

    - Upload

    - Rate

    - Form

  - 数据

    - Table

      ```html
      <!-- 基础表格 -->
      <template>
          <el-table
            :data="tableData"
            style="width: 100%"
            stripe  //斑马纹
            border  //竖线 
            :row-class-name="tableRowClassName"  //添加行样式
             height="250"  //控制表格高，这样可以固定表头
             max-height="250">
            <el-table-column
              prop="date"
              label="日期"
              width="180"
              fixed="true | false | left | right">
            </el-table-column>
            <el-table-column
              prop="name"
              label="姓名"
              width="180">
            </el-table-column>
            <el-table-column
              prop="address"
              label="地址">
            </el-table-column>
          </el-table>
        </template>

      <!-- 单选 -->
      <el-table
          :data="tableData"
          highlight-current-row
          @current-change="handleCurrentChange"
          style="width: 100%">
          <el-table-column
            type="index"  //索引列
            width="50">
          </el-table-column>
      </el-table>

      <!-- 多选和特殊数据用法 -->
      <template>
        <el-table
          :data="tableData3"
          border
          style="width: 100%"
          @selection-change="handleSelectionChange">
          <el-table-column
            type="selection"  //多选列
            width="55">
          </el-table-column>
          <el-table-column
            inline-template  //可以自定义列的内容
            label="日期"
            sortable  //可排序
            width="120">
            <div>{{ row.date }}</div>  //inline-template的内容
          </el-table-column>
          <el-table-column
            prop="name"
            label="姓名"
            width="120">
          </el-table-column>
          <el-table-column
            prop="address"
            label="地址"
            :formatter="Function(row, column)"
            show-overflow-tooltip>  //不折行，以tooltip形式显示
          </el-table-column>
          <el-table-column
            prop="tag"
            label="标签"
            width="100"
            :filters="[{ text: '家', value: '家' }, { text: '公司', value: '公司' }]"
            :filter-method="filterTag(value, row)"    //筛选
            inline-template>
            <el-tag :type="row.tag === '家' ? 'primary' : 'success'" close-transition>{{row.tag}}</el-tag>
          </el-table-column>
        </el-table>
      </template>
      ```

      其他：多级表头，可以嵌套列标签实现。

      `fit` `show-header` `row-style` `row-key` `context` `empty-text` 

      事件：`select` `select-all` `selection-change` `cell-mouse-enter` `cell-mouse-leave` `cell-click` `row-click` `row-contextmenu` `row-dblclick` `header-click` `sort-change` `current-change` 

      方法：`clearSelection` `toggleRowSelection` 

      列属性：`min-width` `render-header` `sort-methord` `resizable` `align` `class-name` `selectable` `reserve-selection` `filters` `filter-multiple` `filter-method` `filtered-value` 

    - Tag

    - Progress

    - Tree

    - Pagination

    - Badge

  - 注意信息

    - Alert
    - Loading
    - Message
    - MessaeBox
    - Notification

  - 导航

    - NavMenu
    - Tabs
    - Breadcrumb
    - Dropdown
    - Steps

  - 其他

    - Dialog
    - Tooltip
    - Popover
    - Card

