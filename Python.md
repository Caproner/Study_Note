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