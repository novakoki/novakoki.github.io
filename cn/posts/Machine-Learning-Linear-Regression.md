---
title: "Machine Learning : Linear Regression"
date: 2015-02-07
locale: zh_CN
---

线性回归即用线性函数对因变量和一个或多个自变量之间的关系进行建模。这个函数是多个称为回归系数的参数的线性组合。


给定数据集（n个变量，m组数据）


用矩阵来表示数据集

$$
X=
\begin{pmatrix}
1&x_{11}&x_{21}&\cdots&x_{n1}\\
\vdots&\vdots&\vdots&\ddots&\vdots\\
1&x_{1m}&x_{2m}&\cdots&x_{nm}
\end{pmatrix}
$$


$$
Y=
\begin{pmatrix}
y_1\\
\vdots\\
y_m
\end{pmatrix}
$$
用最小二乘法，即要寻找一个函数
$$h_\theta(x)=x\theta$$
其中$\theta$ 为参数，$\theta=\begin{pmatrix}
\theta_0\\
\theta_1\\
\vdots\\
\theta_m
\end{pmatrix}$ ,
$h_{\theta}(X)$ 叫做Hypothesis


使得 $$J(\theta)=\frac1{2m}\sum_{i=1}^{m}{(h_{\theta}(x^{(i)})-y_i)^2}$$ 最小，此处 $$x^{(i)}=\begin{pmatrix}x_{i1}&x_{i2}&\cdots&x_{in}\end{pmatrix}$$，$J(\theta)$叫做Cost Function




---


### 非迭代法(Normal Equation)
$$\theta=(X^T X)^{-1}X^TY$$


* 可以不用考虑数据的scale，即每个变量取值范围之间的差距
* 大量矩阵运算，尤其是求逆运算复杂度较高，不适于大量数据，$O(n^3)$
* $X^TX$可能出现奇异的情况，原因：
	1. 自变量太多而数据不够，即 $m\lt n$，可以考虑删除一些
	2. 自变量冗余，即有两个或以上变量线性相关


### 迭代法(梯度下降，Gradient Descent)
$$Repeat:{\
\theta_j:=\theta_j-\alpha\frac1m\sum_{i=1}^m{(h_{\theta}(x^{(i)})-y_i)^2}x_j^{(i)}\
}$$


* 需要判断算法是否已经正确执行，即如何确定稳定。可以观察$J(\theta)$图像，也可以给定一个值，当迭代后的减少量小于该值就认为迭代结束
* 要考虑数据的scale，否则出现梯度下降困难。可以把每个自变量的取值范围都统一到一定的区间内，如：
$$x_i:=\frac{x_i-\mu_i}{S_i}$$
使得$-1\lt x_i\lt1$，其中$\mu_i$为均值，$S_i$为取值范围
* 要考虑速率 $\alpha$，太大容易出现迭代后增加而非减少，太小则运行时间延长。可以尝试从小的值逐渐增大并观察$J(\theta)$图像变化
