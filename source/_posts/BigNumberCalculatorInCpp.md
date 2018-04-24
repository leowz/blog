---
title: SomeTricksAndFunctions
date: 2016-10-17 15:40:06
tags: 
---
`17 Oct 2016`

Here are some Tricks and Functions that i find useful.
Content:


# new
new 返回分配的地址，所以用指针赋值。
`int *p = new int；`

## 动态分配二维数组
手动两次分配空间，生成二维数组
```c
 //动态开辟空间  
  int **p = new int*[m]; //开辟行  
  for(int i = 0; i < m; i++)  
       p[i] = new int[n]; //开辟列  
```

## 动态分配三维数组，在类里定义下标访问
```c
template <class T>
class CArray3D{
    public:
        T *** arr ;
        CArray3D(int d1,int d2,int d3){
             arr = new T**[d1];
            for(int i = 0;i < d1;i++){
                 arr[i] = new T*[d2];
                for(int j = 0;j<d2;j++){
                    arr[i][j] = new T[d3];
                }
            }
        }

    T ** operator[](int i){
        return arr[i];
    }
};
```

# is_number: 返回是否string内全是数字

```c++
bool is_number(const std::string& s){return !s.empty() && std::find_if(s.begin(), s.end(), [](char c) { return !std::isdigit(c); }) == s.end();}
```

# getPrimeNum: 求一个整数质因数的数量
```c++
int getPrimeNum(int t){
  int i;
  int n = t;
  set<int> counter;
  printf("%d=",n);
  for(i=2;i<=n;i++)
    while(n!=i)
    {
      if(n%i==0)
      {
        printf("%d*",i);
        counter.insert(i);
        n=n/i;
      }
      else
        break;
    }
  if(n != t){
  printf("%d\n",n);
  counter.insert(n);
  }
  int res = counter.size();
  return res;
}
```

# Combination: 一个简单的组合书实现
返回数值必须不大于长整型大小
```c++
long long  Combination(int n, int m)
{
    
    long long ans = 1;
    for(int i=n; i>=(n-m+1); --i)
        ans *= i;
    while(m)
        ans /= m--;
    return ans ;
}
```

# 出栈序列统计：Catalan number 应用
栈是常用的一种数据结构，有n个元素在栈顶端一侧等待进栈，栈顶端另一侧是出栈序列。你已经知道栈的操作有两种：push和pop，前者是将一个元素进栈，后者是将栈顶元素弹出。现在要使用这两种操作，由一个操作序列可以得到一系列的输出序列。请你编程求出对于给定的n，计算并输出由操作数序列1，2，…，n，经过一系列操作可能得到的输出序列总数。

解：答案为Catalan number
`h(n) = C(2n,n)/(n+1)`

## Catalan number
令h(1)=1, h(0)=1，catalan数满足递归式：h(n)=h(n-1)*2(2n-1)/(n+1)
```
h(n)=h(0)*h(n-1)+h(1)*h(n-2)+...+h(n-1)h(0) (n>=2)

　　=C(2n, n)/(n+1)

　　=h(n-1)*2(2n-1)/(n+1)

```
