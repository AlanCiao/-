# JavaScript

JavaScript 中的所有变量都是一个对象，里面封装了属性和方法。

自己创建对象 var obj = new Object(); 然后给对象设定属性值。或者直接把对象用大括号表示，在大括号里面写上属性和属性值对。

使用 obj.propertyName 来访问属性；使用 obj.methodName() 来访问方法。可以自己向对象中不断添加新的属性和方法，包括已经存在的对象。

未声明的变量（前面没有加 var ）会被当做全局变量。



另一种创建对象的方法：

使用函数构造器，示例如下。

```javascript
function person(firstname,lastname,age,eyecolor)
{
  this.firstname=firstname;
  this.lastname=lastname;
  this.age=age;
  this.eyecolor=eyecolor;
}

// 生成示例对象
var person=new person("Bill","Gates",56,"blue");
```





for/in 循环用于遍历对象中的属性。这里的遍历会取出属性名。

对对象中的每个属性都会遍历。



检测 cookie ：

```html
<body onload="checkCookies()">

  <script>
    function checkCookies()
    {
      if (navigator.cookieEnabled==true)
      {
        alert("已启用 cookie")
      }
      else
      {
        alert("未启用 cookie")
      }
    }
  </script>

  <p>提示框会告诉你，浏览器是否已启用 cookie。</p>
</body>
```



JavaScript 是基于对象的语言，它不会创建类，也不会用类来创建对象。它使用的是 prototype 。



window 对象表示浏览器窗口，所有的全局变量和全局函数都是 window 的成员，浏览器内的一切都自动成为 window 的成员。

设定窗口大小：

```javascript
var w=window.innerWidth
|| document.documentElement.clientWidth
|| document.body.clientWidth;

var h=window.innerHeight
|| document.documentElement.clientHeight
|| document.body.clientHeight;
```



JavaScript 中，函数也是一个对象。 Function 是一个类，所以可以用 new function(arg1, arg2, ..., argN, fun_body) 来定义一个函数。函数名就是一个变量，它是指向 Function 对象的一个指针。

函数的 length 属性返回定义时指定的参数个数。

arguments 是一个很有意思的对象，它封装了传入函数中的所有内容，最大可以达到 255个。也就是说，虽然函数会指定传入参数的个数，但这并不影响实际传入函数中参数的个数。传入的参数可以多也可以少，传少了，多出来的位置用 undefined 补充，传多了，多出来的传入参数就被忽略，但是所有传入的参数都会保存在 arguments 对象中，可以通过下标访问。



一个函数，可以使用这个函数之外定义的变量，那么这个函数就称为一个闭包。可以获取函数自身以外的变量的函数就是一个闭包。





ECMA 中定义对象是无特定顺序的值的数组。



JavaScript 有三种创建对象的方法，一种是工厂方法，将创建对象的代码放到一个工厂函数中，需要自己 new 对象；第二种是构造函数方法，类似于工厂方法，但是不需要自己创建对象，而是默认创建，并用 this 引用；第三中是原型方法，先定义一个空构造方法 Constructor ，然后用Constructor.prototype.property 的方式来为原型添加属性。但是原型方法只能创建没有参数的对象，且所有的内容都是共享的，会产生修改访问冲突。解决的办法是混合原型和构造函数两种方法，用构造函数来定义属性，而用原型来定义方法。

还有另外一种动态原型方法，如下所示：

```javascript
function Car() {
  var oTempCar = new Object;
  oTempCar.color = "blue";
  oTempCar.doors = 4;
  oTempCar.mpg = 25;
  oTempCar.showColor = function() {
    alert(this.color);
  };

  return oTempCar;
}

```

建议使用这两种流行的混合模式。不建议使用传统的定义方法（自己 new 对象）。



#### 总结：创建对象的方法

- 工厂方法

  缺点：方法不共用。（将方法放在外面定义则导致形式上的分离。）

  ```javascript
  function createCar(sColor,iDoors,iMpg) {
    var oTempCar = new Object;
    oTempCar.color = sColor;
    oTempCar.doors = iDoors;
    oTempCar.mpg = iMpg;
    oTempCar.showColor = function() {
      alert(this.color);
    };
    return oTempCar;
  }

  var oCar1 = createCar("red",4,23);
  var oCar2 = createCar("blue",3,25);

  oCar1.showColor();		//输出 "red"
  oCar2.showColor();		//输出 "blue"
  ```

