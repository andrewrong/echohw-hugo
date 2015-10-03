title: go学习-1
date: 2015-01-29 22:18:46
categories: langauge
tags: go
---

#### 1. 基本语法知识

##### 1.1 变量初始化

	var a int = 1
	var a = 1
	a := 1 //这种方式不能用来定义全局变量;
	
##### 1.2 常量初始化

	const a = 1
	const a int = 1
	
	//由于常量的赋值是一个编译期行为，所以不能用运行时才能得到的数据去初始化常量
	
	iota:一个可变的常量参数,每一次遇到const都会清零;
	
	const {
		a = iota // 0
		b		// 1
		c		// 2
	}
	
##### 1.3 go的数据类型

1.3.1 值类型

	bool(go是一种强类型的语言，类型之间是不允许相互赋值，但是大部分可以通过强制转化来进行赋值，但是bool是不能通过强制类型转化进行赋值的)
	
	整型: int8,uint8(byte),int,uint,int64,uint64
	float32,float64:（类型自动推导出来的float都是float64）
	string:
	rune:字符类型，通常是32位;
	error:一个interface，用来做错误信息返回;
	数组: a := [2]int{1,2}, b := [...]{1,2}
	struct
	interface:go中的万能类型
	pointer:指针类型
	
	
	
1.3.2 引用类型

	slice:切片，类似于C++中的vector，底层还是有array实现的，不过和vector一样维护了len和cap两个变量;cap()和len()函数		
	map:语言原生支持;map[string]int
	channel:通道，go中用来协程通信
	
##### 1.4 流程控制

###### 1.4.1 条件

	if...else...
	
注意一点:不允许最终的return放在if/else之内;

##### 1.4.2 选择语句

	switch..case
	
go的switch语句比较强大

1. 默认每一个case都自带break
2. case 的条件是可以类似于if的条件一样，不用一定要是常量
3. fallthrough：可以让switch中的case和C语言的一样;

##### 1.4.3 循环

	for 
	//不同语言在这个循环这边都做了各种方式的简化
	for k,v := range xxx{
	}
	
#### 1.5 函数

##### 1.5.1 不定参数

	1. func max(args ...int){
	}
	args就是一个不定参数，可以往里面加入任意个int的参数,如果你想往里面传入slice的话就如下调用:
	a := []int{1,2,3}
	max(a...) //...表示将slice的打开并且传入
	
	2. 如果想不限定参数类型传入的话:
	
	func myfunc(args ...interface{}){
	}
	
##### 1.5.2 匿名函数和闭包

匿名函数就是指没有明确的函数名，比如
	
	xx := func(a,b int) int {
		if a > b {
			return a
		} else {
			return b
		}
	}
	
这里的func就是一个匿名函数，并且赋值给xx，匿名函数可以在定义的时候直接被调用，只要在后面加上小括号和必要的参数就可以;

##### 1.6 错误处理

1. error 类型

		type error interface {
			Error() string
		}	
		
2. defer函数

defer函数：在函数中使用，可以保证函数返回的时候绝对会被调用的，用来对一些资源的释放很好；好处在于不管是正常返回还是异常返回都是被调用的；

**多个defer函数按照FILO的形式进行调用**

3.
	


	