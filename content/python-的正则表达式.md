title: 'python 的正则表达式'
date: 2015-01-21 22:35:44
categories: python
tags: 正则
---

python 的正则表达式基本语法与其他语言类似，这文章主要是用来记录一些python相关的正则表达式的使用方法;

### 1. python 的re模块

	import re
	
python有raw-string，这个字符串主要可以保证正则表达式的特殊字符需要转义；比如

	s = r'ABC\-001' # Python的字符串
	# 对应的正则表达式字符串不变：
	# 'ABC\-001'
	
	s = 'ABC\\-001' # Python的字符串
	# 对应的正则表达式字符串变成：
	# 'ABC\-001'
	
### 2. 基本方法

#### 2.1 match

	re.match(r'^\d{3}\-\d{3,8}$', '010-12345')
	
match如果有匹配就返回match对象，如果没有就返回None

#### 2.2 re.split

用正则表达式来进行切分字符串

#### 2.3 分组

在match对象上调用group函数可以得到分组；

	group(0):永远是原始字符串，group(1)、group(2)……表示第1、2、……个子串
	
	groups可以得到所有分组信息;但是不包含原始字符串
	
#### 2.4 complie

	re_telephone = re.compile(r'^(\d{3})-(\d{3,8})$')
	re_telephone.match('010-12345').groups()
	
	

