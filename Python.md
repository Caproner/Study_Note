# Python学习笔记

好吧。。从寒假到现在都没学好Python。。
现在重新再过一遍。。
都不知道是第几次了orz

## 注释

Python使用的注释格式与C++、Java不同：

	# 这是一行注释

或者做一些形如C++中常用的TODO注释：

	# FIXME -- fix these code later
	# TODO -- in future you have to do this


## 输入输出

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

## 多个赋值

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

## 除与整除

Python中对除和整除有明确的区分：

	>>> 3 / 2
	1.5
	>>> 3 // 2
	1

如果需要同时获取整除商和余数可以使用`divmod()`函数  
`divmod(a, b)`返回一个`(a // b, a % b)`的元组：

	a = 50
	print("{}, {}".format(*divmod(a, 30)))	# divmod前面的“*”表示将divmod返回的元组进行拆封

## 逻辑运算符

Python使用的是`and`、`or`、`not`，而不是`&&`、`||`、`!`

## 条件语句

	if ep1:
		...
	elif ep2:
		...
	else:
		...

## 列表

### 定义

Python中使用方括号定义列表：

	a = [1, 342, 223, 'India', 'Fedora']

### 访问

它可以像C语言中的数组一样访问

	>>> a[0]
	1
	>>> a[4]
	'Fedora'

也可以直接使用负数逆向访问：

	>>> a[-1]
	'Fedora'

### 列表切片

Python中的列表可以使用切片提取部分：

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
	>>> a[::-1]     # 这么做可以反转列表
	['Fedora', 'India', 223, 342, 1]

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

### 列表的连接

列表可以直接使用加法连接两个列表：

	>>> a + [36, 49, 64, 81, 100]
	[1, 342, 223, 'India', 'Fedora', 36, 49, 64, 81, 100]

### 多维列表

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

### 列表常用方法

#### 列表本身的方法

1. `append(a)`：尾部添加元素a
2. `insert(pos, a)`：在下标为pos的元素前插入元素a
3. `count(a)`：统计元素a出现的次数
4. `remove(a)`：移除第一个值为a的元素
5. `reverse()`：反转列表
6. `extend(b)`：将列表b连接在原列表后面
7. `sort()`：对列表进行升序排序
8. `pop()`：尾部pop，如果使用`pop(i)`的话则是将下标为i的元素弹出

#### 外部调用

1. `len(a)`：获取列表a的长度
2. `del a[pos]`：删除列表a中下标为pos的元素

### 列表推导式

