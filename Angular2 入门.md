package.json

依赖包包含两种 dependencies 和 devDependencies

前者是运行的基础，后者是只在开发应用是才用得到。

npm install admin --production 命令会只安装 dependencies 。



`dependencies `区下有三类包：

>  **特性** - 特性包为应用程序提供了框架和工具方面的能力。

>  **填充(Polyfills)** - 填充包弥合了不同浏览器上的JavaScript实现方面的差异。

>  **其它** - 其它库对本应用提供支持，比如`bootstrap`包提供了HTML中的小部件和样式。







Module 模块

一个 Angular 应用必须至少有一个根模块 AppModule ，根模块是应用的入口。模块中一定会带有 @NgModule 装饰器类。可以有很多个特性模块，一个模块就是完成一整个功能的代码部分。装饰器类将元数据（传入 JavaScript 类中的参数）附加到模块类上。



@NgModule 里面有几个属性：

declarations 中声明的是本模块拥有的视图类，这些类用来生成视图，又进一步分成组件、指令和管道。

exports 是声明的自己，用于其他模块的组件模板。我理解就是向其他模块暴露出数据或视图交换的类，或者是模板？

imports 从其他模块获取的组件模板

providers 其中声明所要用到的服务类，这些服务可以供全局使用，产生单例供所有其他的组件共享。

bootstrap 指定应用的主视图（根组件），只有根模块能设置这个属性，其中声明的主视图是其他视图的宿主，就是一个最基本的框架，其他的代码都会放到这个视图上来。



通常在 main.ts 文件中引导根模块 AppModule 启动。



Component 组件

组件于视图相对应，一个组件就是用来显示大视图上的一小部分或一块区域，通过模板实现视图的渲染，显示所需要的部分。组件控制视图的显示，是通过一些 API 来完成交互的。

@Component 是组件的装饰类，配置项包括：

moduleId 为与模块相关的URL（如模板文件）提供基地址，一般写作 module.id 。

selector 给出自定义标签，即元素插入父级 HTML 中的位置。

templateUrl 组件的 HTML 模板地址，指向一个 HTML 模板文件。

providers 服务的依赖注入提供商，就是服务类，在这里声明这个服务类，就可以通过依赖注入自动提供服务类的示例了。



Template 模板

模板就是组件中要渲染的 HTML 代码，其中还包含要交互的数据内容，同时还包含一些 Angular 的控制指令。类似于一些数据绑定和逻辑处理部分。

在组件里可以自定义标签，用于将组件显示在其他组件的模板中。



Metadata 元数据

元数据告诉 Angular 如何处理一个类，这些元数据写在类装饰器中，用于给组件等部分设置属性。把元数据传递给一个类，它才会被认为是组件，并用组件的方式去处理。Angular 会基于这些元数据提供的信息来创建和展示组件及视图。



模板、元数据和组件共同描绘出一个视图。通过元数据让 Angular 自动完成任务，渲染视图。



Data Binding 数据绑定

数据绑定就是实现组件内容和模板之间的数据传递，将所有的事情都自动化了，不需要手工写繁琐的代码，而是通过绑定轻松搞定。只要在模板 HTML 中添加一些数据绑定标记，Angular 就会自动完成数据的传递和处理，将组件中的数据动态地显示在视图中。

有三种形式：

{{data}} 数据由组件流向 DOM，插值表达式。

[data]=“field” 数据是动态的，随时改变，属性绑定，在父子组件中传递数据，把父组件中的属性赋值给子组件中的 data 属性。

（event）=“handler（）” 事件绑定，一旦触发事件，就会调用组件中的相应方法。

[(ngModel)]=“data” 双向绑定。



Directive 指令

指令也是一个类，带有“指令元数据”，通过 @Directive 装饰器把元数据附加到类上。

组件有是一个带模板的指令。实际上 一般的组件装饰器都是一个指令装饰器，这样也可以看出组件十分重要，同时理解指定就是通过装饰器类指定如何处理一个类。



另外，还有结构型指令和属性型指令。结构型指令通过操作 DOM 来改变布局。*ngFor 和 *ngIf 就是结构型指令。这就很像是 JSP 标签了。这两种指令都是应用在模板 HTML 标签中，好像是标签的属性一样。双向数据绑定就是一个属性型指令的例子。



Service 服务

