---
title: FileStreamAndTemplate.md
date: 2016-11-08 18:06:04
tags:
---
`8 Nov 2016`

# 文件操作

用于文件操作 — 统称为文件流类。C++标准库中: ifstream, ofstream和fstream共3个类。
先打开文件，再读写文件，再关闭文件。
包含头文件 `#include <fstream>` 

## 打开文件
1. 创建文件流对象并且打开
`ofstream outFile(“clients.dat”, ios::out|ios::binary); `

2. 创建文件流之后打开
```c++
ofstream fout;
fout.open( “test.out”, ios::out|ios::binary );
```

<!-- more -->
## 文件指针
对于输入文件,有一个读指针。对于输出文件, 有一个写指针。对于输入输出文件, 有一个读写指针。标识文件操作的当前位置, 该指针在哪里读写操作就在哪里进行。

1. 读指针

tellg() : 取得读指针的位置。
seekg() : 将读指针移动到指定位置。

例子：
```c++
ifstream fin(“a1.in”,ios::in); 
long location = fin.tellg(); //取得读指针的位置
location = 10L;  
fin.seekg(location);    //将读指针移动到第10个字节处
fin.seekg(location, ios::beg); //从头数location 
fin.seekg(location, ios::cur); //从当前位置数location 
fin.seekg(location, ios::end);//从尾部数location

```

2. 写指针

tellp() : 取得写指针的位置。
seekp() : 将写指针移动到指定位置。
    
```c++
ifstream fin(“a1.in”,ios::in); 
long location = fin.tellg(); //取得读指针的位置
location = 10L; 
fin.seekg(location); //将读指针移动到第10个字节处 
fin.seekg(location, ios::beg); //从头数location
fin.seekg(location, ios::cur); //从当前位置数location 
fin.seekg(location, ios::end); //从尾部数location   
```

## 文件的读写
1. 写如文件内容：
`fout.write( (const char *)(&x), sizeof(int) );`

2. 读取文件内容
`fin.read( (char *)(&x), sizeof(int) );`

## 关闭文件

```c++
ifstream fin(“test.dat”, ios::in);
fin.close();
ofstream fout(“test.dat”, ios::out);
fout.close();
```


# 函数模版

例子：
```c++
template <class T> 
void Swap(T & x, T & y) {
T tmp = x; x = y;
y = tmp;
}

template<class T1, class T2>
T1 myFunction( T1 arg1, T2 arg2) {
cout<<arg1<<“ ”<<arg2<<“\n”;
return arg1; 
}
```

# 类模版

在定义类的时候给它一个/多个参数，这个/些参数表示不同的数据类型。
在调用类模板时, 指定参数, 由编译系统根据参数提供 的数据类型自动产生相应的模板类。

## 类模版的定义

1. 写类模版
```c++
template <class T1, class T2> class Pair{
public:
T1 key; //关键字
T2 value; //值
Pair(T1 k,T2 v):key(k),value(v) {};
bool operator < (const Pair<T1,T2> & p) const;
};
```

2. 声明一个模版类对象

编译器由类模板生成类的过程叫类模板的实例化
由类模板实例化得到的类叫模板类

类模板名 <真实类型参数表> 对象名(构造函数实际参数表);
`Pair<string, int> student("Tom",19);`

## 类模板与非类型参数
类模板的参数声明中可以包括非类型参数

`template <class T, int elementsNumber>`





