title: python快速学习
date: 2015-01-12 21:25:14
categories: learning
tags: python
---

1. python变量之间的赋值是引用的；不会影响到本质的东西;
2. **在计算机内存中，统一使用Unicode编码，当需要保存到硬盘或者需要传输的时候，就转换为UTF-8编码。**
3. Ascii、UTF-8，unicode;

1. list:和数组类似（有序列表）
2. tuple:定义一个数字的tuple要`(1,)`
3. dict:php中的关联数组；如果key不存在，那么dict会报错，两种方式来进行检测:

	1. key in dict
	2. dict.get(key,definevalue)
4. **dict的key必须是不可变对象**，整数、字符串是不可变的，但是list是变的，所以不能作为key
5. set: add,remove 





1. raw_input:输入的永远是字符串,可以通过`int()`来进行转化;

2. 可变对象和不可变的对象：一个是对象的内存可以被变化，而不可变的对象不管如何对象的内存是不会被改变的;



#### 1.循环

	for x in name:（list、tuple）
	while n > x:
	
	dict的循环:
	
	for key in dict:
	for value in dict.itervalues()
	for key,value in dict.iteritems()
	
##### 可循环的判断

	当我们使用for循环时，只要作用于一个可迭代对象，for循环就可以正常运行，而我们不太关心该对象究竟是list还是其他数据类型。
	如何判断一个对象是可迭代对象呢？方法是通过collections模块的Iterable类型判断：
	
```
>>> from collections import Iterable
>>> isinstance('abc', Iterable) # str是否可迭代
True
>>> isinstance([1,2,3], Iterable) # list是否可迭代
True
>>> isinstance(123, Iterable) # 整数是否可迭代
False
```

	Python内置的enumerate函数可以把一个list变成索引-元素对，这样就可以在for循环中同时迭代索引和元素本身：
	
```
>>> for i, value in enumerate(['A', 'B', 'C']):
...     print i, value
...
0 A
1 B
2 C
```

##### 列表生成式

	[x * x for x in range(1,11)]
	写列表生成式时，把要生成的元素x * x放到前面，后面跟for循环，就可以把list创建出来
	
	[x * x for x in range(1, 11) if x % 2 == 0] 
	
	==
	
	for x in range(1,11):
		if x % 2 == 0:
			x * x

##### generator(不用一下子把所有的数据都保存下来，可以通过推算进行得到的)

	列表生成式保存的是具体的数据;
	generator保存的是具体的推算的算法;(generator是一个可循环的对象，通过循环就可以进行推演)
	
	只要把列表生成式的[]变成()，那么返回的就是一个生成器;
	
	
#### 2. 类型转化

	int(),float(),bool(),str(),unicode()
	
#### 3. function

	1. pass:是一个占位符
	2. isinstance(x,(int,float)):表示x是不是这个类型的实例
	3. **Python的函数返回多值其实就是返回一个tuple**
	4. **定义默认参数要牢记一点：默认参数必须指向不变对象;如果指向的是可变的对象，那么默认参数会累积变化的**
	5. 可变参数:python是使用*来表示参数可变，加入*就可以让可变参数变成一个tuple;
	6. 可变参数允许你传入0个或任意个参数，这些可变参数在函数调用时自动组装为一个tuple。而关键字参数允许你传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict;
	
```
def person(name, age, **kw):
    print 'name:', name, 'age:', age, 'other:', kw
```

##### 3.1 装饰器(这个还是比较重要的，类似于spring的切片编程)

	@log
	
	def pr():
		print "hello"
		
	def log(func):
		def wrapper(*args,**dic):
			print '....'
			return pr(*args,**dic)
			
		return wrapper
	
##### 3.2 偏函数

	import functools
	functools.partial
	
	可以用来固定参数;
	
#### 4. module
	
	1. 每一个文件都是一个py模块;
	2. 如果在文件夹中定义了__init__.py表示文件夹是一个包;
	3. 一定不错的导入模式
	
```
	try:
		import json
	except ImportError:
		import simplejson as json 
```

	import xxx as xxx:别名
	
##### 4.1 python 搜索模块的路径

	1. 当前目录
	2. sys.path所指定的目录都是python可能会寻找的地方，这是python默认查找模块的路径信息;<dir()可以知道当前的作用域导入了哪些模块符号>
	
##### 4.2 添加第三方模块

	1. 手动的给sys.path这个list加入路径，不过运行完毕以后会消失
	2. 设置PYTHONPATH环境变量，与PATH的设置一样;

##### 4.3 比较新的模块

	使用__future__来表示比较新的模块，这样可以在python2.x使用python3.x的功能，又不能为了兼容性苦恼;
	
#### 5. 作用域

	1. 模块内部使用:以 _ or __开始,类似于 private;**不应该** 被直接引用,只是最好这么做，而从python角度是没有机制可以限制的;(模块内部)
	2. __xxx__:这是特殊的变量，可以被直接引用，但是通常是有特殊的作用的;
	
	