一个专门提供某一项业务逻辑的类。一般可以用于前端显示和后台数据获取之间的链接。服务类里面就会有一堆方法供服务消费者使用，它里面包含着业务逻辑的。服务也可以依赖于另外一个或多个服务。

设计良好的组件为数据绑定提供属性和方法，把那些其他对它们不重要的事情都委托给服务。



Dependency Injection 依赖注入

依赖注入将服务自动注入到组件当中，供组件使用。所有需要注入的服务必须在注入器中注册一个提供商 Provider 。可以在模块上，也可以在组件上注册提供商。通常是放在根模块上，这样就可以全局使用同一个服务实例了。

提供商其实就是一个服务类。在 providers 中写上这个类就算是注册到注入器了。





快速起步——环境搭建

1. 需要 Node.js 和 npm
2. 需要的依赖文件：
   - package.json：npm依赖包
   - tsconfig.json：TypeScript 编译器配置
   - systemjs.config.js：模块加载器，查找需加载模块信息，注册依赖包；也可以使用 Webpack 来加载模块
3. 安装 npm 依赖包
   - npm install
   - 会多出 node_modules 目录
4. Angular 的运行以模块为单位，应用必须存在一个根模块 app.module.ts
5. 应用至少要有一个根组件 app.component.ts 。组件中包含 HTML 模板和组件类，组件类控制视图。Angular 使用 TypeScript 编写。
6. 需要一个启动文件 main.ts （即时 JiT 编译，也可以用其他引导方式）
7. 需要一个宿主页面 index.html
8. 运行 npm start




### 显示数据

1. 插值表达式  {{field}}  field 是组件类的属性
2. *ngFor="let var of items" 指令，用来循环显示内容
3. *ngIf="条件表达式" 指令，根据条件显示（添加/移除）内容
4. 单向数据绑定，将属性用中括号括起来就完成的组件属性流向元素属性的单向绑定， [attr]="field" 。



Angular2 中定义类中的属性：

```typescript
export class Person {
  constructor(
  	public id: number,
    public name: string) {}
}
```

直接在构造器中给出构造参数的声明，就相当于同时定义的类的属性，并将构造参数赋值给属性。



### 用户输入

事件绑定：

点击事件

```html
<button (click)="onClickMe()">
  Click me!
</button>
```

键盘事件

$event 代表产生的事件，可以传给函数。不建议使用，因为破坏了模板和组件控制部分的分离。建议使用模板引用变量 #var ，代表它所引用的 DOM 元素本身。

按键事件过滤器 keyup.enter ，对回车进行响应。

失去焦点事件

(blur)



### 表单

需要使用 import {FormsModule} from '@angular/forms'; 模块

双向数据绑定

[(ngModel)]="var" ，在表单中使用双向绑定时，必须要使用 name 属性， name 属性用于将绑定的元素注册到 Angular 内部的一个 FormControl 键。

这里的中括号表示单向数据绑定，小括号表示反向数据绑定（事件绑定），数据由 view 流向 model 。

等价形式： [ngModel]="var" (ngModelChange)="var=$event"

ngModel 还为元素添加了几个状态 class ，可以根据这些类选择器来控制不同状态下的样式。

\#var="ngModel" 绑定指令到状态引用变量，属性 var.valid、var.pristine

\#var="ngForm"  绑定表单指令，var.reset() 重置表单

ngSubmit 表单提交事件

[hidden]="boolean" 隐藏属性



### 依赖注入

依赖注入是一个很重要的概念，几乎在任何一门语言中都有体现，所以应该看做是一种设计思想或模式。实际上比较好理解，就是一个类需要用到其他类，或者说依赖到其他的类，不需要手动 new ，而是作为参数来接收，并且这些参数也不需要手动创建并传入，而是由框架后台自动完成创建。因此产生了注入器，它会在创建一个对象时，自动注入该对象所需的依赖。

一般依赖注入依靠一个 Service 类，它提供一个获取对象的一个方法。在 Angular 中，一个类前面加上 @Injectable 指令，就表示它是一个提供依赖注入的服务提供者，标记了注入器的目标。需要在 Angular 注入器中注册这个服务，它才能被自动注入到所需的对象中。

注入器由系统启动时创建，只需要提供 providers 数组就完成了注册。

@Component 、 @Directive 、 @Pipe 是 @Injectable 的子类型。



