# Essential C++

主要用于回顾一下C++基础

UPD：这TM是基础？？？

## 第1章 C++编程基础

### 零散笔记

+ C/C++是无格式语言，也就说可以在语句任意位置（不包括在关键字、函数名）换行
  + 这对格式规范十分重要
+ `NULL`的值是0
+ 在编程的时候，需要时刻注意**空指针**所带来的异常

## 第2章 面向过程的编程风格

### 模板函数

```c++
template <typename elemType>
void fun(elemType a, vector<elemType> v)
{
	// ...
}
```

### 函数指针
+ 定义：`int (*fun)(int, int);`
	+ 如果是函数指针数组的话则是`int (*fun[])(int, int);`
+ 赋值：
	+ 假设有`int max(int, int)`这个函数
	+ 赋值操作为`fun = max;`
+ 调用：`fun(a, b);`

### 零散笔记

+ 在多文件系统中，可以用`extern`关键字声明一个变量
	+ 如果不这么做的话则是定义一个变量，那么该变量的作用范围就仅限于当前文件

## 第3章 泛型编程风格

### 泛型算法

只要目标类型满足一定条件（例如可比较性），就可以无视算法执行对象的类型的算法

例如说STL中的`algorithm`头文件中的算法（`sort()`，`reverse()`，`max()`，...）

### 泛型指针（iterator）
对于STL的容器来说，它们都会有用于迭代的泛型指针

以`vector<string>`为例，其泛型指针的类型便为`vector<string>::iterator`

这些指针与普通的指针差不多，均可以进行取值，自增，自减，比较是否相等

于是便可以使用模板函数进行一些泛型算法的设计：（以`find`为例）  

```c++
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

### Function Object（函数对象）  

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


### Function Object Adapter（函数对象适配器）  

有时候以上15种并不能够满足我们的需求，这个时候就需要adapter来对其做一些改变，使其能满足我们的需求  
+ binder adapter（绑定适配器）：
  + 将二元的function object的一些参数进行一个绑定，使其降为一元
  + 有两种binder adapter：
    + bind1st：绑定第一个参数
      + 例如说，返回该数被5减后的数的function object，就需要将`minus<type>`做一些改动：

         ```c++
         minus<int> lt;
         bind1st(lt, 5);	// 返回我们所需要的function object，当然这句这么用本身是无效的
         ```

    + bind2nd：绑定第二个参数，使用方法同上
+ negator adapter（否定适配器）：
  + 作用是将function object的结果取反
  + `not1()`的作用是对一元的function object取反
  + `not2()`的作用是对二元的function object取反

### Iterator Inserter  

在泛型编程中，往往会遇到需要将列表输出的情况。这个时候一般的是使用列表指针来进行操作：

```c++
template <typename OutputIterator>
OutputIterator fun(OutputIterator at)
{
	for(int i=0;i<10;i++)
		*at++ = i;
	return at;
}
```

对于数组来说，调用这个函数只需要传入其指针就行了：

```c++
int a[15];
fun(a);
```

但是对于vector之类的容器来说，由于不知道其中会产生多大的列表，所以比较棘手

这个时候就需要使用Iterator Inserter

Iterator Inserter分成以下几种：

+ `back_inserter()`：以`push_back()`的形式将返回值加入到容器中：

   ```c++
   vector<int> v;
   fun(back_inserter(v));
   ```

+ `inserter()`：以`insert()`的形式将返回值加入到容器中：

   ```c++
   vector<int> v;
   fun(inserter(v,v.end()));	// 第二个参数是insert的位置
   ```

+ `front_inserter()`：以`push_front()`的形式将返回值加入到容器中：

  ```c++
  deque<int> d;
  fun(front_inserter(d));
  ```

### Iostream Iterator  

假定现在有一个这样的函数：

```c++
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
```

它的功能很简单，就是复制一个列表到另一个列表

但是使用如下Iostream Iterator之后，可以将其修改为从输入中复制，或是从现有列表中输出

如果需要文件输入或文件输出的话，只需要修改初始化时使用的流即可

+ `istream_iterator<type>`	

   ```c++
   int a[20];
   istream_iterator<int> input(cin);
   istream_iterator<int> eof;	// 若对其未做初始化则默认为EOF
   Copy(input, eof, a);	// 将输入赋值给数组a
   ```

+ `ostream_iterator<type>`

   ```c++
   int a[20];
   // 省略对a的赋值操作
   ostream_iterator<int> output(cout, " ")	// 后面的字符串用于规定其分隔符
   Copy(a, a+20, output);	// 将数组a输出
   ```

## 第4章 基于对象的编程风格

### 常成员函数

假设现在有一个类`fun`，和如下一个函数`print()`：

```c++
void print(const fun &t)
{
	t.print();
}
```

其中该函数的参数为一个`fun`的对象的一个常引用

常引用则意味着函数中不能去改变参数`t`的值，但是要怎么保证`t.print()`不会去改变其中的值呢？

这个时候，`fun`中的成员函数`print()`应该如下定义：

```c++
void fun::print() const
{
    // do something...
}
```

注意函数声明中的`const`修饰符，这表明该函数不会修改到类中的成员

即，该函数是一个**常成员函数**

该修饰符不管在声明中还是定义中都需要加

### mutable成员

`mutable`关键字声明一个成员是可变的：

```c++
class fun
{
    public:
    	mutable int a;
    	void have_fun() const
    	{
        	a = 2;	// 该句是合法的
    	}
};
```

听起来比较奇怪，甚至感觉跟没声明一样

事实上区别在于，当一个成员被声明为`mutable`时，该类的常成员函数便可以对它进行修改

此时认为修改`mutable`成员不会破坏该类的常量性（constness）

### 静态成员函数

当一个成员函数只访问该类中的静态成员时，该函数可以声明为`static`，表明其与任何类的对象无关

静态成员函数定义时可以不用加上`static`

调用静态成员函数时需要使用class scope来调用：

```c++
fun::something_static();
```

### Iterator Class

现在假设有一个类`fun`，我们想要让它能够跟`vector`一样用迭代器遍历：

```c++
fun a;
for(fun::iterator it = a.begin(); it != a.end(); it++)
{
    // ...
}
```

事实上只需要写一个`fun_iterator`类作为`fun`类的迭代器，并重载一些诸如赋值，判断，移位等操作，然后再在`fun`类中加入如下代码即可：

```c++
class fun_iterator
{
    // ...
};
class fun
{
    public:
    	// ...
    
