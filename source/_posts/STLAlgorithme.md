---
title: STLAlgorithme
date: 2016-11-15 14:55:43
tags:
---
`15 Nov 2016`

使用模版算法需要 #include <algorithme>

# STL中的算法大致可以分为以下七类:

- 不变序列算法 
- 变值算法
- 删除算法
- 变序算法
- 排序算法
- 有序区间算法 
- 数值算法

大多重载的算法都是有两个版本的,用“==”判断元素是否相等,或用“<”来比较大小 多出一个类型参数“Pred”和函数形参“Predop”:
通过表达式 “op(x,y)” 的返回值: ture/false,判断x是否 “等于” y，或者x是否 “小于” y
<!--more-->
# 不变序列算法
该类算法不会修改算法所作用的容器或对象,适用于顺序容器和关联容器,时间复杂度都是O(n)

- min:求两个对象中较小的(可自定义比较器)
- max:求两个对象中较大的(可自定义比较器)
- find:在区间中查找等于某值的元素
```c++
template<class InIt, class T>
InIt find(InIt first, InIt last, const T& val);

```
返回区间 [first,last) 中的迭代器 i ,使得 * i == val.

- count:计算区间中等于某值的元素个数
```c++
template<class InIt, class T>
size_t count(InIt first, InIt last, const T& val);
```
计算[first, last) 中等于val的元素个数(x==y为true算等于)

- for_each:对区间中的每个元素都做某种操作
```c++
template<class InIt, class Fun>
Fun for_each(InIt first, InIt last, Fun f);
```
对[first, last)中的每个元素e, 执行f(e), 要求 f(e)不能改变e.

- min_element:求区间中的最小值(可自定义比较器)
```c++
template<class FwdIt>
FwdIt min_element(FwdIt first, FwdIt last);
```
返回[first,last) 中最小元素的迭代器, 以 “<” 作比较器,最小指没有元素比它小, 而不是它比别的不同元素都小 因为即便a!= b, a<b 和b<a有可能都不成立.

- max_element:求区间中的最大值(可自定义比较器)
```c++
template<class FwdIt>
FwdIt max_element(FwdIt first, FwdIt last);
```
返回[first,last) 中最大元素(不小于任何其他元素)的迭代器 以 “<” 作比较器.

- count_if:计算区间中符合某种条件的元素个数
```c++
template<class InIt, class Pred>
size_t count_if(InIt first, InIt last, Pred pr);
```
计算[first, last) 中符合pr(e) == true 的元素e的个数

- find_if:在区间中查找符合某条件的元素
```c++
template<class InIt, class Pred>
InIt find_if(InIt first, InIt last, Pred pr);
```
返回区间 [first,last) 中的迭代器 i, 使得 pr(*i) == true.

- find_end:在区间中查找另一个区间最后一次出现的位 置(可自定义比较器)
- find_first_of:在区间中查找第一个出现在另一个区间中的元素 (可自定义比较器)
- adjacent_find:在区间中寻找第一次出现连续两个相等元素的位置(可自定义比较器)
- search:在区间中查找另一个区间第一次出现的位置(可自定义比较器)
- search_n:在区间中查找第一次出现等于某值的连续n个元 素(可自定义比较器)
- equal:判断两区间是否相等(可自定义比较器)
- mismatch:逐个比较两个区间的元素，返回第一次发生不相等的两个元素的位置(可自定义比较器)

# 变值算法

此类算法会修改源区间或目标区间元素的值,值被修改的那个区间, 不可以是属于关联容器的。

- for_each:对区间中的每个元素都做某种操作
- copy:复制一个区间到别处
```c++
template<class InIt, class OutIt> OutIt copy(InIt first, InIt last, OutIt x);
```
本函数对每个在区间[0, last - first)中的N执行一次 *(x+N) = *(first + N), 返回 x + 

- copy_backward:复制一个区间到别处, 但目标区前是从后往前被修改的
- transform:将一个区间的元素变形后拷贝到另一个区间
```c++
template<class InIt, class OutIt, class Unop>
OutIt transform(InIt first, InIt last, OutIt x, Unop uop);
```
对[first,last)中的每个迭代器I,执行 uop( * I ); 并将结果依次放入从 x 开始的地方,要求 uop( * I ) 不得改变 * I 的值.
本模板返回值是个迭代器, 即 x + (last-first) • x可以和 first相等.

- swap_ranges:交换两个区间内容
- fill:用某个值填充区间
- fill_n:用某个值替换区间中的n个元素
- generate:用某个操作的结果填充区间
- generate_n:用某个操作的结果替换区间中的n个元素
- replace:将区间中的某个值替换为另一个值
- replace_if:将区间中符合某种条件的值替换成另一个值
- replace_copy:将一个区间拷贝到另一个区间，拷贝时某个值 要换成新值拷过去
- replace_copy_if:将一个区间拷贝到另一个区间，拷贝时符合某条件的值要换成新值拷过去

