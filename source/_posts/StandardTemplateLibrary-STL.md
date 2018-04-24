---
title: StandardTemplateLibrary(STL)
date: 2016-11-11 14:38:05
tags:
---
`11 Nov 2016`

# 范型程序设计

C++ 语言的核心优势之一就是便于软件的重用，C++中有两个方面体现重用:
1. 面向对象的思想:继承和多态，标准类库
2. 泛型程序设计(generic programming) 的思想: 模板机 制，以及标准模板库 STL。

将一些常用的数据结构(比如链表，数组，二叉树) 和算法(比如排序，查找)写成模板，以后则不论数据 结构里放的是什么对象，算法针对什么样的对象，则都 不必重新实现数据结构，重新编写算法。

标准模板库 (Standard Template Library) 就是一些常用数据结构和算法的模板的集合。
有了STL，不必再写大多的标准数据结构和算法，并且可获得非常高的性能。

# 标准模版库

- 容器:可容纳各种数据类型的通用数据结构,是类模板 
- 迭代器:可用于依次存取容器中元素，类似于指针 
- 算法:用来操作容器中的元素的函数模板

<!-- more-->
## 容器

可以用于存放各种类型的数据(基本类型的变量， 对象等)的数据结构，都是类模版,分为三种:

1. 顺序容器(无序的)
- vector, deque,list
2. 关联容器(排序的)
- set, multiset, map, multimap
3. 容器适配器
- stack, queue, priority_queue(排序的)

### vector
头文件 #include <vector>

可变长的动态数组。元素在内存连续存放。支持随机访问迭代器，随机存取任何元素都能在常数时间完成。
在尾端增删元素具有较佳的性能(大部分情况下是常数时间)。在中间插入慢。
所有STL算法都能对vector操作。

#### 构造函数初始化

- vector(): 无参构造函数, 将容器初始化成空的
- vector(int n) : 将容器初始化成有n个元素
- vector(int n, const T & val) : 假定元素类型是T, 将容器初始化成 有n个元素, 每个元素的值都是val
- vector(iterator first, iterator last) : 将容器初始化为与别的容器上区间 [first, last)一致的内容

#### vector的成员函数

- void pop_back() : 删除容器末尾的元素
- void push_back(const T & val) : 将val添加到容器末尾
- int size() : 返回容器中元素的个数
- T & font() : 返回容器中第一个元素的引用
- T & back() : 返回容器中最后一个元素的引用

### deque 
双向队列 #include <deque>

所有适用于vector的操作都适用于deque。
deque还有 `push_front()` (将元素插入到容器的头部) 和 `pop_front()`(删除头部的元素)操作。

### list
双向链表 #include <list>

不支持根据下标随机存取元素，在任何位置插入/删除都是常数时间。
具有所有顺序容器都有的成员函数。

#### list特有成员函数

- push_front ：在链表最前面插入
- pop_front：删除链表最前面的元素
- sort ：排序 (list 不支持 STL 的算法 sort)
- remove：删除和指定值相等的所有元素
- unique：删除所有和前一个元素相同的元素
- merge：合并两个链表, 并清空被合并的链表
- reverse：颠倒链表
- splice：在指定位置前面插入另一链表中的一个或多个元素, 并在另一链表中删除被插入的元素

#### list容器之sort函数

list容器的迭代器不支持完全随机访问，不能用标准库中sort函数对它进行排序。

list自己的sort成员函数 
```c++
list<T> classname
classname.sort(compare); //compare函数可以自己定义 
classname.sort(); //无参数版本, 按<排序
```

###  顺序容器的常用成员函数
- front :返回容器中第一个元素的引用
- back : 返回容器中最后一个元素的引用
- push_back : 在容器末尾增加新元素
- pop_back : 删除容器末尾的元素
- erase :删除迭代器指向的元素(可能会使该迭代器失效)，或 删除一个区间，返回被删除元素后面的那个元素的迭代器


### set/multiset 

头文件 #include <set>
元素是排序的,插入任何元素，都按相应的排序规则来确定其位置,set 即集合。set中不允许相同元素，multiset中允许存在相同的元素。

```c++
template<class Key, class Pred = less<Key>, class A = allocator<Key> >
class multiset { ...... };
```

Pred类型的变量决定了multiset 中的元素，“一个比另一个小”是怎么定义的。 multiset运行过程中，比较两个元素x,y的大小的做法，就是生成一个 Pred类型的 变量，假定为 op,若表达式op(x,y) 返回值为true,则 x比y小。
Pred的缺省类型是 less<Key>。

#### 成员函数