#### 6. python的代码习惯

	1. 类名通常是大写开始

#### 7. OOP

> 类是创建实例的模板，而实例则是一个一个具体的对象，各个实例拥有的数据都互相独立，互不影响

	1. private:
	
		使用__xx表示私有;有一定的机制可以保护内部变量;
		
		测试:_xxxx:可以被外部访问，但是如果写成这样就最好不要访问,__xxxx:有机制可以阻止访问;
	
	2. 观察对象类型
	
		2.1 判断一个var是否是某一个类型:isinstance(var,type)：
		2.2 是否type可以返回这个var的类型:
			type('123') 返回一个str
			
			type返回的类型，这些类型可以在types模块中找到;有一种类型是types.TypeType:万能类型，都可以匹配
		
		2.3 使用dir(var):可以返回var对象的所有属性和方法
		2.4 可以操作对象的属性：
		
			hasattr(obj,attr):obj是否有attr属性
			setattr(obj,attr,value):将obj的attr属性附上value值
			getattr(obj,attr):获得attr属性的值
			
	3. 动态加载属性
	
		3.1 可以动态加载属性和方法(types.MethodType，但是外加的不能访问private变量)
		3.2 __slots__:可以限制动态加属性,只对当前类有效，对子类无效
		3.3 使用@property，类似于装饰器;
		
	4. 定制类(通过改写类中的某一些特殊的方法就可以让你的类支持很多的操作)
	
		4.1  __slots__
		4.2 __len__()
		4.3 __str__():让你的类可以转化为你想要的字符串
		4.4 __repr__():功能和__str__差不多，区别是不用print它也能打出你规定的东西；可以在类中__repr__ = __str__，这样也是可以的;
		4.5 __iter__():要想你的类是可迭代的，那你就重写这个函数用来支持迭代;实现这个的同时还必须实现next函数，原因是__iter__是返回可迭代的对象，而next是在迭代过程中不断会被调用的;通过StopIteration来停止迭代;

```
def __iter__(self):
        return self
    def next(self):
        if len(self.__name) <= self.length:
            self.length = 0
            raise StopIteration()
        self.length = self.length + 1
        return self.__name[self.length - 1]
```

		4.6 __getitem__():可以实现list中的指定index来取的数据;与之类似的有__setitem__()和__delitem__()
		4.7 __getattr__(self,'score'):当然访问一个不存在的属性的时候，理论上python会提示一个错误，说找不到；其实这个过程是这样的，python先去完美匹配，如果发现没有，就找__getattr__()这个函数，最后如果没有的找到的话就返回错误;(这个很有用哦，动态的属性)
		4.8 __call__():可以实现，比如student实例，可以用student()来调用函数；可以callable()来判断当前的变量是否可调用；
		
	5. meta class
		
		5.1 定义类的本质:python解释器扫描定义的代码，然后用type这个函数来创建这个类;所以type既可以返回类型也可以创建类
		 5.2 metaclass ---> class --->instance（很牛逼的功能，ORM）
		 
		 	metaclass：创建类取决于这个
		 
		 5.3 如果不是在init中定义的属性，放在最外面的属性就类属性
		 
#### 8. 错误处理

	1. python是用
	
		try....except .... finally
		通过raise来把异常抛出给调用方，让调用方来处理这个问题;抛出会打断正常逻辑，但是finally还是继续执行的;
	
	2. 调试的方法
	
		1. print
		2. assert
		3. logging
		4. pdb，单步调试
		5. pycharm支持单步的ide
		
#### 9. 文件处理

##### 1. 基本知识

	file.read():会一次性把所有的内容全部读入内存
	file.read(size):每次最多读入size个字节
	readline:每次读入一行
	readlines：把所有行读入，以list返回
	write(xxxx):写入文件，写入也可以使用with
	
	with open('xxx','r') as f：
		print f.read()
		
	等价于
	
	try:
	
		f = open('xxx','r')
		f.read
	finally：
		if f:
			f.close()
			
##### 2. 目录和文件

	1. import os 与 os.path
	2. 与操作系统有关的东西：
	
		os.uname
		os.name
		os.environ:所有的环境变量
		os.getenv('PATH'):获得相应环境变量;
		
	3. os.path.abspath:获得绝对路径
	4. os.path.join:将两个路径连接起来
	5. os.path.split:将路径分离,后面一个是最后的文件或者目录
	6. os.mkdir():创建目录
	7. os.rmdir:
	8. os.path.splitext:分离扩展名
	9. os.rename 
	10. os.remove
	11. os.path.isfile / os.path.isdir
	
##### 3. 序列化

> 把变量从内存中变成可存储或传输的过程称之为序列化,pickling、unpickling

	1. 两个模块
	
		cPickle、pickle