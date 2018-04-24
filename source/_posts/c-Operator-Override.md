---
title: c++ Operator Overload
date: 2016-10-14 23:19:55
tags:
---
`le 14 Oct`

### 双目运算符+ ，- 等的重载
- 可定义为成员函数或普通函数或友元函数
但成员函数可以c+5 不能5+c这样操作
普通函数不能操作私有成员
所以推荐定义成友元函数

实现复数加法 例：
```c++
Complex operator+( const Complex &a , const Complex &b ){ //a 和 b 是对+号两边操作数的常引用
     return Complex(a.real +b.real , a.imaginary + b.imaginary );
}
```
- 重载为类的成员函数时参赛个数为一

### 赋值运算符 = 重载
- 只能定义为类成员函数
```c++
Sting& String::operator=(const String &s){ ／/注意返回值要与参赛类型一样，以便在连等于时也可以使用
     if(str == s.str) return *this;
     if(str) delete[] str;
     if(s.str){
          str = new char[strlen(s.str) + 1];
          strcpy(str , s.str);
     }else{
          str = NULL;
     }
     return *this;
}
```
ps: void operator=(const Complex &)
如果返回值是void的话，如果出现连续赋值操作比如`c=b=a`,则得到`c=void`.
只有返回赋值

<!---more-->
### 重载[] 运算符
```c++
int& CArray::operator[ ]( int i){// CArray是自定义数组， ptr是指向数组内存的int* 指针， 返回int& 所以可以更改数组内容。
     return ptr[i];
}
```
### 流插入运算符<< , >> 的重载
cin和cout是在<iostream>中定义的，istream和ostream类的对象。
重载例子:
```c++
ostream & operator<<( ostream & o,const CStudent & s){
     o << s.nAge ;
     return o;
}
```

### 自增/自减运算符重载

- 前置运算符作为一元运算符可重载为：
`T & operator++();`成员函数
或
`T & operator++(T &);`普通函数
++obj, obj.operator++(), operator++(obj) 都调用上述函数

- 后置运算符作为二元运算符重载
`T operator++(int);`或
`T operator++(T &, int);`
obj++, obj.operator++(0), operator++(obj,0) 都调用上函数

例子：
```c++
CDemo & CDemo::operator++() { //前置 ++
     n++;
     return * this;
}

CDemo CDemo::operator++(int k) { //后置 ++
     CDemo tmp(*this); //记录修改前的对象
     n++;
     return tmp; //返回修改前的对象
}
```

### 类型强制转换运算符

```c++
operator int ( ) { return n; }//重载(int)id
```
重载了`(int) s ;` 相当于调用`s.int();`
- 不写返回值类型，因为返回值类型就是转换类型

### 运算符重载的注意事项

- C++不允许定义新的运算符
- 重载后运算符的含义应该符合日常习惯
- 运算符重载不改变运算符的优先级
- 以下运算符不能被重载: “.”, “.*”, “::”, “?:”, sizeof
- 重载运算符(), [ ], ->或者赋值运算符=时, 重载函数必须声明 为类的成员函数