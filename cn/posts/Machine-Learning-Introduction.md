---
title: "Machine Learning : Introduction"
date: 2015-02-07
locale: zh_CN
---
定义
--

Old



> Field of study that gives computers the ability to learn without being explicitly programmed
>  —— Arthur Samuel (1959)


Mordern



> A computer program is said to learn from experience E with respect to some class of tasks T and performance measure P, if its performance at tasks in T, as measured by P, improves with experience E
> —— Tom Mitchell (1998)



算法分类
----

1. 监督学习(Supervised Learning)
 特点：”Right answers given”
 即有训练集(training set)，包含输入和期望的输出
	* **回归 (Regression)**
	 Predict **continous** valued output
	* **分类 (Classification)**
	 **Discreted** valued output（判断是或不是）
2. 非监督学习(Unsupervised Learning)
 即没有训练集，譬如典型的根据相似度进行分类，但并不知道某一类是什么
3. 其他还有增强学习(Reinforcement Learning), 推荐系统(Recommender Systems)
