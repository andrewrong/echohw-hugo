title: html_css的学习记录
date: 2014-07-13 09:44:50
categories: 总结
tags: html,css
---
### 学习html标识

1. `<p>`:段落表示，在文章中表示段落，默认样式是段前段后空一行
2. `<em>`:斜体,`<strong>`:粗体;这两个标签的语义是强调
3. `<span>`:没有任何语义，仅仅只是为了单独加入样式;
4. `<q>`:语义是引用别人的话,默认样式是会自动在引用的句子外面加入双引号;
5. `<blockquote>`:与`<p>`的语义一样的，不同点是`<p>`是用来引用一句简短的话语，而`<blockquote>`是用来引用一句很长的话语.而且这个的样式也与`<p>`不一样的。其实这两个标签与markdown的`>`是一样的;
6. `<br>`:回车符,用来换行使用。在html中输入回车和空格都是无效的，html解析器会自动忽略这些字符，所以要换行就必须使用`<br>`。`<br>`是一个空标签，**空标签:就是没有内容的标签**,空标签就没有结束标签，只有单独一个,还有`<img>、<hr>`
7. 空格:在html中要表示输入一个空格就必须使用`&nbsp;`
8. `<hr>`：水平线,语义:分隔
9. `<address>`：用来表示地址，语义:地址
10. `<code>`：表示code，如果是多行要表示代码的话就不能使用
11. `<ul><li></li></ul>`:无序列表
12. `<ol><li></li></ol>`:有序列表
13. `<a></a>`:超链接标签，有属性`href`来表示链接的地址，如果想以新窗口打开的话,就这么写;`<a href="xx" target="_blank">`
14. `mailto`:发邮件,详细介绍看文档
15. `<img>`:`<img src="xxx" alt="xxx" title="xxxx">`:src:图片地址，alt:当图片显示不出来的时候会出现的文字，title:图片的描述，当鼠标进过的时候可以浮现出来

***
### form表单
#### 形式
```
<form method="post/get" action="xxx">
</form>
```

其中method:通常有两种方式：post和get两种;而action:表示接受这次请求的文件，如果是php就可能是某一个后台的action

#### form表单中的一些输入框

1. `<input type="text/password" name="xxx" value="xxx"/>`:text:文本输入框,password:密码输入框,name:文本框的命名,value:文本框的默认值,默认值会显示在输入框中;
2. 当输入大量的文字的时候,使用`<textarea>`：`<textarea cols="50" rows="10"></textarea>`：cols:设置列数，rows设置行数;
3. 单选框、复选框:`<input type="radio/checkbox" value="xxx" name="xxx" checked="checked"/>`:**单选框的name必须要相同，不然起不了单选的作用**
4. 下拉列表框:
```
<select>
    <option value="看书">看书</option>
	<option value="旅游" selected="selected">旅游</option>
	    <option value="运动">运动</option>
	        <option value="购物">购物</option>
</select>
```
value:是用来提交给服务器的数据,通常后端会通过request('name'):来获得这个值;

5. 下拉框还能多选:`<select multiple="multipie">`:这样可以多选(ctrl+单击进行多选)
6. 提交按钮:`<input type="submit" value="submit"/>`:value:用来在按钮上显示的字;
7. 与提交按钮类似的有一个重置按钮:`type=reset`

### div

1. `<div>`:在网页设计中可以把一些独立的逻辑板块划分出来，放到`<div>`标签中去；可以把`<div>`当做一个容器;
> 什么是逻辑部分？它是页面上相互关联的一组元素。如网页中的独立的栏目版块，就是一个典型的逻辑部分。
2. `<div id="xxx">`：给div加入id的话，可以让逻辑更加的清晰
3. `<table>`:表格，表格的表示方式有三个部分构成,`<table>`、`<th>`、`<tr>`、`<td>`,分别表示表格的开始，表格第一行标题行,表格中的一行，表格中的列;
```
    <table>
       <th>name</th>
    	<th>age</th>
    	    <tr>
    		<td>fenglin</td>
    		<td>15</td>
    	    </tr>
    	    <tr>
    		<td>yuedan</td>
	   	  	<td>25</td>
    	    </tr>
    </table>
```
4. 给表格加入标题:在`<table></table>`里面包含`<caption></caption>`

