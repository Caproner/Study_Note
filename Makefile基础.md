# Makefile基础

在Linux中，Shell强迫症勇士总是不想借助IDE的力量。  
于是呢，对于我们来说，Makefile就是一个编译项目的不错的选择。  

## Makefile的格式

Makefile的最基础的格式如下：
```makefile
target... : prerequisites ...
command
...
...
```
+ `target`：要编译生成的文件名（目标文件名），或者是一个标签
+ `prerequisites`：要生成那个target所需要的文件或是目标
+ `command`：make要执行的命令

## 一个简单的例子

以项目[Naive_Compiler](http://github.com/Caproner/Naive_Compiler "Naive_Compiler")为例  
这个项目的结构大致上为，根目录的main.cpp调用三个文件夹中的三个cpp组成一个完整的项目  
因为这个项目正如其名一样十分的Naive，所以它的Makefile写起来也不长：  
```makefile
Naive : main.o Lexical/Lexical.o Grammar/Grammar.o Semantic/Semantic.o
	g++ -o Naive main.o Lexical/Lexical.o Grammar/Grammar.o Semantic/Semantic.o

main.o : main.cpp
	g++ -c -o main.o main.cpp

Lexical/Lexical.o : Lexical/Lexical.cpp
	g++ -c -o Lexical/Lexical.o Lexical/Lexical.cpp

Grammar/Grammar.o : Grammar/Grammar.cpp
	g++ -c -o Grammar/Grammar.o Grammar/Grammar.cpp

Semantic/Semantic.o : Semantic/Semantic.cpp
	g++ -c -o Semantic/Semantic.o Semantic/Semantic.cpp

clean:
	rm Naive main.o Lexical/Lexical.o Grammar/Grammar.o Semantic/Semantic.o
```
`make`命令会先去找所在目录下有没有Makefile，如果有的话就执行Makefile中的第一个target并执行它，如果当前执行的target的依赖中有未执行的target，则会先执行依赖中的target，如此递归下去直到第一个target执行完毕  

注意target的下一行的命令**一定要以tab键开头**  

在上面有一个特殊的target，叫clean。这个并不是一个目标文件，而是一个动作  
运行`make`命令的时候显然不会运行到它，但是运行`make clean`的时候就会从这个动作开始运行，而不是去找第一个动作

## 使用变量

Makefile允许在上面定义变量，用于减少文本量，和使得整个Makefile比较清晰（大概）  
例如，上面的Makefile就可以通过定义变量来减少一定的文本量：
```makefile
objects = main.o Lexical/Lexical.o Grammar/Grammar.o Semantic/Semantic.o

project = Naive

$(project) : $(objects)
	g++ -o $(project) $(objects)

main.o : main.cpp
	g++ -c -o main.o main.cpp

Lexical/Lexical.o : Lexical/Lexical.cpp
	g++ -c -o Lexical/Lexical.o Lexical/Lexical.cpp

Grammar/Grammar.o : Grammar/Grammar.cpp
	g++ -c -o Grammar/Grammar.o Grammar/Grammar.cpp

Semantic/Semantic.o : Semantic/Semantic.cpp
	g++ -c -o Semantic/Semantic.o Semantic/Semantic.cpp

clean:
	rm $(project) $(objects)
```

## make自动推导

`make`可以实现一定的自动推导  
例如说，当需要依赖main.o的时候，make就可以知道它的依赖文件肯定是main.cpp，并可以推导得出编译命令为`g++ -c -o main.o main.cpp`  
于是这样就可以省去十分多的东西：
```makefile
objects = main.o Lexical/Lexical.o Grammar/Grammar.o Semantic/Semantic.o

project = Naive

$(project) : $(objects)
	g++ -o $(project) $(objects)

.PHONY : clean

clean:
	-rm $(project) $(objects)
```

此处顺便对clean做了两处修改：
+ 添加了`.PHONY : clean`，表示clean是一个**伪目标**
+ 在`rm`前面加上`-`，表示也许某些文件出现问题，但不要管，继续做后面的事

## 写在最后

> 我帽子就是累死！  
> 死外边！写Makefile写到猝死！  
> 也不会用你们IDE一点东西的！  

...  
...  
...  

> 真香！