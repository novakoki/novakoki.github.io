---
title: 在快速排序中选择好的枢纽点
date: 2015-02-13
locale: zh_CN
---

快速排序是一个典型的分治(DIVIDE-AND-CONQUER)算法.


```cpp
QuickSort(array A,length n)
    if n == 1  return
    pivot = ChoosePivot(A,n)
    Partition A around pivot
    Recursively sort 1st part
    Recursively sort 2nd part
```




而决定这个算法效率的关键就在于枢纽点(pivot)的选取，因为这直接影响到分割(partition)的好坏。最坏情况下，即每次选取的枢纽点都为最大或最小的数，则时间复杂度为 $\theta(n^2)$ .而如果能够奇迹般地每次都选择了中位数，时间复杂度就为 $\theta(n \log n)$ .以上都为理想情况的分析，而对于排序来说，效率的衡量在于比较次数。以下对各种枢纽点的选取方法做一个比较次数的测量，数据为10000个0~9999之间的整数。



1. 选取首尾元素
> 首：162085次比较
> 尾：164123

这种方法的效率取决于输入数据的预排序程度，如果是已经有序的状态，则效率很低

2. 三数中值分割法，即选择下标为第一个、最后一个、中间的三个数的中位数作枢纽点
> 138382

效率很高

3. 随机选取枢纽点
> 测试了五次：
> 148958，149598，163443，161712，161399

随机选取枢纽点无疑是安全的，因为不可能每次都选中最坏的情形。但是另一方面，随机数的生成也耗费了时间，对实际排序的时间造成了影响。




---


所用的数据：
<http://spark-public.s3.amazonaws.com/algo1/programming_prob/QuickSort.txt>
参考资料：
<http://www.nowamagic.net/librarys/veda/detail/2397>




---


参考代码：


```cpp
#include<iostream>
#include <fstream>
#include <cstdlib>
#include <ctime>
using namespace std;
int sum = 0;
int pivot(int* A,int lo,int hi){
    //	三数中值分割法
    //	int a = A[lo];
    //	int b = A[hi];
    //	int c = A[(lo+hi)/2];
    //	if(( (a>=b) && (a<=c) ) || ( (a>=c) && (a<=b) ))
    //		return lo;
    //	if(((b>=a)&&(b<=c))||((b>=c)&&(b<=a)))
    //		return hi;
    //	if(((c>=b)&&(c<=a))||((c>=a)&&(c<=b)))
    //		return (lo+hi)/2;	
    //首尾 	
    //return lo;	
    //return hi;	
    //随机 	
    //srand((int)time(0));	
    //return lo + rand()%(hi-lo);}

    void swap(int& a,int& b){	
        int tmp = a;	
        a = b;	
        b = tmp;
    }

    void QuickSort(int* A,int l,int r){	
        if(l == r+1) return;
        //	for(int i = 0;i < 10;i++)
        //		cout<<A[i]<<" ";
        //	cout<<endl;	
        int p = pivot(A,l,r);	
        //cout<<A[p]<<" ";	
        swap(A[l],A[p]);	
        int i = l + 1;	
        for(int j = l + 1;j <= r;j++){		
            if(A[j] < A[l]){			
                swap(A[j],A[i]);			
                i++;		
            }		
            sum++;	
        }	
        swap(A[l],A[i-1]);	
        QuickSort(A,l,i-2);	
        QuickSort(A,i,r);
    }
```