providers [ Class, { provide: Class, useClass | useExisting: Class | useValue: Object }]

工厂提供：let provider = { provide: Service, useFactory: serviceFactory, deps:[Class, Class, ...] };

注入是是查找令牌（一般为类名）对应的类或对象，但是 TypeScript 中的接口不能作为令牌，当需要注入一个接口对象时，需要使用不透明令牌 OpaqueToken 。

```typescript
import { OpaqueToken } from '@angular/core';
export let APP_CONFIG = new OpaqueToken('app.config');

providers: [{ provide: APP_CONFIG, useValue: HERO_DI_CONFIG }]

constructor(@Inject(APP_CONFIG) config: AppConfig) {
  this.title = config.title;
}
```

可选依赖：

```typescript
import { Optional } from '@angular/core';

constructor(@Optional() private logger: Logger) {
  if (this.logger) {
    this.logger.log(some_message);
  }
}
```



### 模板语法

1. 插值表达式：{{ 模板表达式 }}

2. 模板表达式：一般用于插值表达式和属性绑定中

   模板表达式只能访问到表达式上下文，如组件中的成员或模板引用变量，而不能访问全局成员。

3. 模板语句：一般用于事件绑定中

4. 绑定语法：

   - 单向（组件到视图）：加上方括号才会计算模板表达式的值，否则会被当做一个字符串。

     特殊：[attr.colspan], [style.style-property[.em | px | %]], [class], [class.class-name]="boolean"

   - 单向（视图事件到组件）：加上圆括号表示监听该事件。

     $event 对象用于传递事件，可以是 DOM 事件，也可以是组件自定义的事件。

     用 EventEmitter 类来发射事件

     myEvent = new EventEmitter<Object>()

     myEvent.emit(Object);

     父组件可以监听这个 myEvent 事件并从 $event 中获得 Object

   - 双向：[(x)] 一个组件有 x 属性和 xChange 事件属性，就可以使用双向绑定。

     在表单元素上没有这样的属性，但是可以使用 ngModel 属性在表单元素上进行数据的双向绑定。ngModel 属性属于 FormsModule 模块之中。 ngModel 专门针对表单输入的 value 进行设置和监听的。

   - 内置指令：

     - ngClass ：批量控制添加或移除 class 属性

     - ngStyle ：批量控制样式 style 属性

       以上都要用到一个包含 key:value 的控制对象作为设置属性的值。

     - *ngIf

     - *ngFor="let var of items; let i=index; trackBy:trackFunction"

       这个 trackBy 属性检查指定的值，如果该值不变，就不会重新更新全部的列表，而是仅仅更新改变的内容。

     - [ngSwitch]="var" ... *ngSwitchCase="enum" ... *ngSwitchDefault

       上述对 DOM 的操作都是创建或移除，而不是单纯的控制显示或隐藏。

       指令前面的 * 是 <template> 的简化语法，

   **attribute 是由 HTML 定义的。property 是由 DOM (Document Object Model) 定义的。**

   **attribute 初始化 DOM property，然后它们的任务就完成了。property 的值可以改变；attribute 的值不能改变。**

   Angular 中操作的所有目标都是 DOM 的 property 或事件。

   插值表达式会被翻译成属性绑定，两种形式的效果是一样的，选择自己的风格。

   模板中会忽略 <script> 标签。 

   **We must use attribute binding when there is no element property to bind.**

5. 模板引用变量

   模板中对 DOM 元素或指令的引用，用 #var 或 ref-var 定义。

   ```html
   <form (ngSubmit)="onSubmit(theForm)" #theForm="ngForm">
     <button type="submit" [disabled]="!theForm.form.valid">Submit</button>
   </form>
   ```

   这里的 theForm 引用的是一个内置指令 ngForm ，它是对 HTML 表单对象的一个封装。

##### 输入或输出属性

绑定的目标属性必须显示被标记为输入或输出 @Input()/@Output()

另外一种方式是在 @Component 中标注 inputs:[] 和 outputs:[]

可以给指令起别名：

@Output('alias') propertyName = new EventEmitter<string>();

outputs: ['propertyName:alias']

##### 模板表达式操作符

管道操作符—— 表达式 | 管道函数:'参数'

安全导航操作符 ?. 用来防止 null 和 undefined 情况



### 填充库 polyfills

用来添加填充库，以支持多浏览器。



