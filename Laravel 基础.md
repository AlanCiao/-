## Laravel 基础

[TOC]

### Composer

php 依赖包管理器， https://packagist.org

通过 Composer 可以下载各种包和框架，包括 Laravel 。

https://getcomposer.org 可以下载 Composer ，以及获得相关信息。

composer.phar 就是程序文件，安装需查看文档。

> composer require <vendorName>/<packageName>
>
> 下载依赖包，下载的同时，会同样下载该包的依赖包

composer.json 用于记录下载的依赖， vendor 目录下存放下载的依赖包，其中的 bin 目录存放可执行文件。

> composer create-project laravel/laravel learning-laravel-5 [dev-develop]
>
> 创建 Laravel5 项目，最后的选项表示下载正在开发的分支

创建一个服务器来运行 Laravel

> php -S localhost:8888 -t public

### Homestead

一个虚拟机系统，提供操作系统、数据库、php 等一系列环境，可以在这个虚拟机系统上运行 Laravel 框架。

首先，需要安装虚拟机  Virtual Box ，然后是  Vagrant 。

> vagrant box add laravel/homestead
>
> 安装依赖包
>
> composer global require "laravel/homestead=~2.0"
>
> 安装 Homestead
>
> homestead init/edit
>
> 启动或编辑
>
> homestead up
>
> 启动虚拟机

### Routes

app/Http/routes.php

路由文件，用来进行将 url 对应到一个指定的类或方法中处理的文件。

examples:

```php
Route::get('/', 'WelcomeController@index');

// WelcomController
  public function index()
  {
    //return 'Hello World';
    return view('welcome');
  }

// resources/views/welcome.blade.php
// 文件名层级可以用点式替代
// frame/blog == frame.blog
```

#### RESTful

> php artisan route:list
>
> 列出所有的路由
>
> Route::resource('url', 'controller');
>
> RESTful 路由

### Views

#### data

```php
// 传递参数到view
$name = 'Alan Ciao';
// 1 with
return view('welcome')->with('name', $name);
// 2 数组
return view('welcome')->with([
  'first' => 'Alan',
  'last'  => 'Ciao'
]);
// 3 参数
return view('welcome', $data);
// 4 compact
$first = 'Alan';
$last = 'Ciao';
return view('welcome', compact('first', 'last'));

// 使用
{{ $name }}
{{ $fast }} {{ $last }}
// 非逃逸显示
{!! $name !!}
```

#### Blade

模板文件：

> app.blade.php
>
> @yield('content')
>
> 用于插入其他片段的地方，'content' 是一个表示符。
>
> 
>
> 使用
>
> @extends('app')
>
> @section('content')
>
> ​	...... // 这里的内容会插入到 'content' 的部分
>
> @stop
>
> 
>
> 包含文件
>
> @include('filename'[, ['varibles' => 'value']])

条件控制

> @if (...) / @unless (...)
>
> ​	......
>
> @endif

循环控制

> @foreach ($items as \$var)
>
> ​	......
>
> @endforeach

#### href

```html
<a href="/article/{{ $article->id }}">{{ $article->title }}</a>

<a href="{{ action('ArticlesController@show', [$article->id]) }}">{{ $article->title }}</a>

<a href="{{ url('/articles', $article->id) }}">{{ $article->title }}</a>

<a href="{{ route('routeName') }}">{{ $article->title }}</a>
```

### Elixir

#### gulp

管理前端文件的工具，存放在 package.json 文件中，由 Nodejs 获取。

> npm install
>
> gulp    //编译 LESS 或 SASS 文件
>
> gulp watch
>
> gulp --production

Elixir 资源管理文件为 gulpfile.js

```javascript
var elixir = require('laravel-elixir');

elixir(function(mix) {
  mix.sass('app.scss');  //resources\assets\sass\app.scss
  // or mix.less('app.less');
  // other file
  mix.sass('app.scss').coffee(); //resources\assets\coffee\...
  
  mix.styles([...], null, 'public/css'); //merging css files, (files[], outputBasePath, basePath)
  mix.scripts([...]);
  
  // 测试
  mix.phpUnit()[.phpSpec()];
  
  mix.version('filename');  // 给文件增加版本号
  // href="{{ elixir('css/all.css') }}"
  // 会参考 rev-manifest.json 中的映射
});

gulp
gulp watch
gulp tdd
```