    	// 声明友元类，声明friend关系可以使fun_iterator类能访问到fun的private成员
    	friend class fun_iterator;
    
    	typedef fun_iterator iterator;
    	
    	fun_iterator begin() const
        {
            // ...
        }
    
    	fun_iterator end() const
        {
            // ...
        }
    
    // ...
};
```

### 零散笔记

+ 在类中定义函数（而不是只是声明）的话，该成员函数会自动被视为`inline`函数
+ 注意，当一个成员被设计成`const`的时候，要保证在类设计的时候该成员永远都无法被修改（各种渠道上都）
+ 在进行对象的复制的时候，需要注意判断这两个对象**是否是同一个对象**

## 第5章 面向对象编程风格

### 虚函数

虚函数是为了实现基类与派生类之间的动态绑定，当在基类中调用虚函数时，虚函数会知道其需要调用哪个派生类的函数。例如：

```c++
base_fun *a = new fun();
```

假设fun为base_fun的派生类，那么此时，如果a需要调用一个虚函数，则其会去寻找其指向的派生类的对应函数执行

+ 对于每个虚函数，要么其必须有定义，要么其必须为**纯虚函数**：

  ```c++
  virtual void have_fun() = 0;	// 声明其为纯虚函数
  ```

+ 虚函数的声明只需要在基类声明即可，其派生类的对应函数会自动声明为虚函数

+ 虚析构函数：基类的析构函数需要声明为虚函数

  + 原因：考虑以下情况：

    ```c++
    base_fun *a = new fun();
    delete a;
    ```

  + 如果base_fun的析构函数不是虚函数的话，那么在释放a所知的对象时，就只会调用base_fun的析构函数（因为它毕竟还是个base_fun的指针），而当其析构函数被声明为虚析构函数时，其就会从虚函数表中找到对应派生类的析构函数执行，之后再执行自身的析构函数

### typeid运算符

使用`typeid`运算符可以用于判定一个变量的类型：

```c++
base_fun *a = new fun();
if(typeid(*a) == typeid(fun))	// true
{
    cout<<"bingo!"<<endl;
}
if(typeid(*a) == typeid(base_fun))	// false
{
    cout<<"prime bingo!"<<endl;
}
```

## 第6章 以template进行编程

### 非参数类型

`template`的参数不单单可以是一种类型，还可以是一个常量表达式：

```c++
template <int len>
class num_sequence
{
public:
    num_sequence( int beg_pos = 1 );
    // ...
};

template <int len>
class Fibonacci : public num_sequence<len>
{
public:
	Fibonacci( int beg_pos = 1 )
		: num_sequence<len>( beg_pos ){}
	// ...
};
```

当使用这个类定义对象的时候，便可以这么做：

```c++
Fibonacci< 17 > fib1;
Fibonacci< 17 > fib2( 16 );
```

常量表达式与函数参数一样，也支持默认参数值：

```c++
template <int len, int beg_pos = 1>
class num_sequence
{
    // ...
};
```

## 第7章 异常处理

### 抛出异常

