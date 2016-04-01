<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Design Patterns Notes](#design-patterns-notes)
	- [Modular Patterns](#modular-patterns)
		- [Object Literal](#object-literal)
		- [立即函数模式](#立即函数模式)
		- [UMD(Universal Module Definition) Pattern](#umduniversal-module-definition-pattern)
	- [Common Design Patterns](#common-design-patterns)
		- [Classification](#classification)
			- [创建者模式](#创建者模式)
			- [结构设计模式](#结构设计模式)
			- [行为设计模式](#行为设计模式)
		- [Class Pattern](#class-pattern)
		- [Mix-In Pattern](#mix-in-pattern)
		- [Singleton Pattern](#singleton-pattern)
		- [Abstract Factory](#abstract-factory)
		- [Factory Method](#factory-method)
		- [Decorator Pattern](#decorator-pattern)
			- [实现1(关键 - 实现传递方式)](#实现1关键-实现传递方式)
			- [return this.uber.function()](#return-thisuberfunction)
			- [Decorators List](#decorators-list)
			- [实现2](#实现2)
		- [Facade Pattern](#facade-pattern)
		- [Flyweight Pattern](#flyweight-pattern)
		- [Command Pattern](#command-pattern)
		- [Mediator Pattern](#mediator-pattern)
		- [Observer/Pub-Sub Pattern](#observerpub-sub-pattern)
			- [Observer](#observer)
			- [Pub/Sub](#pubsub)
				- [Implementation](#implementation)
				- [Sample](#sample)
					- [Ajax Callback](#ajax-callback)

<!-- /TOC -->

# Design Patterns Notes

[Awesome Book](http://www.dofactory.com/javascript/design-patterns)

## Modular Patterns

### Object Literal

通过对象字面量创建命名空间

```javascript
MYAPP.namespace = function (namespaceString) {
	var parts = namespaceString.split('.'),
		parent = MYAPP,
		i;
	// strip redundant leading global
	if (parts[0] === "MYAPP") {
		// remove leading global
		parts = parts.slice(1);
	}
	for (i = 0; i < parts.length; i += 1) {
		// create a property if it doesn't exist
		if (typeof parent[parts[i]] === "undefined") {
			parent[parts[i]] = {};
		}
		//关键: 向内嵌套
		parent = parent[parts[i]];
	}
	// 返回最内层模块
	return parent;
};
```

```javascript
// assign returned value to a local var
var module2 = MYAPP.namespace('MYAPP.modules.module2');
module2 === MYAPP.modules.module2; // true
// skip initial `MYAPP`
MYAPP.namespace('modules.module51');
// long namespace
MYAPP.namespace('once.upon.a.time.there.was.this.long.nested.property');
```

### 立即函数模式

通过调用立即函数，返回一个对象，暴露(exposed to public)公共接口(特权/公共方法):

-   闭包: 定义私有变量与特权方法
-   返回对象: 即使通过外部代码改变返回对象的接口，也不会影响原接口

```js
var myobj = (function () {
		// private member
	var name = "tazimi",

		// private method
		// excluded in return object

    	// privileged method
    	function getName() {
    		return name;
    	},
		function setName(n) {
			if (typeof n === 'string') {
			    name = n;
			}
			return this;
		},

		// public method
		function logName() {
			console.log(name);
		};

    // 闭包
    return {
		// 公共接口: 特权/公共方法

        // 特权方法
    	getName: getName,
		setName: setName,

		// 公共方法
		log: logName;
    };
}());
```

### [UMD(Universal Module Definition) Pattern](https://github.com/https://github.com/umdjs/umd/tree/master/templates)

-   先判断是否支持 Node.js 的模块(exports)，存在则使用 Node.js 模块模式
-   再判断是否支持 AMD(define)，存在则使用 AMD 方式加载模块

```js
(function (window, factory) {
    if (typeof exports === 'object') {
        module.exports = factory();
    } else if (typeof define === 'function' && define.amd) {
        define(factory);
    } else {
        window.eventUtil = factory();
    }
})(this, function () {
    //module ...
});
```

## Common Design Patterns

### Classification

|Creational|Class|
|:---------------:|:-------------------------------------------------------------:|
|Factory Method(工厂方法)|通过将数据和事件接口化来构建若干个子类。|
|Abstract Factory(抽象工厂)|建立若干族类的一个实例，这个实例不需要具体类的细节信息。（抽象类）|
|Builder(建造者)|将对象的构建方法和其表现形式分离开来，总是构建相同类型的对象。|
|Prototype(原型)|一个完全初始化的实例，用于拷贝或者克隆。|
|Singleton(单例)|一个类只有唯一的一个实例，这个实例在整个程序中有一个全局的访问点。|


|Structural|Class|
|:---------------:|:-----------------------------------------------------:|
|Adapter(适配器)|将不同类的接口进行匹配，调整，这样尽管内部接口不兼容但是不同的类还是可以协同工作的。|
|Bridge(桥接模式)|将对象的接口从其实现中分离出来，这样对象的实现和接口可以独立的变化。|
|Composite(组合模式)|通过将简单可组合的对象组合起来，构成一个完整的对象，这个对象的能力将会超过这些组成部分的能力的总和，即会有新的能力产生。|
|Decorator(装饰器)|动态给对象增加一些可替换的处理流程。|
|Facade(外观模式)|一个类隐藏了内部子系统的复杂度，只暴露出一些简单的接口。|
|Flyweight(享元模式)|一个细粒度对象，用于将包含在其它地方的信息 在不同对象之间高效地共享。|
|Proxy(代理模式)|一个充当占位符的对象用来代表一个真实的对象。|

|Behavioral|Class|
|:---------------:|:-----------------------------------------------------:|
|Interpreter(解释器)|将语言元素包含在一个应用中的一种方式，用于匹配目标语言的语法。|
|Template Method(模板方法)|在一个方法中为某个算法建立一层外壳，将算法的具体步骤交付给子类去做。|
|Chain of Responsibility(响应链)|一种将请求在一串对象中传递的方式，寻找可以处理这个请求的对象。|
|Command(命令)|封装命令请求为一个对象，从而使记录日志，队列缓存请求，未处理请求进行错误处理 这些功能称为可能。|
|Iterator(迭代器)|在不需要直到集合内部工作原理的情况下，顺序访问一个集合里面的元素。|
|Mediator(中介者模式)|在类之间定义简化的通信方式，用于避免类之间显式的持有彼此的引用。|
|Observer(观察者模式)|用于将变化通知给多个类的方式，可以保证类之间的一致性。|
|State(状态)|当对象状态改变时，改变对象的行为。|
|Strategy(策略)|将算法封装到类中，将选择和实现分离开来。|
|Visitor(访问者)|为类增加新的操作而不改变类本身。|

#### 创建者模式

-   构造器模式(Constructor)
-   工厂模式(Factory)
-   抽象工厂模式(Abstract)
-   原型模式(Prototype)
-   单例模式(Singleton)
-   建造者模式(Builder)

#### 结构设计模式

-   装饰模式
-   外观模式
-   享元模式
-   适配器模式
-   代理模式

#### 行为设计模式

-   迭代模式
-   中介者模式
-   观察者模式
-   访问者模式

### Class Pattern

```js
var Person =  function( firstName , lastName ){
  this.firstName = firstName;
  this.lastName =  lastName;
  this.gender = "male";
};

// Define a subclass constructor for for "Superhero":
var Superhero = function( firstName, lastName , powers ){
    // Invoke the superclass constructor on the new object
    // then use .call() to invoke the constructor as a method of
    // the object to be initialized.
    Person.call( this, firstName, lastName );

    // Finally, store their powers, a new array of traits not found in a normal "Person"
    this.powers = powers;
};
SuperHero.prototype = Object.create( Person.prototype );
```

```js
var superman = new Superhero( "Clark" ,"Kent" , ["flight","heat-vision"] );
console.log( superman );
```

### Mix-In Pattern

将多个对象的属性混入同一个对象,达到继承/扩展/组合的效果

-   不改变原型链

```js
function mix() {
	var arg, prop, child = {};

	for (arg = 0; arg < arguments.length; arg += 1) {
		for (prop in arguments[arg]) {
			if (arguments[arg].hasOwnProperty(prop)) {
				child[prop] = arguments[arg][prop];
			}
		}
	}

	return child;
}
```

```js
var cake = mix(
	{eggs: 2, large: true},
	{butter: 1, salted: true},
	{flour: "3 cups"},
	{sugar: "sure!"}
);
```

-   改变原型链

```js
// Extend an existing object with a method from another
function mix( receivingClass, givingClass ) {

    // mix-in provide certain methods
    if ( arguments[2] ) {
        for ( var i = 2, len = arguments.length; i < len; i++ ) {
            receivingClass.prototype[arguments[i]] = givingClass.prototype[arguments[i]];
        }
    }
    // mix-in provide obj
    else {
        for ( var methodName in givingClass.prototype ) {
            if ( !receivingClass.prototype[methodName] ) {
               receivingClass.prototype[methodName] = givingClass.prototype[methodName];
            }
        }
    }
}
```

### Singleton Pattern

原型与构造函数指针运作正常

```javascript
function Universe() {
	// 缓存实例
	var instance;

	// anti-Self-Defined Function Pattern
	// 反-自定义函数模式: 先重写,再初始化
	Universe = function Universe() {
		return instance;
	};

	// 保存原型,使其一直保持于同一位置
	// (this指针指向不重要)
	Universe.prototype = this;

	instance = new Universe();
	// 重定向constructor指针
	instance.constructor = Universe;
	// 功能代码
	instance.start_time = 0;
	instance.bang = "Big";

	return instance;
}
```

### Abstract Factory

```js
var AbstractVehicleFactory = (function () {
    // Storage for our vehicle types
    var types = {};

    function _getVehicle( type, customizations ) {
		var Vehicle = types[type];
		return (Vehicle ? new Vehicle(customizations) : null);
	}
	function _registerVehicle( type, Vehicle ) {
		var proto = Vehicle.prototype;

		// only register classes that fulfill the vehicle contract
		if ( proto.drive && proto.breakDown ) {
			types[type] = Vehicle;
		}

		return AbstractVehicleFactory;
	}

    return {
		getVehicle: _getVehicle,
		registerVehicle: _registerVehicle
    };
})();
```

### Factory Method

```js
module.exports = (function () {
	function VehicleFactory() {
		var publicVehicle = new Object();

	    // specific factory
	    function Car( options ) {
		  this.type = 'car';
	      this.doors = options.doors || 4;
	      this.state = options.state || "brand new";
	      this.color = options.color || "silver";
	      this.speed = options.speed || 10;
	    }
	    function Truck( options){
		  this.type = 'truck';
	      this.state = options.state || "used";
	      this.wheelSize = options.wheelSize || "large";
	      this.color = options.color || "blue";
	      this.speed = options.speed || 8;
	    }

		// public features of vehicle , added to __proto__
		function _run() {
			var args = [].slice.call(arguments);

			if (args.length === 0) {
				console.log(this.type + ' - run with: ' + this.speed + 'km/s');
			} else if (toString.apply(args[0]) === '[object Number]') {
				this.speed = args[0];
			}
		}
		function _withColor() {
			var args = [].slice.call(arguments);

			if (args.length === 0) {
				console.log('The color of this ' + this.type + ' product is : ' + this.color);
			} else if (toString.apply(args[0]) === '[object String]') {
				this.color = args[0];
			}
		}
		// provide a function to change other public features
		function _reform(funcName, newFunc) {
			if (typeof this[funcName] === 'function' || typeof this.prototype[funcName] === 'function') {
				delete this[funcName];
				this.prototype[funcName] = newFunc;
			}
		}
		// provide a function to add new public features
		function _addFeature(funcName, newFunc) {
			if (typeof this[funcName] === 'undefined') {
				this[funcName] = newFunc;
				this.prototype[funcName] = newFunc;
			}
		}

		// private features, added to obj

	    // core: create method
		this.create = function (options) {
			var vehicleClass = '',
				newVehicle = {};

			if (options.type === 'car') {
				vehicleClass = Car;
			} else {
				vehicleClass = Truck;
			}

			// create new vehicle with options, by pre-defined specific constructor
			newVehicle =  new vehicleClass(options);
			// set up prototype
			newVehicle.__proto__ = publicVehicle;
			newVehicle.prototype = publicVehicle;

			// add public feature
			newVehicle.prototype.run = _run;
			newVehicle.prototype.withColor = _withColor;
			newVehicle.prototype.reform = _reform;
			newVehicle.prototype.addFeature = _addFeature;

			// add private(seperately) feature

			// return new obj
			return newVehicle;
		};
	}

	// define more factory

	return {
		vehicleFactory: VehicleFactory
	};
}());
```

### Decorator Pattern

-   重写/重载/扩展对象原有的行为(method),但不改变对象原有属性
-   可以添加新属性，并围绕新属性扩展对象的原行为 e.g 原对象只会说中文，装饰后同时说中文与英文
-   避免了通过继承来为类型添加新的职责的形式可取，通过继承的方式容易造成子类的膨胀

关键: 将每次装饰后的结果向后传递,以达到叠加装饰效果

#### 实现1(关键 - 实现传递方式)

两种方式:

-   uber属性获得每次装饰后结果
-   循环叠加每次装饰后结果

#### return this.uber.function()

```javascript
// 构造函数
function Sale(price) {
	this.price = price || 100;
}
Sale.prototype.getPrice = function () {
	return this.price;
};

// 定义具体装饰器
// 通过uber属性获得上一次装饰后的结果
Sale.decorators = {};
Sale.decorators.fedtax = {
	getPrice: function () {
		var price = this.uber.getPrice();
		price += price * 5 / 100;
		return price;
	}
};
Sale.decorators.quebec = {
	getPrice: function () {
		var price = this.uber.getPrice();
		price += price * 7.5 / 100;
		return price;
	}
};
Sale.decorators.money = {
	getPrice: function () {
		return "$" + this.uber.getPrice().toFixed(2);
	}
};
Sale.decorators.cdn = {
	getPrice: function () {
		return "CDN$ " + this.uber.getPrice().toFixed(2);
	}
};

Sale.prototype.decorate = function (decorator) {
	var F = function () {},
		overrides = this.constructor.decorators[decorator],
		i,
		newobj;

	// 临时代理构造函数
	F.prototype = this;
	newobj = new F();
	// 传递实现的关键
	// 通过uber属性获得上一次装饰后的结果
	newobj.uber = F.prototype;

	for (i in overrides) {
		if (overrides.hasOwnProperty(i)) {
			newobj[i] = overrides[i];
		}
	}

	return newobj;
};
```

#### Decorators List

```javascript
// 构造函数
function Sale(price) {
	this.price = (price > 0) || 100;
	this.decorators_list = [];
}
Sale.prototype.getPrice = function () {
	return this.price;
};

// 定义具体装饰器
Sale.decorators = {};
Sale.decorators.fedtax = {
	getPrice: function (price) {
		return price + price * 5 / 100;
	}
};
Sale.decorators.quebec = {
	getPrice: function (price) {
		return price + price * 7.5 / 100;
	}
};
Sale.decorators.money = {
	getPrice: function (price) {
		return "$" + price.toFixed(2);
	}
};


Sale.prototype.decorate = function (decorator) {
	this.decorators_list.push(decorator);
};
Sale.prototype.getPrice = function () {
	var price = this.price,
		i,
		max = this.decorators_list.length,
		name;

	for (i = 0; i < max; i += 1) {
		name = this.decorators_list[i];
		// 传递实现的关键
		// 通过循环叠加上一次装饰后的结果
		price = Sale.decorators[name].getPrice(price);
	}

	return price;
};
```

#### 实现2

```js
// The constructor to decorate
function MacBook() {
  this.cost = function () { return 997; };
  this.screenSize = function () { return 11.6; };

}

// Decorator 1
function Memory( macbook ) {
  var v = macbook.cost();
  macbook.cost = function() {
    return v + 75;
  };

}

// Decorator 2
function Engraving( macbook ){
  var v = macbook.cost();
  macbook.cost = function(){
    return  v + 200;
  };

}

// Decorator 3
function Insurance( macbook ){
  var v = macbook.cost();
  macbook.cost = function(){
     return  v + 250;
  };

}
```

```js
var mb = new MacBook();
Memory( mb );
Engraving( mb );
Insurance( mb );

// Outputs: 1522
console.log( mb.cost() );

// Outputs: 11.6
console.log( mb.screenSize() );
```

### Facade Pattern

将多个复杂的子系统封装+合并，实现一个复杂功能，但只暴露一个简单的接口 - 封装复杂逻辑

```js
var sabertazimi = {};

sabertazimi.addMyEvent = function(el,ev,fn){
    if (el.addEventListener) {
        el.addEventListener(ev,fn, false);
    } else if (el.attachEvent) {
        el.attachEvent("on" + ev, fn);
    } else {
        el["on" + ev] = fn;
    }
};
```

### Flyweight Pattern

-   内在信息 - 对象中的内部方法所需信息/属性, 一个单独的享元可替代大量具有相同内在信息的对象
-   某个类型的对象有大量的实例，对这些实例进行分类，合并相同分类的对象，只创建少量实例(享元)
-   通过享元工厂来管理一组享元，当所需享元已存在时，返回已存在享元;当所需享元不存在时，创建新享元

```js
function Flyweight (make, model, processor) {
    this.make = make;
    this.model = model;
    this.processor = processor;
};

var FlyWeightFactory = (function () {
    var flyweights = {};

    return {
        get: function (make, model, processor) {
            // 不存在所需享元，新建新享元
            if (!flyweights[make + model]) {
                flyweights[make + model] =
                    new Flyweight(make, model, processor);
            }

            return flyweights[make + model];
        },

        getCount: function () {
            var count = 0;
            for (var f in flyweights) count++;
            return count;
        }
    }
})();

var Computer = function (make, model, processor, memory, tag) {
    this.flyweight = FlyWeightFactory.get(make, model, processor);
    this.memory = memory;
    this.tag = tag;
    this.getMake = function () {
        return this.flyweight.make;
    }
    // ...
}

function ComputerCollection () {
    var computers = {};
    var count = 0;

    return {
        add: function (make, model, processor, memory, tag) {
            computers[tag] =
                new Computer(make, model, processor, memory, tag);
            count++;
        },

        get: function (tag) {
            return computers[tag];
        },

        getCount: function () {
            return count;
        }
    };
}

(function () {
    var computers = new ComputerCollection();

    computers.add("Dell", "Studio XPS", "Intel", "5G", "Y755P");
    computers.add("Dell", "Studio XPS", "Intel", "6G", "X997T");
    computers.add("Dell", "Studio XPS", "Intel", "2G", "NT777");
    computers.add("Dell", "Studio XPS", "Intel", "2G", "0J88A");
    computers.add("HP", "Envy", "Intel", "4G", "CNU883701");
    computers.add("HP", "Envy", "Intel", "2G", "TXU003283");

    console.log("Computers: " + computers.getCount());
    console.log("Flyweights: " + FlyWeightFactory.getCount());
}())
```

### Command Pattern

-   将方法/动作封装成对象, 使得外部通过唯一方法 excute/run 调用内部方法/动作

```js
module.exports = (function () {
    var manager = {};

    // command to be encapsulted
    manager.isNull = function (nu) {
        return toString.apply(nu) === '[object Null]';
    };
    manager.isArray = function (arr) {
        return toString.apply(arr) === '[object Array]';
    };
    manager.isString = function (str) {
        return toString.apply(str) ==='[object String]';
    };

    // public api
    function execute(command) {
        return manager[command] && manager[command].apply(manager, [].slice.call(arguments, 1));
    }
    function run(command) {
        return manager[command] && manager[command].apply(manager, [].slice.call(arguments, 1));
    }

    return {
        execute: execute,
        run: run
    };
}());
```

### Mediator Pattern

中央集权的控制中心 - 所有观察者共享一个共有的被观察者(所有订阅者订阅同一个节点)

### Observer/Pub-Sub Pattern

被观察者(Subject)维护一组观察者列表，每当被观察者状态改变时，调用 notify 函数，此函数中调用观察者(Observer)的 update 函数(可自定义)

#### Observer

```js
function ObserverList(){
  this.observerList = [];
}

ObserverList.prototype.Add = function( obj ){
  return this.observerList.push( obj );
};

ObserverList.prototype.Empty = function(){
  this.observerList = [];
};

ObserverList.prototype.Count = function(){
  return this.observerList.length;
};


ObserverList.prototype.Get = function( index ){
  if( index > -1 && index < this.observerList.length ){
    return this.observerList[ index ];
  }
};

ObserverList.prototype.Insert = function( obj, index ){
  var pointer = -1;

  if( index === 0 ){
    this.observerList.unshift( obj );
    pointer = index;
  }else if( index === this.observerList.length ){
    this.observerList.push( obj );
    pointer = index;
  }

  return pointer;
};

ObserverList.prototype.IndexOf = function( obj, startIndex ){
  var i = startIndex, pointer = -1;

  while( i < this.observerList.length ){
    if( this.observerList[i] === obj ){
      pointer = i;
    }
    i++;
  }

  return pointer;
};

ObserverList.prototype.RemoveAt = function( index ){
  if( index === 0 ){
    this.observerList.shift();
  }else if( index === this.observerList.length -1 ){
    this.observerList.pop();
  }
};

//  被观察者维护一个观察者列表
function Subject(){
  this.observers = new ObserverList();
}

Subject.prototype.AddObserver = function( observer ){
  this.observers.Add( observer );
};

Subject.prototype.RemoveObserver = function( observer ){
  this.observers.RemoveAt( this.observers.IndexOf( observer, 0 ) );
};

Subject.prototype.Notify = function( context ){
  var observerCount = this.observers.Count();
  for(var i=0; i < observerCount; i++){
    this.observers.Get(i).Update( context );
  }
};

// The Observer
function Observer(){
  this.Update = function(){
    // ...
  };
}

// Extend an object with an extension
function extend( extension, obj ){
  for ( var key in extension ){
    obj[key] = extension[key];
  }
}

```

#### Pub/Sub

##### Implementation

-   pubsubz.js

```js
module.exports = (function ( window, doc, undef ) {

    var pubsubz ={};

    var topics = {},
        subUid = -1;

    pubsubz.publish = function ( topic, args ) {

        // undefined check
        if (!topics[topic]) {
            return false;
        }

        setTimeout(function () {
            var subscribers = topics[topic],
                len = subscribers ? subscribers.length : 0;

            while (len--) {
                subscribers[len].func(topic, args);
            }
        }, 0);

        return true;

    };

    pubsubz.subscribe = function ( topic, func ) {

        // undefined check
        if (!topics[topic]) {
            topics[topic] = [];
        }

		// add observer to observerlist(topics)
        var token = (++subUid).toString();
        topics[topic].push({
            token: token,
            func: func
        });
        return token;
    };

    pubsubz.unsubscribe = function ( token ) {
        for (var m in topics) {
            if (topics[m]) {
                for (var i = 0, j = topics[m].length; i < j; i++) {
                    if (topics[m][i].token === token) {
                        topics[m].splice(i, 1);
                        return token;
                    }
                }
            }
        }
        return false;
    };

    return pubsubz;
}( this, this.document, undefined ));
```

-   test.js

```js
var pubsub = require('./pubsubz.js');

// add observer to observerlist
var testFirstSub = pubsub.subscribe( 'login', function (topic , data ) {
        console.log( topic + ": " + data );
    });

// subject broadcast/notify, observer update
pubsub.publish( 'login', 'hello world!' );
pubsub.publish( 'login', ['test','a','b','c'] );
pubsub.publish( 'login', [{'color':'blue'},{'text':'hello'}] );

setTimeout(function(){
    pubsub.unsubscribe(testFirstSub);
}, 0);

// permanent subscribe
pubsub.subscribe('sum', function (topic, data) {
    if (toString.apply(data) !== '[object Array]') {
        console.log('Please input array: * ' + data + ' * is not array!');
    } else {
        var tmp = data.filter(function (item) {
                return toString.apply(item) === '[object Number]';
            });

        if (tmp.length) {
            var sum = tmp.reduce(function (previous, current) {
                return previous + current;
            }, 0);
            console.log('sumof ' + data + ' : ' + sum);
        } else {
            console.log('Please input number array: * ' + data + ' * is not number array!');
        }
    }

    return this;
});

pubsub.publish( 'login', 'hello again!' );
pubsub.publish('sum', 'hello again!');
pubsub.publish('sum', [1, 2, 3, 4, 5]);
pubsub.publish('sum', ['a', 'b', 'c', 'd', 'e']);
```

##### Sample

###### Ajax Callback

-   当请求返回，并且实际的数据可用的时候，会生成一个通知
-   如何使用这些事件（或者返回的数据），都是由订阅者自己决定的
-   可以有多个不同的订阅者，以不同的方式使用返回的数据
-   Ajax层: 唯一的责任 - 请求和返回数据，接着将数据发送给所有想要使用数据的地方

```js
(function ($) {

   // Pre-compile template and "cache" it using closure
   var resultTemplate = _.template($( "#resultTemplate" ).html());

   // Subscribe to the new search tags topic
   $.subscribe( "/search/tags" , function( tags ) {
       $( "#searchResults" )
                .html("


	Searched for:" + tags + "

");
   });

   // Subscribe to the new results topic
   $.subscribe( "/search/resultSet" , function( results ){

       $( "#searchResults" ).append(resultTemplate( results ));

   });

   // Submit a search query and publish tags on the /search/tags topic
   $( "#flickrSearch" ).submit( function( e ) {

       e.preventDefault();
       var tags = $(this).find( "#query").val();

       if ( !tags ){
        return;
       }

       $.publish( "/search/tags" , [ $.trim(tags) ]);

   });


   // Subscribe to new tags being published and perform
   // a search query using them. Once data has returned
   // publish this data for the rest of the application
   // to consume

   $.subscribe("/search/tags", function( tags ) {

	   // Ajax Request
       $.getJSON( "http://api.flickr.com/services/feeds/photos_public.gne?jsoncallback=?" ,{
              tags: tags,
              tagmode: "any",
              format: "json"
            },

          function( data ){

              if( !data.items.length ) {
                return;
              }

              $.publish( "/search/resultSet" , data.items  );
       });

   });
}());
```
