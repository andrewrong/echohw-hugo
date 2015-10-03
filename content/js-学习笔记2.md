title: js-学习笔记2
date: 2014-07-13 10:34:55
categories: 学习
tags: javascript
---

> 在日常生活中，我们说话的时候基本上是动词和名词都是一起的，而在编程世界内，名词就是object的属性，而动词就是object的函数

觉得这样的类比还是有点意思的；

在javascript中，我们可以认为object其实就一个key-value的集合体，只不过key不能是数字1，2，3，它们是string或者是变量;个人突然觉得js中的object很像php的关联数组orpython的字典;

1. 定义一个object
	
	```
	var object = {name:'xxx',age:5}
	```
	
2. 两种方式创建一个object(上面是一种，或者使用构造函数)

	```
	 var me = new Object();//o要大写
	```

3. for in(用来遍历数据中的数据和对象中的属性值)

	```
	for(var i in [object | array])
	```
	i在object的时候是属性(key)
	
4. js到目前为止我还只是学习了object，对象，而没有类

5. 访问object的属性的方式有两种:

	```
	obj.name;
	obj['name'];//要加引号哦
	```
	
6. this关键字，这个关键字竟然可以直接用来普通的function中，如果被设置为object的method的话，它会自动指向这个object;用来匿名函数的时候比较多;

7. js的有构造函数，构造函数与object的对象名是一样的,好像构造函数本来就应该一样；js大概是没有类这样的东西，所以定义构造函数就像定义普通函数一样.**感觉可能js中的object类似于类这样的概念，因为有统一的构造函数**，之前的object有构造函数，只是这个构造函数基本没有做什么显性的东西，所以就导致它看上去没有用，其实使用object就表示你的对象都是来源于object的;

	```
	
	function Person(name,age) {
  	this.name = name;
  	this.age = age;
	}

	// Let's make bob and susan again, using our constructor
	var bob = new Person("Bob Smith", 30);
	var susan = new Person("Susan Jordan", 25);
	
	```
	
8. 可以在构造函数中初始化一个值，这个值不是经过参数传入初始化的，那么就表示所有的这个object有这么一个常数，其实有点象C++的类常量，可以把object当做类，虽然他不是；

9. 在js中，函数其实就是一个对象，函数对象，类似于C++中的函数对象；

10. js中无处不对象;

11. js中给对象指定一个函数的方式

	```
	this.xxx = function(){}
	or
	xxx:function(){}
	```
	
	两种表示法;
	
12. 可以通过`typeof xxx` 来知道一个变量的数据类型;
13. 可以通过`myObj.hasOwnProperty('name')`来判断对象是否有name属性
14. 一个类，一个对象可以添加、修改类中的属性和方法;
15. 通常我们只是给对象添加属性和方法，如果我们想给class添加方法之类的，语法为:

	```
	className.prototype.newMethod =function() {statements;};
	```
	
	prototype:关键词,js的面向对象是通过原型的方法来进行的；当在当前的object中没有找到的时候会去找class中的方法；
	
16. 关于js的继承；因为js是通过原型来实现的，所以对于js的继承就是设置这个类的prototype = new base();

	```
		// the original Animal class and sayName method
		function Animal(name, numLegs) {
    			this.name = name;
    			this.numLegs = numLegs;
		}
		Animal.prototype.sayName = function() {
    			console.log("Hi my name is " + this.name);
		};

		// define a Penguin class
		function Penguin(name){    
    			this.name = name;
    			this.numLegs = 2;
		}
		Penguin.prototype = new Animal();

		// set its prototype to be a new instance of Animal
		var a = new Penguin('fenglin');
	```
	
17. 奇葩的private权限

	```
	function Person(first,last,age) {
    		this.firstname = first;
    		this.lastname = last;
    		this.age = age;
    		var bankBalance = 7500;
    		var returnBalance = function() {
      			return bankBalance;
   		};
     }
	```
	
18. 在成员函数中，**访问成员变量不用加上this**
19. xxx.prototype用来知道某一个类的原因