# 删除算法
删除一个容器里的某些元素
删除不会使容器里的元素减少
删除算法不应作用于关联容器
算法复杂度都是O(n)的
- 将所有应该被删除的元素看做空位子
- 用留下的元素从后往前移, 依次去填空位子
- 元素往前移后, 它原来的位置也就算是空位子
- 也应由后面的留下的元素来填上
- 最后, 没有被填上的空位子, 维持其原来的值不变

- remove:删除区间中等于某个值的元素
- remove_if:删除区间中满足某种条件的元素
- remove_copy:拷贝区间到另一个区间. 等于某个值的元素不拷贝
- remove_copy_if:拷贝区间到另一个区间. 符合某种条件的元素不拷贝
- unique:删除区间中连续相等的元素, 只留下一个(可自定义比较器)
```c++
template<class FwdIt>
FwdIt unique(FwdIt first, FwdIt last);
//用 == 比较是否等

template<class FwdIt, class Pred>
FwdIt unique(FwdIt first, FwdIt last, Pred pr);
//用 pr (x,y)为 true说明x和y相等
```
对[first,last) 这个序列中连续相等的元素, 只留下第一个,返回值是迭代器, 指向元素删除后的区间的最后一个元 素的后面

- unique_copy:拷贝区间到另一个区间. 连续相等的元素, 只拷贝第一个到目 标区间 (可自定义比较器)

# 变序算法
变序算法改变容器中元素的顺序,是不改变元素的值,变序算法不适用于关联容器,算法复杂度都是O(n)的.

- reverse:颠倒区间的前后次序
- reverse_copy:把一个区间颠倒后的结果拷贝到另一个区间,源区间不变
- rotate:将区间进行循环左移
- rotate_copy:将区间以首尾相接的形式进行旋转后的结果,拷贝到另一个区间，源区间不变
- next_permutation:将区间改为下一个排列(可自定义比较器)
- prev_permutation:将区间改为上一个排列(可自定义比较器)
- random_shuffle:随机打乱区间内元素的顺序
- partition:把区间内满足某个条件的元素移到前面，不满足该条件的移到后面

# 有序区间算法
要求所操作的区间是已经从小到大排好序的 需要随机访问迭代器的支持 有序区间算法不能用于关联容器和list

- binary_search:判断区间中是否包含某个元素
```c++
template<class FwdIt, class T>
bool binary_search(FwdIt first, FwdIt last, const T& val);
//上面这个版本, 比较两个元素x, y 大小时, 看 x < y template<class FwdIt, class T, class Pred>
bool binary_search(FwdIt first, FwdIt last, const T& val, Pred pr);
//上面这个版本, 比较两个元素x, y 大小时, 若 pr(x,y) 为true, 则 认为x小于y
```
- includes:判断是否一个区间中的每个元素，都在另一个区间中
- lower_bound:查找最后一个不小于某值的元素的位置
```c++
template<class FwdIt, class T>
FwdIt lower_bound(FwdIt first, FwdIt last, const T& val);
//要求[first,last)是有序的
//查找[first,last)中的, 最大的位置 FwdIt, 使得[first,FwdIt)中所有的元素都比 val 小
```
- upper_bound:查找第一个大于某值的元素的位置
```c++
template<class FwdIt, class T>
FwdIt upper_bound(FwdIt first, FwdIt last, const T& val);
//要求[first,last)是有序的
//查找[first,last)中的, 最小的位置 FwdIt, 使得[FwdIt,last)中所有的元素都比 val 大
```

- equal_range:同时获取lower_bound和upper_bound
- merge:合并两个有序区间到第三个区间
```c++
template<class InIt1, class InIt2, class OutIt>
OutIt merge(InIt1 first1, InIt1 last1, InIt2 first2, InIt2 last2, OutIt x);
//用 < 作比较器
template<class InIt1, class InIt2, class OutIt, class Pred>
OutIt merge(InIt1 first1, InIt1 last1, InIt2 first2, InIt2 last2, OutIt x, Pred pr);
//用 pr 作比较器把[first1,last1), [ first2,last2) 两个升序序列合并, 形成第 3 个升序序列, 第3个升序序列以 x 开头
```
- set_union:将两个有序区间的并拷贝到第三个区间
- set_intersection:将两个有序区间的交拷贝到第三个区间
- set_difference:将两个有序区间的差拷贝到第三个区间
- set_symmetric_difference:将两个有序区间的对称差拷贝到第三个区间
- inplace_merge:将两个连续的有序区间原地合并为一个有序区间

