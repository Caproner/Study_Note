# Essential C++

主要用于回顾一下C++基础

## 第1章 C++编程基础

+ C/C++是无格式语言，也就说可以在语句任意位置（不包括在关键字、函数名）换行
	+ 这对格式规范十分重要
+ `NULL`的值是0
+ 在编程的时候，需要时刻注意**空指针**所带来的异常

## 第2章 面向过程的编程风格

+ 模板函数：
	```
	template <typename elemType>
	void fun(elemType a, vector<elemType> v)
	{
		//...
	}
	```
+ 函数指针
	+ 定义：`int (*fun)(int, int);`
		+ 如果是函数指针数组的话则是`int (*fun[])(int, int);`
	+ 赋值：
		+ 假设有`int max(int, int)`这个函数
		+ 赋值操作为`fun = max;`
	+ 调用：`fun(a, b);`
+ 在多文件系统中，可以用`extern`关键字声明一个变量
	+ 如果不这么做的话则是定义一个变量，那么该变量的作用范围就仅限于当前文件

## 第3章 泛型编程风格

+ 泛型算法：只要目标类型满足一定条件（例如可比较性），就可以无视算法执行对象的类型的算法
	+ 例如说STL中的`algorithm`头文件中的算法（`sort()`，`reverse()`，`max()`，...）
+ 泛型指针（iterator）：对于STL的容器来说，它们都会有用于迭代的泛型指针
	+ 以`vector<string>`为例，其泛型指针的类型便为`vector<string>::iterator`
	+ 这些指针与普通的指针差不多，均可以进行取值，自增，自减，比较是否相等
	+ 于是便可以使用模板函数进行一些泛型算法的设计：（以`find`为例）
		```
		template <typename IteratorType, typename ElemType>
		IteratorType find(IteratorType     begin, 
		                  IteratorType     end, 
		                  const ElemType&  elem)
		{
			for(IteratorType it = begin; it != end; it++)
			{
				if((*it) == elem)
				{
					return it;
				}
			}
			return end;
		}
		```
+ Function Object（函数对象）
	指一类仅重载了`()`的类  
	可以用于`sort()`等算法中的参数（例如说`sort(v.begin(), v.end(), greater<int>)`），也可以用于自行设计泛型算法时用到  
	在标准库`functional`中就有15个已经预先定义好的function object：
	+ 算术运算：
		+ `plus<type>`：`x+y`
		+ `minus<type>`：`x-y`
		+ `multiplies<type>`：`x*y`
		+ `divides<type>`：`x/y`
		+ `modules<type>`：`x%y`
		+ `negate<type>`：`-x`
	+ 关系运算：
		+ `less<type>`：`x<y`
		+ `less_equal<type>`：`x<=y`
		+ `greater<type>`：`x>y`
		+ `greater_equal<type>`：`x>=y`
		+ `equal_to<type>`：`x==y`
		+ `not_equal_to<type>`：`x!=y`
	+ 逻辑运算：
		+ `logical_and<type>`：`x&&y`
		+ `logical_or<type>`：`x||y`
		+ `logical_not<type>`：`!x`
+ Function Object Adapter（函数对象适配器）
	有时候以上15种并不能够满足我们的需求，这个时候就需要adapter来对其做一些改变，使其能满足我们的需求
	+ binder adapter（绑定适配器）：
		+ 将二元的function object的一些参数进行一个绑定，使其降为一元
		+ 有两种binder adapter：
			+ bind1st：绑定第一个参数
				+ 例如说，返回该数被5减后的数的function object，就需要将`minus<type>`做一些改动：
					```
					minus<int> lt;
					bind1st(lt, 5);	//返回我们所需要的function object，当然这句这么用本身是无效的
					```
			+ bind2nd：绑定第二个参数，使用方法同上
	+ negator adapter（否定适配器）：
		+ 作用是将function object的结果取反
		+ `not1()`的作用是对一元的function object取反
		+ `not2()`的作用是对二元的function object取反
+ Iterator Inserter
	在泛型编程中，往往会遇到需要将列表输出的情况。这个时候一般的是使用列表指针来进行操作：
		template <typename OutputIterator>
		OutputIterator fun(OutputIterator at)
		{
			for(int i=0;i<10;i++)
				*at++ = i;
			return at;
		}
	对于数组来说，调用这个函数只需要传入其指针就行了：
		int a[15];
		fun(a);
	但是对于vector之类的容器来说，由于不知道其中会产生多大的列表，所以比较棘手  
	这个时候就需要使用Iterator Inserter  
	Iterator Inserter分成以下几种：
	+ `back_inserter()`：以`push_back()`的形式将返回值加入到容器中：
			vector<int> v;
			fun(back_inserter(v));
	+ `inserter()`：以`insert()`的形式将返回值加入到容器中：
			vector<int> v;
			fun(inserter(v,v.end()));	//第二个参数是insert的位置
	+ `front_inserter()`：以`push_front()`的形式将返回值加入到容器中：
			deque<int> d;
			fun(front_inserter(d));
+ Iostream Iterator
	假定现在有一个这样的函数：
		template <typename InputIterator, typename OutputIterator>
		OutputIterator Copy(InputIterator   begin,
		                    InputIterator   end,
							OutputIterator  at)
		{
			InputIterator it = begin;
			while(it != end)
			{
				*at++ = *it++;
			}
			return at;
		}
	它的功能很简单，就是复制一个列表到另一个列表  
	但是使用如下Iostream Iterator之后，可以将其修改为从输入中复制，或是从现有列表中输出
	如果需要文件输入或文件输出的话，只需要修改初始化时使用的流即可
	+ `istream_iterator<type>`：
			int a[20];
			istream_iterator<int> input(cin);
			istream_iterator<int> eof;	//若对其未做初始化则默认为EOF
			Copy(input, eof, a);	//将输入赋值给数组a
	+ `ostream_iterator<type>`：
			int a[20];
			//省略对a的赋值操作
			ostream_iterator<int> output(cout, " ")	//后面的字符串用于规定其分隔符
			Copy(a, a+20, output);	//将数组a输出