### Controller

> php artisan make:controller <Name> [--plain]
>
> 创建控制器，如果不加参数，会创建 RESTful 控制器
>
> php artisan help make:controller
>
> 获取帮助

重定向

> return redirect('url');

### Middleware

中间件是用来处理和过滤请求的，中间件类中有一个 handle 方法，处理过程在这里完成。中间件需要注册到 Kernel.php 文件中，有两种模式， middleware 针对所有请求，而 routeMiddleware 可以给特定路由添加中间件。

添加的中间件可以写在控制器的构造方法中。

```php
public function __construct() 
{
  $this->middleware('auth', ['only' => 'create']);
  //$this->middleware('auth', ['except' => 'create']);
}

//在路由中直接添加中间件
Route::get('about', ['middleware' => 'auth', 'uses' => 'PagesController@about']);  //uses 也可以直接使用一个闭包
```

创建中间件

> php artisan make:middleware <Name>

```php
public function handle($request, Closure $next) 
{
  $response = $next($request);
  //
  return $response;
}
```

### Config

该目录下包含 Laravel 的所有配置文件，并且有详细的注释说明。其中包括数据库等的设置。

### Migrations

用于创建数据库表结构的文件。

包含两个方法 up 和 down 。

```php
public function up()
{
	Schema::create('articles', function (Blueprint $table))
    {
      $table->increments('id');
      $table->integer('user_id')->unsigned();
      $table->timestamps();
      
      $table->foreign('user_id')
            ->references('id')
            ->on('users')
            ->onDelete('cascade');
    }
}

public function down()
{
  Schema::drop('users');
}
```

> migration 的命令
>
> php artisan make:migration create_articles_table [--create="articles"]    // 创建迁移文件
>
> php artisan migrate    // 运行迁移，创建表
>
> php artisan migrate:rollback    // 取消表迁移
>
> php artisan migrate:refresh    // 回滚表，然后运行迁移
>
> php artisan make:migration add_column_to_articles_table [--table="articles"]   // 创建添加列的迁移文件

如果需要 dropColumn 的话，需要安装一个依赖

> composer require doctrine/dbal

### Service Provider

#### Route-Model-Binding

在 RouteServiceProvider 中进行，可以在 boot 方法中实现路由模型绑定。当传入一个参数时（例如 id ）， Laravel 会自动将其封装成绑定的对象传入参数。这样，在路由传入控制器参数的时候，不需要手动去查找模型，而是交给 Laravel 自动完成。

```php
public function boot(Router $router) 
{
  parent::boot($router);
  
  $router->model('articles', 'App\Article')
  // Route::model('RouteKey', 'Model');
    
  //另一种方式
  $router->bind('articles', function($id) 
  {
    return App\Article::published()->findOrFail($id);
  })
}
```

### Eloquent

与表对应的模型，一般表名以复数命名，模型以相应的单数命名。

> php artisan make:model <Name>

所有的模型都会继承 Model 类。

```php
class Article extends Model {
  protected $fillable = {
    'title',
    'body',
    'published_at'
  };
  
  // 可以将该字段以时间对象 Carbon 返回
  protected $dates = ['published_at'];
  
  // setNameAttribute 设置属性
  // 同理也存在 getNameAttribute 在取得属性时进行操作
  public function setPublishedAtAttribute($date) 
  {
    $this->attributes['published_at'] = Carbon::createFromFormat('Y-m-d', $date);
    // Carbon::parse($date)
  }
  
  // Query Scope 获取数据时的过滤器，添加额外的统一的查询条件
  public function scopePublished($query) 
  {
    $query->where('published_at', '<=', Carbon::now());
  }
}
```

> php artisan tinker
>
> 一个命令行操作系统，可以直接在命令行中执行 php 代码。可以进行一些测试。

对于一个 Model ，它的属性可以直接用箭头引用进行访问，如

```php
$article = new App\Article;
$article->date = Carbon\Carbon::now();
$article->toArray();  // 将对象变成数组展示出来

$article->save();  // 将对象持久化
App\Article::all();  // 获取全部
App\Article::latest()->get();
$article = App\Article::find(1);
$article = App\Article::findOrFail(1);    // 按id获取
$articles = App\Article::where(...)->get();  // 生成查询构造器，返回一个结果集合
// ->first() 用来获取第一个结果

$article = App\Article::create([...]);
// 这里需要指定模型中的 $fillable 属性，允许批量赋值
$article->update([...]);
```

