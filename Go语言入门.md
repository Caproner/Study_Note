# Go语言入门

好像是工作需要用到？嘛先学一下防止工作扑街吧

## Hello World

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
} 
```

## Go语言结构

以**Hello World**为例：

+ `package main`：表示该文件属于main包中，`package`语句必须是文件的第一句非注释语句
+ `import "fmt"`：表示该文件需要使用fmt包里的东西。fmt包包含了格式化I/O的东西
+ `func main(){}`：相当于C++的`int main(){}`
+ `fmt.Println("Hello, World!")`：输出语句。注意Go不需要分号

以下为另外需要注意的地方：

+ `/*...*/`：这个表示注释。注释的方式跟C++一样
+ 文件内的任何标识符（包括常量、变量、类型、函数名、结构字段等等），如果以大写字母开头，则表示其对外部可见；如果以小写字母开头，则表示其对外部不可见（也就是说，需要public出去的必须是大写字母开头的）
+ **注意Go程序的左大括号前不能换行！！！**这十分违反之前自己习惯的代码习惯

## 如何执行Go程序

假设上面的程序保存为`hello.go`，那么运行该程序的命令为：

```shell
go run hello.go
```

## 变量

Go语言使用如下格式定义变量：

```go
var 变量名 变量类型
```

例如说：

```go
var a int
var b bool = true
```

如果有初始值的话甚至可以省略类型，让其自行判断：

```go
var b = true
```

甚至使用`:=`运算符来省略`var`关键字：（但只能用于函数体内）

```go
b := true
```

Go语言同样支持多变量的定义：

```go
var var1, var2, var3 = 1, 2, 3
```

甚至可以利用这个特性进行变量值交换：

```go
a, b = b, a
```

或者：（这种多用于全局变量）

```go
var (
	a int
	b bool
)
```

Go语言的变量类型跟Python差不多，一般常用的就`int`，`float64`，`bool`

## 常量

Go语言的常量只需要将变量前面的`var`换成`const`修饰符就行了。

## iota

`iota`为一个特殊的常量，其可以看做是`const`语句块中的行索引：

```go
const (
	a = iota	// 此时iota=0，故a=iota=0
	b			// 此时iota=1，故b=iota=1
	c			// 此时iota=2，故c=iota=2
)
```

### 例子1

```go
package main

import "fmt"

func main() {
    const (
            a = iota   //0
            b          //1
            c          //2
            d = "ha"   //独立值，iota += 1
            e          //"ha"   iota += 1
            f = 100    //iota +=1
            g          //100  iota +=1
            h = iota   //7,恢复计数
            i          //8
    )
    fmt.Println(a,b,c,d,e,f,g,h,i)
}
```

其运行结果为：

```shell
0 1 2 ha ha 100 100 7 8
```

### 例子2

```go
package main

import "fmt"

const (
    i=1<<iota
    j=3<<iota
    k
    l
)

func main() {
    fmt.Println("i=",i)
    fmt.Println("j=",j)
    fmt.Println("k=",k)
    fmt.Println("l=",l)
}
```

其运行结果为：

```shell
i= 1
j= 6
k= 12
l= 24
```

## 运算符

Go语言的运算符几乎与C++无异。甚至是取地址符和定义指针都是一样的。

+ 指针的定义：`var a *int`

## 条件判断

### if系列

Go的`if...else`跟C++差别不大。主要有两个差别：

+ 判断条件可以不加小括号

+ 大括号不换行TMD！！！！

  + 也就是说，Go的`if...else`必须是：

  ```go
  if a > 20 {
      ...
  } else {
      ...
  }
  ```


### switch系列

同样是不需要小括号以及蛋疼的不换行，还有就是**不需要break**：

```go
switch var1 {
    case val1:
        ...
    case val2:
        ...
    default:
        ...
}
```

## 循环语句

Go中没有`while`语句，只有`for`语句。其支持以下三种方式：

```go
for init; condition; post { }	// C++的for
for condition { }				// C++的while
for { }							// 无限循环
```

其同样支持`break`和`continue`

## 函数

Go的函数格式为：

```go
func function_name( [parameter list] ) [return_types] {
   函数体
}
```

以如下函数为例：

```go
/* 函数返回两个数的最大值 */
func max(num1, num2 int) int {
   /* 声明局部变量 */
   var result int

   if (num1 > num2) {
      result = num1
   } else {
      result = num2
   }
   return result 
}
```

Go函数的返回值可以是多个的：

```go
func swap(s1, s2 string) (string, string) {
    return s2, s1
}
```

## 闭包

Go支持闭包函数，也就是在函数内定义一个匿名函数用于返回给外部使用，这种函数就称为闭包函数

闭包函数可以访问和修改其所在的函数内的变量

例如以下代码：

```go
package main

import "fmt"

func getSequence() func() int {
   i:=0
   return func() int {
      i+=1
     return i  
   }
}

func main(){
   /* nextNumber 为一个函数，函数 i 为 0 */
   nextNumber := getSequence()  

   /* 调用 nextNumber 函数，i 变量自增 1 并返回 */
   fmt.Println(nextNumber())
   fmt.Println(nextNumber())
   fmt.Println(nextNumber())
   
   /* 创建新的函数 nextNumber1，并查看结果 */
   nextNumber1 := getSequence()  
   fmt.Println(nextNumber1())
   fmt.Println(nextNumber1())
}
```

