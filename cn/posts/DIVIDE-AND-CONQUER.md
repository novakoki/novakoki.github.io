---
title: 分治法
date: 2015-02-13
locale: zh_CN
---

## 理论

### 流程

1. 分割成很多个同类型的子问题(**Divide**)
2. 递归地解决这些子问题(**Conquer**)
3. 合并子问题的答案(**Combine**)



### 主序定理(The Master Method)


对于递推式：
$$T(n)=aT(n/b)+O(n^d)$$
有如下结论：


$$T(n)=\begin{cases}
&O(n^d)&\text{if}&d\gt\log_ba\\
&O(n^d\log n)&\text{if}&d=\log_ba\\
&O(n^{\log_ba})&\text{if}&d\lt\log_ba
\end{cases}$$


## 经典算法


1. 整数乘法(Karatsuba Multiplication)
2. 归并排序(Mergesort)
3. 逆序数(Counting Inversions)
4. 最近点对(Closest Pair)
5. 矩阵乘法(Strassen’s Subcubic Matrix Multiplication)
6. 快速排序(Quicksort)
7. 选择问题(Selection)