可以使用将for循环写入列表的定义中的方式进行列表的创建：

	>>> squares = [x**2 for x in range(10)]
	>>> squares
	[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
	>>> a = [[x, y] for x in [1, 2, 3] for y in [1, 2, 3] if x != y]
	>>> a
	[[1, 2], [1, 3], [2, 1], [2, 3], [3, 1], [3, 2]]

推导式同样支持嵌套

## for循环

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

for循环可以直接接else，表示循环终止后需要做的事情（如果是break出来的则不会执行else里的东西）：

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

## 元组

元组可以看做是一经定义就不能改变的列表  
其定义方式为：

	>>> a = 1, 2, 3, 4
	>>> a
	(1, 2, 3, 4)

上面提到的多个赋值的方式实际上就是元组的封装和拆封

## 集合

集合使用花括号进行定义：

	>>> a = {1, 2, 3, 3}
	>>> a
	{1, 2, 3}
	>>> 1 in a
	True

集合支持`-`（差集），`&`（交集），`|`（并集），`^`（异或）运算

## 字典

字典相当于C++中的map，它使用键值对表示一对映射，同样使用花括号进行定义：

	>>> a = {1: 1, 2: 10, 3: 11, 4: 100}
	>>> a[4]
	100
	>>> a
	{1: 1, 2: 10, 3: 11, 4: 100}
	>>> a['62'] = 'A'
	>>> a
	{1: 1, 2: 10, 3: 11, 4: 100, '62': 'A'}
	>>> 4 in a
	True
	>>> for x, y in a.items():    # 使用items()将其中每个元素转换成元组输出
	...     print("{} is {}".format(x, y))
	... 
	1 is 1
	2 is 10
	3 is 11
	4 is 100
	62 is A

## 字符串

跟C++一样，Python的字符串也使用引号表示，不过Python单双引号都是允许的   
同时，字符串也是列表的一种，故其支持切片操作

### 字符串常用方法

1. `upper()`：返回其全大写的版本
2. `lower()`：返回其全小写的版本
3. `isalnum()`：判断其是否全由字母和/或数字组成
4. `isdigit()`：判断其是否全是数字
5. `isalpha()`：判断其是否全是字母
6. `isupper()`：判断其是否全是大写
7. `islower()`：判断其是否全是小写
8. `split(c)`：按字符c分割字符串，c缺省时默认按空格分割
9. `join()`：连接字符串，使用方法如下：
	
		>>> "-".join("GNU/Linux is great".split())
		'GNU/Linux-is-great'
	
10. `strip(s)`：剥离字符串首尾的所有**包含在字符串s中**的字符

		>>> s = """\         
		...   what   
		... the
		... fuck   
		... 
		... """
		>>> s
		'  what\nthe\nfuck   \n\n'
		>>> s.strip()
		'what\nthe\nfuck'

11. `lstrip(s)`：相对于`strip()`而言，只剥离左边的
12. `rstrip(s)`：相对于`strip()`而言，只剥离右边的
13. `find(s)`：返回字符串中第一个与s相同的子串的下标，如果没有返回-1

## 函数

### 函数的定义

函数的一般定义形式如下：

	def functionname(params):
    	statement

例如如下函数：

	def sum(a, b):
		return a + b

同时，函数支持默认参数：

	def sum(a, b = 10):
		return a + b

### 函数中使用全局变量

假设有一个全局变量a，此时函数需要使用它（假设给它赋值为2）  
但是在python中，直接使用`a = 2`的话，结果会是创建一个局部变量2并给其赋值为2  
所以需要在函数中先加入如下语句：

    global a

### 关键字参数

在Python中函数不一定要按照其原本的顺序进行调用   
例如调用上面的`sum()`的时候就可以使用形如`sum(b = 9, a = 10)`的形式调用

### map函数

	>>> lst = [1, 2, 3, 4, 5]
	>>> def square(num):
	...     "返回所给数字的平方."
	...     return num * num
	...
	>>> print(list(map(square, lst)))
	[1, 4, 9, 16, 25]

## 异常处理

在Python中的异常处理依旧是使用`try+except`的模式

### 抛出异常

`raise`关键字用于抛出异常：

	raise ValueError("A value error happened.")

## 类

Python中的类通常用如下定义：

	class MyClass(parent_class):
    	statements
		...
其中，如果该类不需要继承自其他类时，需要继承自`object`类

而使用该类定义对象只需要如下操作：

	x = MyClass()

### 构造函数

Python的类中可以使用`__init__()`函数，该函数等同于C++的构造函数：

	def __init__(self):
    	self.data = []

其中`self`是必加的，它类似于C++中的`this`

### 删除对象

假设定义一个对象为`s`，则删除它需要使用的语句为：

	del s

### 属性读取方法

在Python中，不需要去定义其属性；读取的时候也只需要直接读取就行（不需要特地写个`getName()`之类的方法）：

	>>> class Student(object):
	...     def __init__(self, name):
	...         self.name = name
	...
	>>> std = Student("Kushal Das")
	>>> print(std.name)
	Kushal Das
	>>> std.name = "Python"
	>>> print(std.name)
	Python

### 装饰器

在Python中，如果需要更加明确地声明类中的方法，可以使用装饰器`@property`：

	#!/usr/bin/env python3
	
	class Account(object):
	    """账号类,
	    amount 是美元金额.
	    """
	    def __init__(self, rate):
	        self.__amt = 0
	        self.rate = rate
	
	    @property
	    def amount(self):
	        """账号余额（美元）"""
	        return self.__amt
	
	    @property
	    def cny(self):
	        """账号余额（人名币）"""
	        return self.__amt * self.rate
	
	    @amount.setter
	    def amount(self, value):
	        if value < 0:
	            print("Sorry, no negative amount in the account.")
	            return
	        self.__amt = value
	
	if __name__ == '__main__':
	    acc = Account(rate=6.6) # 基于课程编写时的汇率
	    acc.amount = 20
	    print("Dollar amount:", acc.amount)
	    print("In CNY:", acc.cny)
	    acc.amount = -100
	    print("Dollar amount:", acc.amount)

## 模块

一个Python程序本身就可以作为一个模块
其中的函数或类在别的程序import它之后就可以使用了
而在一个文件夹中加入`__init__.py`的文件之后，这个文件夹就会成为一个包

## collections模块

`collections`模块是Python的一个内建模块，其中提供了一些例如`sort`之类的常用方法

### Counter类

`Counter`类用于进行对字符等进行计数：

	>>> from collections import Counter
	>>> c = Counter(a=4, b=2, c=0, d=-2)
	>>> list(c.elements())
	['b','b','a', 'a', 'a', 'a']
	>>> Counter('abracadabra').most_common(3)
	[('a', 5), ('r', 2), ('b', 2)]

### defaultdict类

事实上就是一个扩展的`dict`  
由于`dict`不支持像C++的map一样在键缺省时自动创建（会报错），所以就有了`defaultdict`：

	>>> from collections import defaultdict
	>>> s = [('yellow', 1), ('blue', 2), ('yellow', 3), ('blue', 4), ('red', 1)]
	>>> d = defaultdict(list)
	>>> for k, v in s:
	...     d[k].append(v)
	...
	>>> d.items()
	dict_items([('blue', [2, 4]), ('red', [1]), ('yellow', [1, 3])])

### namedtuple类

顾名思义，就是带命名的元组：

	>>> from collections import namedtuple
	>>> Point = namedtuple('Point', ['x', 'y'])  # 定义命名元组
	>>> p = Point(10, y=20)  # 创建一个对象
	>>> p
	Point(x=10, y=20)
	>>> p.x + p.y
	30
	>>> p[0] + p[1]  # 像普通元组那样访问元素
	30
	>>> x, y = p     # 元组拆封
	>>> x
	10
	>>> y
	20