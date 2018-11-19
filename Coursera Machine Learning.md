# Coursera: Machine Learning

## 第1周

### Introduction

#### 机器学习（Machine Learning）的定义

从经验E中学习使得能在任务T中提升完成度P的过程

#### 监督学习（Supervised Learning） vs 无监督学习（Unsupervised Learning）

监督学习为从一些有标签的数据进行学习，然后对后续的数据添加标签

而无监督学习则是给定一些没有标签的数据，让其进行打标签

#### 分类（Classification） vs 回归（Regression）

两者都属于监督学习的范畴

可以简单理解为，分类问题中的标签是离散的，回归问题中的标签是连续的

（或认为回归问题的标签需要连续，例如说假设标签为人民币，人民币事实上可以是离散的（最低单位为1分），但是在大部分问题上该标签需要当做是连续数值来看待）

### Model and Cost Function

#### 问题

假设有一个房价预测问题，需要通过房的面积预测出房的价格

#### 一些约定

这里定义m为数据的组数（也就是行数），那么每组特征都可以表示成一个向量。例如这里的房价预测问题就可以用一个二维向量组表示：
$$
(x^{(i)},y^{(i)});i=1,...,m
$$

#### 解决问题的流程

整体流程如下图所示：

![image](img/Coursera Machine Learning/1.png)

大体思路便是，将训练数据扔进学习算法中，由学习算法生成一个函数h，然后待标记的数据集x经由函数h得到预测结果y

在这里的问题中，我们假设函数h为如下形式：
$$
h_{\theta}(x)={\theta}_0+{\theta}_1x
$$
显然，这是一个线性函数的形式，故我们用于解决这个问题所用的模型就称之为**线性回归**

由于我们的问题只有一个特征，故其称之为**单变量线性回归**

#### 单变量线性回归

单变量线性回归的模型为上述形式：
$$
h_{\theta}(x)={\theta}_0+{\theta}_1x
$$
其中θ0和θ1则是需要求出来的参数

#### 代价函数

现有的问题便是，如何利用当前已知的数据来求地最理想的函数参数