- iterator find(const T & val)： 在容器中查找值为val的元素，返回其迭代器。如果找不到，返回end()。
- iterator insert(const T & val)： 将val插入到容器中并返回其迭代器。
- void insert( iterator first,iterator last)： 将区间[first,last)插入容器。
- int count(const T & val)：  统计有多少个元素的值和val相等。
- iterator lower_bound(const T & val)：查找一个最大的位置 it,使得[begin(),it) 中所有的元素都比 val 小。
- iterator upper_bound(const T & val)：
查找一个最小的位置 it,使得[it,end()) 中所有的元素都比 val 大。
- pair<iterator,iterator> equal_range(const T & val)：同时求得lower_bound和upper_bound。
- iterator erase(iterator it)：
删除it指向的元素，返回其后面的元素的迭代器(Visual studio 2010上如此，但是在 C++标准和Dev C++中，返回值不是这样)。

### map/multimap

头文件 #include <map>
map与set的不同在于map中存放pari,其有且仅有两个成员变量，一个名 为first,另一个名为second, map根据first值对元素进行从小到大排序，并可快速地根据first来检索元素。
map同multimap的不同在于是否允许相同first值的元素。

```c++
template<class Key, class T, class Pred = less<Key>, class A = allocator<T> >
class multimap { ....
};
typedef pair<const Key, T> value_type; .......
//Key 代表关键字的类型
```

#### pair 模板
map/multimap里放着的都是pair模版类的对象，且按first从小到大排序。

```c++
template<class _T1, class _T2> struct pair
{
typedef _T1 first_type; typedef _T2 second_type;
_T1 first;
_T2 second;
pair(): first(), second() { }
pair(const _T1& __a, const _T2& __b) : first(__a), second(__b) { } template<class _U1, class _U2> pair(const pair<_U1, _U2>& __p)
: first(__p.first), second(__p.second) { } };
```

multimap中的元素由 <关键字,值>组成，每个元素是一个pair对象，关键字
就是first成员变量,其类型是Key。

multimap 中允许多个元素的关键字相同。元素按照first成员变量从小到大
排列，缺省情况下用 less<Key> 定义关键字的“小于”关系。

####  map的[]成员函数

若pairs为map模版类的对象，
pairs[key]
返回对关键字等于key的元素的值(second成员变量)的引用。若没有关键 字为key的元素，则会往pairs里插入一个关键字为key的元素，其值用无参构造函数初始化，并返回其值的引用。

#### 成员函数

- find: 查找等于某个值 的元素(x小于y和y小于x同时不成立即为相等) 
- lower_bound : 查找某个下界
- upper_bound : 查找某个上界
- equal_range : 同时查找上界和下界
- count :计算等于某个值的元素个数(x小于y和y小于x同时不成立即为相等) 
- insert: 用以插入一个元素或一个区间

### 顺序容器和关联容器中都有的成员函数
- begin： 返回指向容器中第一个元素的迭代器
- end： 返回指向容器中最后一个元素后面的位置的迭代器  
- rbegin 返回指向容器中最后一个元素的迭代器
- rend： 返回指向容器中第一个元素前面的位置的迭代器 
- erase 从容器中删除一个或几个元素
- clear： 从容器中删除所有元素

### 迭代器

用于指向顺序容器和关联容器中的元素，迭代器用法和指针类似。
有const 和非 const两种。通过迭代器可以读取它指向的元素，通过非const迭代器还能修改其指向的元素。

定义一个容器类的迭代器的方法可以是:
`容器类名::iterator 变量名;`
或:
`容器类名::const_iterator 变量名;`
访问一个迭代器指向的元素:
`* 迭代器变量名`

迭代器上可以执行 ++ 操作, 以使其指向容器中的下一个元素。 如果迭代器到达了容器中的最后一个元素的后面，此时再使用 它，就会出错，类似于使用NULL或未初始化的指针一样.


### stack
头文件 #include <stack>
后进先出,容器适配器上没有迭代器。

容器适配器都有3个成员函数:
- push: 添加一个元素;
- top: 返回栈顶部或队头元素的引用 
- pop: 删除一个元素

### queue
头文件 #include <queue>
先进先出，只能插入, 删除, 访问栈顶的元素。同样也有push, pop, top函数
- push发生在队尾
- pop, top发生在队头, 先进先出

### priority_queue
头文件 #include <queue>
最高优先级元素总是第一个出列，priority_queue 通常用堆排序技术实现, 保证最大的元素总是在最前面，默认的元素比较器是 less<T>。
- 执行pop操作时, 删除的是最大的元素
- 执行top操作时, 返回的是最大元素的引用

```c++
#include <queue> 
#include <iostream>
using namespace std; 
int main() {
priority_queue<double> priorities; 
priorities.push(3.2);
priorities.push(9.8); 
priorities.push(5.4);
while( !priorities.empty() ) {
cout << priorities.top() << " "; priorities.pop();
}
return 0;
} //输出结果: 9.8 5.4 3.2
```
