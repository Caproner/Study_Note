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

![3](F:\github\Study_Note\img\Coursera Machine Learning\3.png)

但如果函数h为逻辑回归函数，则有可能出现函数非凸的情况：

![4](F:\github\Study_Note\img\Coursera Machine Learning\4.png)

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

![5](F:\github\Study_Note\img\Coursera Machine Learning\5.png)

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

