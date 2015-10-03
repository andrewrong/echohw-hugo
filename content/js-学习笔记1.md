title: 'js 学习笔记1'
date: 2014-07-13 10:21:54
categories: 学习
tags: javascript
---

1. 在html中地方和存放的问题:
```
	<script type="text/javascript">
	</script>
```
Javascript是一门脚本语言，只有运行到了才会被执行；通常js的代码放在html的`<head>` 和 `<body>`两个地方;

2. js的代码的注释和c基本一样
3. js的变量是先声明后使用,方式为`var hello`,定义变量hello
4. `<input>`中的button类型，要定义onclick的行为，并给与js的函数，点击这个按钮就会触发调用js的函数;
5. 与C++不一样，像js这种语言的标准输出的地方不是终端而是网页，所以对js的标准输出和标准输入的方法为document.write("xxxx"),这样就可以输出到网页了;
6. 字符串连接操作符:`+`
7. alert出现一个警告弹窗,只能输出字符串和变量
8. confirm("xxx"):这个是确认对方框，会根据你的点击对方框的按钮来执行接下来的事情;我觉得这个比较适合的场景是当你要删除一个东西的时候会询问你是否真的删除;
9. prompt:和上面的输入输出一样，可以认为这是js的标准输入吧，当然js的标准输入来源于网页
10. window.open打开新窗口
11. window.close关闭窗口

### 关于DOM(document object model)

关于dom这个对象，是操作html节点的对象，总的来说它是一个树的根节点，通过这个节点我们去选择需要的节点进行操作；下面是他的节点图

![节点图](http://frgeek-bed.qiniudn.com/other_js%E8%8A%82%E7%82%B9%E5%9B%BE.jpg)

节点图的节点可以分为三种:

- 元素节点:`<html>、<p>`之类的;
- 文本节点:用来显示出来的内容,通常指的是内容本身;
- 属性节点:比如`<a>`标签内部的href属性;
***
1. getElementById("id"):通过Id去获取某一个节点:id是每一个html元素的唯一标示符，类似于人类的身份证一样的东西;
2. innerHTML:如果上面的get函数是用来获得某一个节点的对象的话，那么这个就是上面这个对象的一个属性，用来返回这个节点内容;
3. 修改某一个节点的样式：其实就是修改这个节点的属性节点style,加入一些color、font-size之类的
4. display:通过这个属性可以控制某一个节点的显示和隐藏;none:隐藏,block:显示

关于样式的基本属性:

	color
	fontSize
	backgroundColor
	width
	height
	font
	fontFamily