- 构造函数方法

  类似工厂方法（注意细节区别），只是可以用 new 对象的形式，同样存在方法不共用的问题。

  ```javascript
  function Car(sColor,iDoors,iMpg) {
    this.color = sColor;
    this.doors = iDoors;
    this.mpg = iMpg;
    this.showColor = function() {
      alert(this.color);
    };
  }

  var oCar1 = new Car("red",4,23);
  var oCar2 = new Car("blue",3,25);
  ```

- 混合工厂方法

  与工厂方法存在相同的劣势，这只是一个伪造成构造方法的工厂方法，除非万不得已应避免使用。

  ```javascript
  function Car() {
    var oTempCar = new Object;
    oTempCar.color = "blue";
    oTempCar.doors = 4;
    oTempCar.mpg = 25;
    oTempCar.showColor = function() {
      alert(this.color);
    };

    return oTempCar;
  }

  var car = new Car();
  ```

- 原型方法

  缺点：属性也变成共用的了，且创建对象时不能带有参数。

  ```javascript
  function Car() {
  }

  Car.prototype.color = "blue";
  Car.prototype.doors = 4;
  Car.prototype.mpg = 25;
  Car.prototype.showColor = function() {
    alert(this.color);
  };

  var oCar1 = new Car();
  var oCar2 = new Car();
  ```

- 混合方式（最常用）

  特点：用构造函数方法来创建属性，用原型方法来创建共用方法函数。

  ```javascript
  function Car(sColor,iDoors,iMpg) {
    this.color = sColor;
    this.doors = iDoors;
    this.mpg = iMpg;
    this.drivers = new Array("Mike","John");
  }

  Car.prototype.showColor = function() {
    alert(this.color);
  };

  var oCar1 = new Car("red",4,23);
  var oCar2 = new Car("blue",3,25);

  oCar1.drivers.push("Bill");

  alert(oCar1.drivers);	//输出 "Mike,John,Bill"
  alert(oCar2.drivers);	//输出 "Mike,John"
  ```

- 动态原型方法

  形式上将原型定义方法放到了构造函数里面，这样更符合封装性。

  ```javascript
  function Car(sColor,iDoors,iMpg) {
    this.color = sColor;
    this.doors = iDoors;
    this.mpg = iMpg;
    this.drivers = new Array("Mike","John");
    
    if (typeof Car._initialized == "undefined") {
      Car.prototype.showColor = function() {
        alert(this.color);
      };
  	
      Car._initialized = true;
    }
  }
  ```



#### 继承

- 对象冒充：可以实现多继承

  ```javascript
  function ClassA(sColor) {
      this.color = sColor;
      this.sayColor = function () {
          alert(this.color);
      };
  }

  function ClassB(sColor, sName) {
      this.newMethod = ClassA;
      this.newMethod(sColor);
      delete this.newMethod;
      // ClassA.call(this, sColor);              //方法1
      // ClassA.apply(this, new Array(sColor));  //方法2

      this.name = sName;
      this.sayName = function () {
          alert(this.name);
      };
  }
  ```


- 原型链：只有单继承

  ```javascript
  function ClassA() {
  }

  ClassA.prototype.color = "blue";
  ClassA.prototype.sayColor = function () {
      alert(this.color);
  };

  function ClassB() {
  }

  ClassB.prototype = new ClassA();
  ClassB.prototype.name = "";
  ClassB.prototype.sayName = function () {
      alert(this.name);
  };

  ```

- 混合模式（建议）

  ```javascript
  function ClassA(sColor) {
      this.color = sColor;
  }

  ClassA.prototype.sayColor = function () {
      alert(this.color);
  };

  function ClassB(sColor, sName) {
      ClassA.call(this, sColor);
      this.name = sName;
  }

  ClassB.prototype = new ClassA();

  ClassB.prototype.sayName = function () {
      alert(this.name);
  };
  ```

