# Python学习笔记

好吧。。从寒假到现在都没学好Python。。
现在重新再过一遍。。
都不知道是第几次了orz

### 注释

Python使用的注释格式与C++、Java不同：

	# 这是一行注释

或者做一些形如C++中常用的TODO注释：

	# FIXME -- fix these code later
	# TODO -- in future you have to do this


### 输入输出

输入使用`input()`函数：

	a = int(input("Please input a integer: "))
	b = float(float("Please input a number: "))

输出使用`print()`函数进行格式化输出：

	print("Year {} Rs. {:.2f}".format(year, value))
	print("{:7.2f}".format(test))	# 7表示整个数字占7个字符，右对齐，空格补全

其中`print()`函数可以通过`end`控制输出结尾为其他字符而非空格

	print("{}".format(a), end = " ")

以及直接使用乘法进行连续输出：
	
	print("-" * 50)

### 多个赋值

	>>> a , b = 45, 54
	>>> a
	45
	>>> b
	54
	>>> a, b = b, a    # 可以用此法交换数值
	>>> a
	54
	>>> b
	45

### 除与整除

Python中对除和整除有明确的区分：

	>>> 3 / 2
	1.5
	>>> 3 // 2
	1

如果需要同时获取整除商和余数可以使用`divmod()`函数  
`divmod(a, b)`返回一个`(a // b, a % b)`的元组：

	a = 50
	print("{}, {}".format(*divmod(a, 30)))	# divmod前面的“*”表示将divmod返回的元组进行拆封

### 逻辑运算符

Python使用的是`and`、`or`、`not`，而不是`&&`、`||`、`!`

### 条件语句

	if ep1:
		...
	elif ep2:
		...
	else:
		...

### 列表

Python中使用方括号定义列表：

	a = [1, 342, 223, 'India', 'Fedora']

它可以像C语言中的数组一样访问

	>>> a[0]
	1
	>>> a[4]
	'Fedora'

也可以直接使用负数逆向访问：

	>>> a[-1]
	'Fedora'

也可以使用切片：

	>>> a[0:-1]
	[1, 342, 223, 'India']
	>>> a[2:-2]
	[223]
	>>> a[:]
	[1, 342, 223, 'India', 'Fedora']
	>>> a[:-2]
	[1, 342, 223]
	>>> a[-2:]
	['India', 'Fedora']

切片可以允许第三个参数，表示步长：

	>>> a[1::2]     # 此处省略第二个参数，表示从a[1]开始到最后，每两个取一个
	[342, 'India']

列表可以直接使用加法连接两个列表：

	>>> a + [36, 49, 64, 81, 100]
	[1, 342, 223, 'India', 'Fedora', 36, 49, 64, 81, 100]

列表可以直接对切片赋值，从而达到改变长度、批量操作之类的目的：

	>>> letters = ['a', 'b', 'c', 'd', 'e', 'f', 'g']
	>>> letters
	['a', 'b', 'c', 'd', 'e', 'f', 'g']
	>>> # 替换某些值
	>>> letters[2:5] = ['C', 'D', 'E']
	>>> letters
	['a', 'b', 'C', 'D', 'E', 'f', 'g']
	>>> # 现在移除他们
	>>> letters[2:5] = []
	>>> letters
	['a', 'b', 'f', 'g']
	>>> # 通过替换所有元素为空列表来清空这个列表
	>>> letters[:] = []
	>>> letters
	[]

`len()`函数可以获取列表的长度：

	>>> len(a)
	5

列表允许嵌套，此时便是类似于多维数组的存在（准确的说应该是多维不定长的vector）

	>>> a = ['a', 'b', 'c']
	>>> n = [1, 2, 3]
	>>> x = [a, n]
	>>> x
	[['a', 'b', 'c'], [1, 2, 3]]
	>>> x[0]
	['a', 'b', 'c']
	>>> x[0][1]
	'b'

### for循环

Python中的for循环与C语言的差别较大：

	>>> a = [1,2,4,6]
	>>> for i in a:
	...     print(i)
	... 
	1
	2
	4
	6

如上所示，for循环被特化用于遍历一个列表（当然它也可以遍历一个切片）  
如果需要直接枚举数字的话（例如实现C语言中`for(i=0;i<10;i++)`这样的），可以直接使用`range()`函数  
例如在C语言中的`for(i=0;i<10;i++)`，在Python中便可以使用`for i in range(10)`就行了  

`range()`函数的作用如下所示：

	>>> for i in range(5):
	...     print(i)
	...
	0
	1
	2
	3
	4
	>>> range(1, 5)      
	range(1, 5)
	>>> list(range(1, 5))
	[1, 2, 3, 4]
	>>> list(range(1, 15, 3))
	[1, 4, 7, 10, 13]
	>>> list(range(4, 15, 2))
	[4, 6, 8, 10, 12, 14]

for循环可以直接接else，表示循环终止后需要做的事情（如果是break出来的则不会执行els里的东西）：

	>>> for i in range(0, 5):
	...     print(i)
	... else:
	...     print("Bye bye")
	...
	0
	1
	2
	3
	4
	Bye bye