这里采取的指标便是让该函数与数据集的**均方误差和**尽量小：
$$
{\min_{\theta_0,\theta_1}}\sum_{i=1}^{m}(h_{\theta}(x^{(i)}-y^{(i)})^2
$$
于是便可以设代价函数J为如下形式（其中除2m是后续需要）：
$$
J(\theta_0,\theta_1)={\frac {1} {2m}}\sum_{i=1}^{m}(h_{\theta}(x^{(i)})-y^{(i)})^2
$$
那么问题便转换为求函数J的最小值

### Parameter Learning

#### 梯度下降法

梯度下降法的大体思路是：

+ 任意选取参数初始值
+ 重复该步骤直至参数收敛
  + 沿着当前所在的点的梯度最小的方向**同步**更新所有参数

显然，梯度下降法会找到函数的某一个极小值

假设需要用梯度下降法求的函数为：
$$
J(\theta_0,\theta_1,...,\theta_n)
$$
那么每遍迭代都可以写为如下形式：
$$
{\theta_i}:={\theta_i}-\alpha{\frac {\partial}{\partial\theta_i}}J(\theta_0,\theta_1,...,\theta_n)
$$

#### 学习率

迭代过程中的alpha即为**学习率**

学习率的大小与单次梯度下降对参数的改变的大小成正比

如果学习率太小则会出现迭代次数过多的情况

而如果学习率太大则会出现单次迭代越过最低点，甚至无法收敛的情况

#### 针对单变量线性回归的梯度下降法

对于单变量线性回归而言，其代价函数是凸的，也就是说对其运用梯度下降法可以求得全局最优解

其每次迭代需要做的更新为：
$$
\begin{cases}
\theta_0:=\theta_0-
\alpha\frac{\partial}{\partial\theta_0}
({\frac {1} {2m}}\sum_{i=1}^{m}({\theta}_0+{\theta}_1x^{(i)}-y^{(i)})^2) 
\\ 
\theta_1:=\theta_1-
\alpha\frac{\partial}{\partial\theta_1}
({\frac {1} {2m}}\sum_{i=1}^{m}({\theta}_0+{\theta}_1x^{(i)}-y^{(i)})^2)
\end{cases}
$$
求偏微分后为：
$$
\begin{cases}
\theta_0:=\theta_0-
{\frac {\alpha} {m}}
\sum_{i=1}^{m}({\theta}_0+{\theta}_1x^{(i)}-y^{(i)})
\\ 
\theta_1:=\theta_1-
{\frac {\alpha} {m}}
\sum_{i=1}^{m}(({\theta}_0+{\theta}_1x^{(i)}-y^{(i)})x^{(i)})
\end{cases}
$$

### Linear Algebra Review (Optional)

+ 向量（Vector）：可以看做是一个n*1的矩阵（本来就是）

+ 在遇到需要多重for循环计算多个结果的时候，要尽量转换为矩阵乘法
  + 因为一般语言内会封装关于矩阵乘法的诸多优化，使得其比单纯for循环要快的多
+ 只有n*n的矩阵才会有逆矩阵

## 第2周

### Multivariate Linear Regression

#### 问题

假设我们在房价预测的问题中，可以参考的不只是房子的面积，还有其他的因素，例如说浴室个数、房间个数、有没有空调、等等。那么特征便有了很多个。

在这种情况下做线性回归的话，得到的结果便不是一条直线，而是一个**超平面**

此时的模型便称为**多变量线性回归**

#### 一些约定

这里定义n为特征数量，那么第i行数据的第j个特征则写为：
$$
x^{(i)}_j
$$

#### 多变量线性回归

类比单变量线性回归的模型，多变量线性回归的式子则为：
$$
h_{\theta}(x)=\theta_0+\theta_1x_1+\theta_2x_2+...+\theta_nx_n
$$
在这里做一些约定：
$$
\begin{cases}
x_0=1
\\
x=
\begin{bmatrix}
x_0\\x_1\\\vdots\\x_n
\end{bmatrix}
,
\theta=
\begin{bmatrix}
\theta_0\\\theta_1\\\vdots\\\theta_n
\end{bmatrix}
\end{cases}
$$
那么，多变量线性回归便可以简写为如下形式：
$$
h_\theta(x)=\theta^Tx
$$

#### 多变量线性回归的梯度下降法

首先仍旧需要类比单变量线性回归拟定一个代价函数：
$$
J(\theta)={\frac {1} {2m}}\sum_{i=1}^{m}(h_{\theta}(x^{(i)})-y^{(i)})^2
$$
然后定义迭代的更新步骤：
$$
\theta_j:=\theta_j-
{\frac {\alpha} {m}}
\sum_{i=1}^{m}((h_{\theta}(x^{(i)})-y^{(i)})x^{(i)}_j)
$$
显然这跟之前的更新步骤是类似的

#### 归一化

假设有两个特征，一个的取值范围为[-0.1,0.1]，另一个是[0,100000]。那么在梯度下降的时候必然会出现迭代次数过多的情况

一般而言，我们需要将特征进行归一化：

+ 将范围缩放至[-1,1]之间（允许个位数常数级的偏差，例如[-3,4]也是可以的）
+ 令其平均数为0（总体均减去平均数即可）
  + 或者更统计点的，令其标准差为0

#### 多项式回归

多项式回归事实上就是将假设的回归函数定义为多项式而已

事实上多项式回归可以看做是增加了特征的线性回归

例如说如下形式：
$$
h_\theta(x)=\theta_0+\theta_1x+\theta_2x^2+\theta_3x^3
$$
只要假设：
$$
\begin{cases}
x_1=x
\\
x_2=x^2
\\
x_3=x^3
\end{cases}
$$
就可以转换为线性回归问题

### Computing Parameters Analytically

#### 正规方程

假设：
$$
\begin{cases}
x^{(i)}_0=1
\\
x^{(i)}=
\begin{bmatrix}
x^{(i)}_0\\x^{(i)}_1\\\vdots\\x^{(i)}_n
\end{bmatrix}
,
\theta=
\begin{bmatrix}
\theta_0\\\theta_1\\\vdots\\\theta_n
\end{bmatrix}
,
y=
\begin{bmatrix}
y_1\\y_2\\\vdots\\y_m
\end{bmatrix}
\\
X=
\begin{bmatrix}
x^{(1)}\\x^{(2)}\\\vdots\\x^{(m)}
\end{bmatrix}
\end{cases}
$$
于是我们可以得出求向量θ的正规方程为：
$$
\theta=(X^TX)^{-1}X^Ty
$$
正规方程的来源事实上跟解一元二次函数的零点的正规方程是一样的

不过这里有一个并不是采用上述思想的，并不严谨的推导方式：

首先根据上面的假设，我们可以知道我们最终需要求的θ必须尽可能满足下述形式：
$$
X\theta=y
$$
两边各自左乘：
$$
(X^TX)^{-1}X
$$
于是就可以得到：
$$
(X^TX)^{-1}(X^TX)\theta=(X^TX)^{-1}X^Ty
$$
等式左边那一坨抵消之后只剩θ，证毕

不严谨的原因是，根据线性回归的模型，当m>=n时候，Xθ=y是无解的

而一般情况下数据组数会远远大于特征数，也就是说m>=n几乎是必定会出现的

#### 正规方程 vs 梯度下降

|             梯度下降             |       正规方程        |
| :------------------------------: | :-------------------: |
|       需要选择学习率alpha        | 不需要选择学习率alpha |
|           需要多次迭代           | 不需要迭代，一次求解  |
| 时间复杂度为O(kn^2)，k为迭代次数 |  时间复杂度为O(n^3)   |
|            需要归一化            |     不需要归一化      |

#### 正规方程中矩阵不可逆的情况

实际上在上述式子中，(X^T)X本身存在不可逆的情况（也就是奇异矩阵的情况）

一般而言有一下两种情况会使得其不可逆：

+ 存在线性相关的变量：例如说一个特征是公里数，另一个特征是英里数
  + 遇到这种情况就删掉一个特征就行了
+ 出现m<n的情况（从矩阵上讲这也算是一种线性相关）
  + 遇到这种情况，要么增加数据，要么删掉一些特征，要么降维

不过一般求矩阵的逆的时候使用的是求其**伪逆矩阵**，所以即使是不可逆的矩阵也是可以求逆的

## 第3周

### Classification and Representation

#### 逻辑回归

逻辑回归用于解决二分类问题。其函数形式为：
$$
h_\theta(x)={\frac 1 {1+e^{-z}}}
$$
其中z为线性回归函数：
$$
z=\theta^Tx
$$
其原型函数为：
$$
g(x)={\frac 1 {1+e^{-x}}}
$$
其图像为：

![2](img/Coursera Machine Learning/2.png)



故其值在大部分情况下要么趋近于0，要么趋近于1。故其适合用于二分类问题

而在这里逻辑回归函数的值表示的意义为，当值为x的时候，y=1的概率

#### 决策边界

事实上令函数z=0所表示的曲线便是决策边界

将逻辑回归的结果中的z以z=0的形式画出来，可以得到一个曲线，在曲线的一侧(z>0)便是y=1的决策，另一侧(z<0)便是y=0的决策

### Logistic Regression Model

#### 代价函数

在线性回归中使用的代价函数为：
$$
J(\theta)={\frac {1} {2m}}\sum_{i=1}^{m}(h_{\theta}(x^{(i)})-y^{(i)})^2
$$
其可以写作如下形式：
$$
\begin{cases}
J(\theta)={\frac {1} {m}}\sum_{i=1}^{m}cost(h_\theta(x^{(i)}),y^{(i)})
\\
cost(h_\theta(x),y)={\frac {1} {2}}(h_{\theta}(x)-y)^2
\end{cases}
$$
此时函数J可以看做是求cost函数的平均数，而cost函数求的便是平方误差

（以下情况均为只有一个theta且x和y为随机生成（但保证y必定为0或1）的情况下的theta-J曲线）

如果是在线性回归的情况的话，对其使用梯度下降是可行的，因为其函数是凸的：

![3](img\Coursera Machine Learning\3.png)

但如果函数h为逻辑回归函数，则有可能出现函数非凸的情况：

![4](img\Coursera Machine Learning\4.png)

可以看出，右边那块单调递减，如果梯度下降的起点在10这个位置的话，则梯度下降会滑向右边

所以需要修改cost函数为如下形式：
$$
cost(h_\theta(x),y)=
\begin{cases}
-\log(h_\theta(x)), & \text {if y=1}
\\
-\log(1-h_\theta(x)), & \text {if y=0}
\end{cases}
$$
或者直接简写成如下形式：
$$
cost(h_\theta(x),y)=-y\log(h_\theta(x))-(1-y)\log(1-h_\theta(x))
$$
此时的函数图像近似于线性回归的梯度下降的图像：

![5](img\Coursera Machine Learning\5.png)

于是该函数便可以用于逻辑回归的梯度下降

于是完整的代价函数J为：
$$
\begin{align}
J(\theta)&={\frac {1} {m}}\sum_{i=1}^{m}(-y^{(i)}\log(h_\theta(x^{(i)}))-(1-y^{(i)})\log(1-h_\theta(x^{(i)})))
\\
&=-{\frac {1} {m}}\sum_{i=1}^{m}(y^{(i)}\log(h_\theta(x^{(i)}))+(1-y^{(i)})\log(1-h_\theta(x^{(i)})))
\end{align}
$$

#### 梯度下降

类比线性回归，逻辑回归的梯度下降法也是一样的：
$$
{\theta_j}:={\theta_j}-\alpha{\frac {\partial}{\partial\theta_j}}J(\theta)
$$
现在对J求导：

首先，有：
$$
\begin{align}
\log(1-h_\theta(x^{(i)}))&=\log({\frac {e^{-\theta^Tx^{(i)}}} {1+e^{-\theta^Tx^{(i)}}}})
\\
&=-\theta^Tx^{(i)}+\log(h_\theta(x^{(i)}))
\end{align}
$$
故可以将J简化如下：
$$
\begin{align}
J(\theta)
&=-{\frac {1} {m}}\sum_{i=1}^{m}(y^{(i)}\log(h_\theta(x^{(i)}))+(1-y^{(i)})\log(1-h_\theta(x^{(i)})))
\\
&=-{\frac {1} {m}}\sum_{i=1}^{m}(y^{(i)}\log(h_\theta(x^{(i)}))+(1-y^{(i)})(-\theta^Tx^{(i)}+\log(h_\theta(x^{(i)}))))
\\
&=-{\frac {1} {m}}\sum_{i=1}^{m}(y^{(i)}\log(h_\theta(x^{(i)}))+(-\theta^Tx^{(i)}+\log(h_\theta(x^{(i)})))-y^{(i)}(-\theta^Tx^{(i)}+\log(h_\theta(x^{(i)}))))
\\
&=-{\frac {1} {m}}\sum_{i=1}^{m}((y^{(i)}-1)\theta^Tx^{(i)}+\log(h_\theta(x^{(i)})))
\end{align}
$$
为了简化后续的计算，先对函数h求导：
$$
\begin{align}
{\frac {\partial}{\partial\theta_j}}h_\theta(x)
&={\frac {\partial}{\partial\theta_j}}{\frac {1} {1+e^{-\theta^Tx}}}
\\
&=-{\frac {1} {(1+e^{-\theta^Tx})^2}}\cdot{\frac {\partial}{\partial\theta_j}}{(1+e^{-\theta^Tx})}
\\
&=-{\frac {e^{-\theta^Tx}} {(1+e^{-\theta^Tx})^2}}\cdot{\frac {\partial}{\partial\theta_j}}{(-\theta^Tx)}
\\
&={\frac {e^{-\theta^Tx}} {(1+e^{-\theta^Tx})^2}}\cdot{x_j}
\\
&=h_\theta(x)\cdot(1-h_\theta(x))\cdot{x_j}
\end{align}
$$
于是：
$$
\begin{align}
{\frac {\partial}{\partial\theta_j}}J(\theta)
&=-{\frac {1} {m}}\sum_{i=1}^{m}{\frac {\partial}{\partial\theta_j}}((y^{(i)}-1)\theta^Tx^{(i)}+\log(h_\theta(x^{(i)})))
\\
&=-{\frac {1} {m}}\sum_{i=1}^{m}({\frac {\partial}{\partial\theta_j}}(y^{(i)}-1)\theta^Tx^{(i)}+{\frac {\partial}{\partial\theta_j}}\log(h_\theta(x^{(i)})))
\\
&=-{\frac {1} {m}}\sum_{i=1}^{m}((y^{(i)}-1)x^{(i)}_j+{\frac {{\frac {\partial}{\partial\theta_j}}h_\theta(x^{(i)})} {h_\theta(x^{(i)})}})
\\
&=-{\frac {1} {m}}\sum_{i=1}^{m}((y^{(i)}-1)x^{(i)}_j+(1-h_\theta(x^{(i)}))\cdot{x^{(i)}_j})
\\
&={\frac {1} {m}}\sum_{i=1}^{m}((h_\theta(x^{(i)})-y^{(i)}){x^{(i)}_j})
\end{align}
$$
最终会得到一个很有趣的结论：在无视函数h自身的计算的情况下，逻辑回归的梯度下降跟线性回归是完全一致的
$$
\theta_j:=\theta_j-
{\frac {\alpha} {m}}
\sum_{i=1}^{m}((h_{\theta}(x^{(i)})-y^{(i)})x^{(i)}_j)
$$
虽然事实上是为了令其一致而构造出了新的cost函数

#### 高级优化算法

事实上，求代价函数的最小值有比梯度下降更为优秀的算法，但是其实在是过于复杂，而且在Octave中有对应的库可以调用，所以只需要知道怎么用即可

要用这个，首先需要写一个代价函数：

```octave
function [jVal, gradient] = costFunction(theta)
  jVal = [...code to compute J(theta)...];
  gradient = [...code to compute derivative of J(theta)...];
end
```

其中，`theta`便是参数，`jVal`指的是J(θ)的值，而gradient则是梯度向量，也就是对于每个参数的偏导组成的向量

然后在主程序中只需要做如下调用：

```octave
options = optimset('GradObj', 'on', 'MaxIter', 100);
initialTheta = zeros(2,1);
[optTheta, functionVal, exitFlag] = fminunc(@costFunction, initialTheta, options);
```

其中`options`为一些配置选项，返回值中：

+ `optTheta`为最终收敛的参数向量
+ `functionVal`为该向量计算出来的J(θ)
+ `exitFlag`表示是否收敛

### Multiclass Classification

简单的讲，对于k分类问题，可以视为k个二分类问题。

假设现在做的是四分类问题，那么就需要4个二分类模型，第i个二分类模型将第i类视为正例，其他视为反例

于是就需要做k次逻辑回归

### Solving the Problem of Overfitting

#### 过拟合

简单地说，当模型在训练集上表现很好，但是在验证集上表现很差的时候，就说明其可能存在过拟合的现象

避免过拟合的两个主要的方法：

1. 减少特征：手动选择哪些特征要保留，或者使用模型选择算法（后续会讲）

2. 正则化：保留所有特征，但减少参数θ的大小

#### 正则化

正则化的思路便是减少参数θ的大小，使得函数尽可能光滑，从而使函数更不易过拟合

于是就需要引进正则项加入代价函数，使得代价函数能够惩罚参数：
$$
{\frac{\lambda}{2m}}\sum_{j=1}^{n}\theta_{j}^2
$$
其中λ为正则化参数

在引入正则项之后的代价函数则为（以线性回归为例）：
$$
J(\theta)={\frac {1} {2m}}[\sum_{i=1}^{m}(h_{\theta}(x^{(i)})-y^{(i)})^2+\lambda\sum_{j=1}^{n}\theta_{j}^2]
$$
注意到正则项中并没有对θ0进行惩罚，因为其仅仅是偏置项，惩罚该参数对减少函数过拟合没有作用

对于正则化参数λ，显然的：

+ 当λ过大时，惩罚力度太大，会使参数更容易趋近于零，于是会发生欠拟合的现象
+ 当λ过小时，正则项起到的作用微乎其微，于是该过拟合的还是会过拟合

#### 线性回归中的正则化

线性回归中有两种方法可以拟合模型：梯度下降和正规方程。

首先是梯度下降，事实上只需要按照梯度下降的方法对函数J求偏导就行了。

于是显然的，就可以得到如下迭代更新步骤：
$$
\begin{cases}
\theta_0:=\theta_0-
{\frac {\alpha} {m}}
\sum_{i=1}^{m}((h_{\theta}(x^{(i)})-y^{(i)})x^{(i)}_0)
&
\\
\theta_j:=\theta_j-
{\frac {\alpha} {m}}
[\sum_{i=1}^{m}((h_{\theta}(x^{(i)})-y^{(i)})x^{(i)}_j)+\lambda\theta_j]
& \text{if j>0}
\end{cases}
$$
第二条式子可以写作如下形式：
$$
\theta_j:=\theta_j(1-\alpha{\frac{\lambda}{m}})-{\frac {\alpha} {m}}\sum_{i=1}^{m}((h_{\theta}(x^{(i)})-y^{(i)})x^{(i)}_j)
$$
这样就可以看做是每次迭代先对参数θ进行缩小（1-aλ/m<1）再进行原先的梯度下降

其次是正规方程。只需要改动一个地方即可：
$$
\theta=(X^TX+A)^{-1}X^Ty
$$
其中A为(n+1)*(n+1)的矩阵：
$$
A=\lambda
\begin{bmatrix}
0 & 0 & 0 & \cdots & 0 \\
0 & 1 & 0 & \cdots & 0 \\
0 & 0 & 1 & \cdots & 0 \\
\vdots & \vdots &\vdots & \ddots & \vdots \\
0 & 0 & 0 & \cdots & 1 \\
\end{bmatrix}
$$
由于X^TX+A必定有逆矩阵，故不需要讨论其不可逆的情况

#### 逻辑回归中的正则化

显然的，其代价函数后面也只需要补上正则项即可：
$$
J(\theta)=-{\frac {1} {m}}\sum_{i=1}^{m}(y^{(i)}\log(h_\theta(x^{(i)}))+(1-y^{(i)})\log(1-h_\theta(x^{(i)})))+{\frac{\lambda}{2m}\sum_{i=1}^{n}}\theta_i^2
$$
于是梯度下降的过程也是类似的：
$$
\begin{cases}
\theta_0:=\theta_0-
{\frac {\alpha} {m}}
\sum_{i=1}^{m}((h_{\theta}(x^{(i)})-y^{(i)})x^{(i)}_0)
&
\\
\theta_j:=\theta_j(1-\alpha{\frac{\lambda}{m}})-{\frac {\alpha} {m}}\sum_{i=1}^{m}((h_{\theta}(x^{(i)})-y^{(i)})x^{(i)}_j)
& \text{if j>0}
\end{cases}
$$

## 第4周

### Motivations

#### 多特征的情况

一般来说，想要得到一个比较好一点的曲线的话，一般我们会使用二次或三次的多项式回归（不管是线性回归还是逻辑回归都是）

于是，假设有n个特征，则需要造出O(n^2)甚至O(n^3)的特征数量

如果n比较小的话尚能在线性回归和逻辑回归的接受范围之内，但是如果n稍微大一点（例如1000），可能就无法承受如此庞大数量的特征了

而特征数量爆炸在很多情况下都是比较常见的（例如说计算机视觉，每张图片都是有数量巨大的像素，每个像素值都是一个特征），在这种情况下线性回归或者逻辑回归就不适用了

### Neural Networks

#### 神经元

神经元为一个神经网络的单位，也叫作逻辑单元（激励单元）：

![6](img\Coursera Machine Learning\6.png)

每个逻辑单元事实上可以看做是一个逻辑回归

其中x0为偏置单元，默认为1

#### 神经网络模型

而神经网络模型事实上就是由多个神经元拼起来的模型：

![7](img\Coursera Machine Learning\7.png)

其中第一层为输入层，第二层为隐藏层，第三层为输出层

除了输出层之外每一层都会有一个偏置单元

于是此时参数便不再只是一个向量，而是扩张到一个矩阵Θ

对于每一层的Θ均由该层的每个θ^T从上往下拼接而成

对于上图的三层模型而言，一共有两个矩阵Θ（当然，第二个Θ仅仅是向量规模）

于是，该神经网络的计算模式为：
$$
\begin{align*} a_1^{(2)} = g(\Theta_{10}^{(1)}x_0 + \Theta_{11}^{(1)}x_1 + \Theta_{12}^{(1)}x_2 + \Theta_{13}^{(1)}x_3) \newline a_2^{(2)} = g(\Theta_{20}^{(1)}x_0 + \Theta_{21}^{(1)}x_1 + \Theta_{22}^{(1)}x_2 + \Theta_{23}^{(1)}x_3) \newline a_3^{(2)} = g(\Theta_{30}^{(1)}x_0 + \Theta_{31}^{(1)}x_1 + \Theta_{32}^{(1)}x_2 + \Theta_{33}^{(1)}x_3) \newline h_\Theta(x) = a_1^{(3)} = g(\Theta_{10}^{(2)}a_0^{(2)} + \Theta_{11}^{(2)}a_1^{(2)} + \Theta_{12}^{(2)}a_2^{(2)} + \Theta_{13}^{(2)}a_3^{(2)}) \newline \end{align*}
$$
该计算模式称为**前向传播算法**

#### 前向传播算法的向量化计算

首先，假设一共有k层，并假设：
$$
a^{(1)}=x
\\
a^{(k)}=h_\Theta(x)
$$
于是其计算过程便可以简化为：
$$
\text {for each i in [1,k-1]:}
\\
\begin{cases}
a^{(i)}_0=1
\\
a^{(i+1)}=g(\Theta^{(i)}a^{(i)})
\end{cases}
$$
其中函数g为逻辑回归中的原型函数：
$$
g(x)={\frac 1 {1+e^{-x}}}
$$

### Applications

#### 多分类问题

对于多分类问题，要么就套用逻辑回归的思路，建立多个神经网络，要么就将神经网络的输出层设为k个而不是1个，于是输出便不再是一个数，而是一个向量，用于表示其属于某个分类的概率：

![8](img\Coursera Machine Learning\8.png)

## 第5周

### Cost Function and Backpropagation

#### 一些约定

+ `L`：神经网络的总层数
+ `s_l`：第`l`层的逻辑单元数量（不包括偏置单元）
+ `K`：输出层的逻辑单元数

#### 代价函数

神经网络的代价函数如下：
$$
\begin{gather*} J(\Theta) = - \frac{1}{m} \sum_{i=1}^m \sum_{k=1}^K \left[y^{(i)}_k \log ((h_\Theta (x^{(i)}))_k) + (1 - y^{(i)}_k)\log (1 - (h_\Theta(x^{(i)}))_k)\right] + \frac{\lambda}{2m}\sum_{l=1}^{L-1} \sum_{i=1}^{s_l} \sum_{j=1}^{s_{l+1}} ( \Theta_{j,i}^{(l)})^2\end{gather*}
$$
这里主要分为两项：

+ 代价函数本体

  区别于逻辑回归的代价函数有两点：

  + 由于神经网络在输出层可能有多个节点（也就是有多个结果），所以需要将每个结果单独看成一个函数来进行逻辑回归的代价函数计算
  + 在这里函数h不再是逻辑回归函数的计算结果，而是神经网络经过层层前项传播得到的最终结果，其中h(x)_k为输出层的第k个输出结果

+ 正则项

  事实上就是把所有的θ（偏置项除外）的平方加起来，再乘上λ/2m

#### 反向传播算法

大约从此处开始，很多算法将会不加证明的给出。因为证明实在是太难了。

首先，对于一个及其学习模型，其最终目的是要通过数据训练来拟合数据。

目前以来使用的思路均为，求出代价函数，并使用梯度下降或高级的优化算法令其尽可能最小化。

于是每一步迭代均需要求出各个θ对代价函数J的偏导，也就是：
$$
{\frac {\partial} {{\partial}\Theta^{(l)}_{ij}}}J(\Theta)
$$
由于神经网络本身十分复杂，故我们无法直接求出其偏导，而是需要使用反向传播算法来求出其偏导。

反向传播算法的步骤如下：

+ 假设训练集为：
  $$
  \{(x^{(1)},y^{(1)}),\cdots,(x^{(m)},y^{(m)})\}
  $$







+ 令：
  $$
  \Delta^{(l)}_{ij}=0\ {\text {for all i,j,l}}
  $$







+ 依次扫描每组数据（for i from 1 to m）：

  + 令：
    $$
    a^{(1)}=x^{(i)}
    $$







  + 使用前向传播算法计算出所有的a，得到当前模型对训练数据x^{(i)}的预测值

  + 反向计算误差值δ：

    + 令：
      $$
      \delta^{(L)}=a^{(L)}-y^{(i)}
      $$

    + 使用如下式子依次计算δ^{(L-1)}，δ^{(L-2)}，。。。，δ^{(2)}（注意不需要计算δ^{(1)}）：
      $$
      \delta^{(l)}=((\Theta^{(l)})^T\delta^{(l+1)}).*a^{(l)}.*(1-a^{(l)})
      $$





  + 更新Δ：
    $$
    \Delta^{(l)}:=\Delta^{(l)}+\delta^{(l+1)}(a^{(l)})^T
    $$







+ 令：
  $$
  \begin{cases}
  D^{(l)}_{ij}:={\frac 1 m}\Delta^{(l)}_{ij}+\lambda\Theta^{(l)}_{ij} & {\text {if j≠0}}
  \\
  D^{(l)}_{ij}:={\frac 1 m}\Delta^{(l)}_{ij} & {\text {if j=0}}
  \end{cases}
  $$







+ 于是就可以得出：
  $$
  {\frac {\partial} {{\partial}\Theta^{(l)}_{ij}}}J(\Theta)=D^{(l)}_{ij}
  $$






之后，每次迭代均利用反向传播算法求偏导，然后使用梯度下降或者是更高级的优化算法，便可让代价函数收敛到全局最小值

### Backpropagation in Practice

#### Octave 中使用高级优化算法迭代神经网络

步骤基本跟线性回归或逻辑回归一致，不过需要注意的是，函数`fminunc`只能接收一个矩阵作为初始参数，以及只能返回一个矩阵作为梯度，但是在多层神经网络中可能会有多个矩阵作为参数，同样的也会有多个矩阵作为梯度。这个时候就需要对矩阵们进行向量表示和重组。

将多个矩阵展开成一个长向量的方式为：

```octave
thetaVector = [ Theta1(:); Theta2(:); Theta3(:); ]
deltaVector = [ D1(:); D2(:); D3(:) ]
```

将长向量重组成多个矩阵的方式为：

```octave
Theta1 = reshape(thetaVector(1:110),10,11)
Theta2 = reshape(thetaVector(111:220),10,11)
Theta3 = reshape(thetaVector(221:231),1,11)
```

#### 梯度检测

为了确定反向传播算法是否真的是能求出梯度，这个时候需要对反向传播算法求出的梯度进行梯度检测。

事实上就是验证下式是否成立：
$$
{\frac {\partial} {{\partial}{\theta}}}J(\theta)\approx{\frac {J(\theta+\epsilon)-J(\theta-\epsilon)} {2\epsilon}}
$$
注意，在训练模型的时候需要关掉梯度检测，因为梯度检测计算梯度会很慢。

#### 随机初始化

按照之前线性回归和逻辑回归，参数θ在初始化的时候全部置为0。但是在神经网络中这么做会使其同一层的所有神经元参数不管怎么迭代都是完全一致的。

所以在神经网络中，所有的参数的初始值应该为一个相互尽量不同但又接近零的随机数。（范围在[-ε,+ε]内）

```octave
% If the dimensions of Theta1 is 10x11, Theta2 is 10x11 and Theta3 is 1x11.

Theta1 = rand(10,11) * (2 * INIT_EPSILON) - INIT_EPSILON;
Theta2 = rand(10,11) * (2 * INIT_EPSILON) - INIT_EPSILON;
Theta3 = rand(1,11) * (2 * INIT_EPSILON) - INIT_EPSILON;
```

## 第6周

### Evaluating a Learning Algorithm

#### 检验误差

比较通用的方法是，将手头上的数据按照7:3的比例，分成训练集和验证集。

于是误差便可以分为训练集误差和验证集误差。

在回归问题中，误差可以直接使用代价函数来代替。

而在逻辑回归中，有一种比代价函数更加直接的方法就是直接计算正确率。