抛出异常

> abort(404);

#### Relationships

##### 一对多 / 多对一

注意，调用的时候如果没有括号，返回的是一个结果集合，如果带有括号，返回一个对象，可以继续链式调用查询方法。

```php
class Article extends Model {
  ...
  public function user() 
  {
    return $this->belongsTo('App\User');
  }
}

class User extends Model {
  ...
  public function articles() // 这个名字可以任意，相当于调用一个方法
  {
    return $this->hasMany(Article::class);
  }
}
```

##### 多对多 / 标签

多对多关系的两张表 articles 和 tags 之外，还要有一个中间表，被称为 pivot table ， article_tag 。

```php
class Article extends Model {
  ...
  public function tags() 
  {
    return $this->belongsToMany('App\Tag')->withTimestamps();
  }
}

class Tag extends Model {
  ...
  public function articles() 
  {
    return $this->belongsToMany('App\Article');
  }
}

$article->tags()->attach($tagId);
```



### Utils

#### Form

可以使用 FormBuilder 和 FormFacade 这两个类。还可以使用 HtmlFacade 和 HtmlBuilder 。

```php
{!! Form::open(['url' => 'articles']) !!}
// Form::model($article, ['method' ...])  这个方法可以绑定模型数据到表单
	{!! Form::label('name', 'Name:') !!}
	{!! Form::text('name', null, ['class' => 'form-control', 'foo' => 'bar']) !!}

	{!! Form::label('body', 'Body:') !!}
	{!! Form::textarea('body', null, ['class' => 'form-control']) !!}

	{!! Form::submit('Add Article', ['class' => 'btn btn-primary']) !!}
{!! Form::close() !!}

// 显示错误， errors 变量会自动传递到页面，它是 ViewErrorBag 实例对象
@if ($errors->any())
  @foreach ($errors->all() as $error)
    <li>{{ $error }}</li>
  @endforeach
@endif
```

#### 确认信息 Flash Message

```php
Session::flash('flash_message', 'Your article has been created!');
session()->flash('flash_message_important', true);
return redirect('articles')->with([
  'flash_message' => 'Your article has been created',
  'flash_message_important' => true
]);
// set 将数据存入 session ，而 flash 只存入一次，使用后就会被删除。

//简化的方法
flash('Your article has been created')->important();
//下载工具包，添加 ServiceProvider 即可使用，也可以添加 Facade

@if (Session::has('flash_message'))
  <div>{{ Session::get('flash_message') }}</div>
@endif
```



#### Date

```php
{!! Form::label('published_at', 'Publish On:') !!}
{!! Form::input('date', 'published_at', Carbon\Carbon::now()->format('Y-m-d'), ['class' => 'form-control']) !!}

//or date('Y-m-d')
```

#### Validation

一个对表单进行验证的方法，当不满足时会抛出异常。

有两种解决方法，第一个是自己创建一个 Request 类，该类继承自 Illuminate\Http\Request ，该类可以对原 Request 对象进行加强，主要用于校验功能；另一种方法是用 Laravel 提供的 validate 方法，简单地对 Request 对象中的字段进行校验。 

> php artisan make:request <Name>

```php
class CreateArticleRequest extends Request {
  
  public function authorize() 
  {
    return true;  // 用户验证，指定可以使用这个类的用户，返回 true ，否则返回 false 。
  }
  
  public function rules() 
  {
    return [
      'title' => 'required|min:3',
      'body' => 'requered',
      'published_at' => 'required|date'
      // 'published_at' => ['required', 'date']
    ];
  }
}

//使用： public function store(CreateArticleRequest $request) {...} ，如果失败，会返回上一个页面。

// trait ValidatesRequests
// public function validate(Request $request, array $rules)
$this->validate($request, [...]);
```

#### Auth

```php
Route::controllers([
  'auth' => 'Auth\AuthController',
  'password' => 'Auth\PasswordController',
]);

\Auth::user()  // 获得登录用户
  
Auth::user()->articles()->save(new Article($request->all()));
```