### Css

> 如果html是网页本身的内容的话，那么css就是用来让这些更加形象化的东西，类似于形式和内容之间的关系；

##### css:样式由选择符和声明组成，而声明又由属性和值组成，如图:

![css](http://frgeek-bed.qiniudn.com/other_css.jpg)

> **选择符**：又称选择器，指明网页中要应用样式规则的元素，如本例中是网页中所有的段（p）的文字将变成蓝色，而其他的元素（如ol）不会受到影响。**声明**：在英文大括号“｛｝”中的的就是声明，属性和值之间用英文冒号“：”分隔。当有多条声明时，中间可以英文分号“;”分隔，

1. css的注释表示:`/*xxxx*/`
2. 哪些地方可以写css呢?
							    
    - 内联式:直接把css写在现有的html标签上:`<p style="color:red">xxx</p>`;**css样式代码要写在style=""双引号中，如果有多条css样式代码设置可以写在一起，中间用分号隔开。**
    - 嵌入式:把css代码写在当前的文件中,通常是写在`<head></head>` 之间，形式如下:`<style type="text/css"> span{color:red;} </style>`
    - 外部式: css文件是单独成文件的，然后在文件内部通过一定的语法调用这个文件，然后让css在此文件中起作用;语法为:`<link href="base.css" rel="stylesheet" type="text/css" />`
    - css样式文件名称以有意义的英文字母命名，如 main.css。
    - rel="stylesheet" type="text/css" **是固定写法不可修改**。
    - `<link>`标签位置一般写在`<head>`标签之内。
				    
### CSS高级

1. 选择器
```
    选择器{
	样式;
	 }	
```
选择器的主要作用就是我们现在写的样式对网页上哪一个部分起作用;

2. 标签选择器:
标签就是指那些html标签，比如`<html>,<a>,<img>`之类的;

3. 类选择器:
标签选择器的缺点就是太过于死板，能控制的东西太少和太僵硬，而类选择器就很灵活，可以把你想要规定的东西写在一个类里面，然后在标签中使用class来指定;形式:`.类选器名称{css样式代码;}`
4. ID选择器:与类选择器的区别有两点:
    - 不是用class进行限制，而是使用id="xx"来指定;
    - 语法有区别，Id选择器是用#开始的,具体的形式:`#xxx{样式}`
5. 关于Id选择器与类选择器之间的区别;
6. 子选择器:在上面三种标签下面再进一步对里面的一些标签进行设置样式:`.first>span{xxx}`
7. 后代选择器:与子选择器不同的是，后代选择器作用于所有后代元素，但是子选择器则是直接后代元素;形式为:`.first span{xxx}`;
8. 通用选择器:`*{xxxx}`
9. 伪类选择器: 可以为html不存在的标签打上样式.比如`a:hover{xxxx}`;
10. 分组选择器:让多个标签使用同一个样式;`h1,html{xxxx}`;
11. 继承性:所谓的继承，其实就是样式不只是对指定的标签进行影响，还会对包含与内部的标签进行影响，默默的继承了父节点的样式，当然有的样式是不能的，比如`border:{}`;
12. 不同的选择器有不同的权限,标签选择器(1),类选择器(10),id选择器(100)
13. **层叠** 就是在html文件中对于同一个元素可以有多个css样式存在，当有相同权重的样式存在时，会根据这些css样式的前后顺序来决定，处于最后面的css样式会被应用;
13. CSS引入方式的优先级:内联样式表（标签内部）`>` 嵌入样式表（当前文件中）`>` 外部样式表（外部文件中）
