---
title: Key Algorithms
date: 2016-07-13 21:59:47
tags: 算法总结
---

[TOC]

# Quick Sort
- explain:    
Set a pivot point in the array,all bigger than pivot to the right and all smaller than pivot to the left.divide the array through the pivot point into two subarray and do the sorting recursively.

The steps are:
1 Pick an element, called a pivot, from the array.
2 Partitioning: reorder the array so that all elements with values less than the pivot come before the pivot, while all elements with values greater than the pivot come after it (equal values can go either way). After this partitioning, the pivot is in its final position. This is called the partition operation.
3 Recursively apply the above steps to the sub-array of elements with smaller values and separately to the sub-array of elements with greater values.

- 分治法，快速排序

数组中找一个中心点，把大于中心点的放在右，小于中心点的放在左。在中心点左右两个子数组中迭代的使用快速排序再排列。伪代码给出的实现方法用双指针法，把数组最右边元素当做中心点，前一个指针遍历数组，后一个指针纪录比中心点小的元素子列头位置，一旦前面指针遍历到有比中心点小的元素，则后面指针自增一并与前指针交换元素，形成前大于中心点子数组和后小于中心点子数组。最后把最右边的中心点与后指针加一位置元素交换再对两个子数组迭代。

步骤：
1 选中心点，一般为数组结尾元素。
2 分隔：重新调整数组，使比中心点小的元素在数组左边，比中心点大的元素在数组右边。分隔之后中心点在两个子数组中间。
3 在比中心点小的元素子数组和比中心大的子数组上迭代以上步骤，直到排序完成。

- pseudo code:
```python
QuickSort(Array A,fistIndex p,lastIndex r) //p and r represent the range to sort
if  p < r:
     q = Partition(A,p,r);
     QuickSort(A,p,q-1);
     QuickSort(A,q+1,r);

Partition(Array A,startIndex p,endIndex r)
x = A[r];
i = p - 1;
for j = p to r -1:
     if A[j] <= x :
          i = i + 1;
          exchange A[i] with A[j]; //if A[j] is smaller than pivot point,put into left subarray
exchange A[i + 1] with A[r]; // after iteration put pivot into where it divide the two subarrays.
return i + 1; // return the index of the pivot point
```
- runtime O( )
worst case: O(n^2);
best case: O(n * log n) or O(n);
average case performance : O(n * log n);
<!--more-->

# Insertion Sort

- explanation:
Iterate from left of the array to the right of the array. For each iteration item, bubble sort left subarray to its  position.In place sort.
插入排序为原位排序，不需要多余空间复杂度。从数组左边遍历到右边，对于每一个遍历元素向左冒泡到其位置。

- pseudo code:
```python
INSERTION-SORT(A)
for j = 1 to A.length - 1:
     key = A[ j ];// insert key to subarray A[0 … j - 1]
     i = j - 1 ;
     while i >= 0  and A[ i ] > key: //bubble sort
          A[i + 1] = A[ i ];     // if not shift right
          i = i - 1;
     A[i + 1] = key; // if yes insert
```

- runtime:          
worst case : O ( n * (n*(n+1))/2) = O(n^2);         
best case : O(n) , with n*(n+1)/2 = 1;            
average case : O (n^2);

# Merge Sort:
Merge Sort algorithm follows the divide-and-conquer paradigm closely.
For each iteration we follow the instructions below:
- Divide:Divide the array into two n/2 subarray.
- Conquer: Sort the two array recursively using merge sort
- Combine: Merge the two sorted subarray

对于每一次迭代，有3步走：

- 切分为两部分，
- 对两部分迭代mergeSort算法
- 把排序过的两部分融合。

通过把数列分解成单个元素，在融合单个元素的过程中在原位排序。

- pseudo code:

```python
MergeSort(A, startIndex, endIndex)      //to initialise the sorting, MergeSort(A,0,A.length)
if startIndex < endIndex :
     midIndex = Math.round((startIndex + endIndex) / 2 ); //divide
     MergeSort(A,startIndex , midIndex);     //conquer
     MergeSort(A,midIndex + 1 , endIndex);
     Merge(A , startIndex, endIndex);     //combine

Merge(A, startIndex , midIndex , endIndex)
n1 = midIndex - startIndex +1;
n2 = endIndex - midIndex ;
let L[n1 + 1],R[n2 + 1] be new arrays;
for i = 0 to n1 - 1:     //from starIndex to midIndex
     L[ i ] = A[startIndex + i ];
for j = 0 to n2 -1:     //from midIndex + 1 to endIndex
     R[ j ] = A[(midIndex + 1) + j ];
L[n1] = infinite;      //here use two infinite so that the for loop below don`t need to decide whether any subarray are iterate out
R[n2] = infinite;
i = 0 ;
j = 0 ;
for k = startIndex to endIndex:
     if L[i] <= R[j]:
          A[k] = L[i];
          i = i + 1;
     else:
          A[k] = R[j];
          j = j + 1;
```

# Binary converter

Convert a Decimal number to binary sequence by applying Decimal-Binary convert algorithm.
```c
void convert(int x){
     if(x / 2 != 0){
          convert(x / 2);     //recursion to the bound
          print("%d",x%2);
     }else{
          print("%d",x);      //print the last bit
     }

}
```

# Tips For XOR(^)
1. to perform NOT(!) on certain bit 
ex. a = 01111010(binary) , b = 00001111(binary);
     c = a^b;
result of c is 01110101, which means a^b performs a NOT operation on last 4bits of a.

2. exchange two values of two variable without defining a temp variable .

```c
template<class T>
void swapNoTemp(T &a,T &b){
     if (a != b){ // if a == b or &a == &b; than no need to swap
          a = a^b;
          b = b^a;
          a = a^b;
     }
}
```
          
# Tips For AND(&) and OR(|)
1. In bit manipulation, `&` AND can be used to set some bits to zero while other bits remain its value. Set the mask byte the bit zero where you want to erase, and set one where you want to maintain its value. 
ex.  n & = 0xffffff00;//set the last 8 bits to zero and others bits remain the same. 

2. Often used to manipulate bits that you can set certain bits to 1 while keep other bits. Set the bits in the mask to 1 where you want to set to 1 in the byte to be set, and set bits in mask 0 to remain the value of corresponding bits in the bits which you want to set.
ex.  n | = 0xff;// set last 8 bits of n to all one

# KMP String Match Algorithm KMP字符串算法
已知，匹配字符串P = p0p1p2……pj-1pj;
目标字符串T = t0t1t2…..tn-1;

- 特征向量N：用了表示模式P字符分布特征,N有时也称next数组
N =    n0n1n2….nm-1 ​;
N[j] 当j=0时N[j] = -1; 其他时候N[j] 等于j位置之前P的收尾最长匹配串长度k(意为，头尾两个可重叠的相等的子串的长度)。

做法：
顺序从头开始对比P和T字符串：
1. 如果出现了不同pj和ti，查表N[j] = k;
2. 用P[k]和当前出错的位置对齐开始比较

KMPMatching c++ 实现：
```c++
int KMPStrMatching(string T, string P, int *N, int start) { 、
    int j= 0;// 模式的下标变量
    int i = start; // 目标的下标变量
    int pLen = P.length( );// 模式的长度
    int tLen = T.length( );  // 目标的长度
    if (tLen - start < pLen)
         return (-1);   // 若目标比模式短，匹配无法成功 
    while ( j < pLen && i < tLen) { // 反复比较，进行匹配
        if ( j == -1 ||T[i] == P[j])
             i++, j++;       
        else j = N[j];    
    }    
    if (j >= pLen)       
        return (i-pLen);    
    else return (-1); 
}
```

求特征向量N(Next)的函数：
```c++
int findNext(string P) {
    int j, k;
    int m = P.length( );    // m为模式P的长度
    int *next = new int[m];  //动态存储区开辟整数数组
    next[0] = -1;  
    j=0; k=-1; 
    while (j < m-1){    //写成   j < m 会越界          
        while (k >= 0 && P[k] != P[j])  //若不等，采用 KMP 找首尾子串
            k = next[k]; // k 递归地向前找
        j++; 
        k++;
    if (P[k] == P[j])            
        next[j] = next[k];       // 前面找 k 值，没有受优化的影响  
    else next[j] = k;            // 取消if判断，则不优化
  }
  return next; 
}         

```
