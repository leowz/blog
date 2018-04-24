---
title: polymorphism
date: 2016-10-29 13:12:34
tags:
---

`29 Oct 2016`

面向对象程序设计中的类有三大特性：继承(Inheritance)，封装(Encapsulation)，多态(Polymorphism)

# C++ 多态

多态分为动态多态(dynamic polymorphism)和静态多态(static polymorphism)。
静态多态包括： 重载(overload)、模版。动态多态包括：虚函数，抽象类，覆盖(override)。一般所谓的多态是动态多态。
C++使用虚函数（虚函数表）来实现动态绑定，当基类对象的指针（或引用）指向派生类的对象时候，实际调用的是派生类相应的函数。

<!--more-->

### 实现方法
- 表现形式

1. 派生类的指针可以赋给基类指针 
```c
CBase * p = & ODerived;
p -> SomeVirtualFunction();
```
2.派生类的对象可以赋给基类引用 
```c
CBase & r = ODerived;
r.SomeVirtualFunction();
```

- 虚析构函数

由于通过new产生的基类的指针删除派生类对象时只调用基类的析构函数。为了保证不出现内存泄漏，需要把基类的析构函数定义成虚析构函数。
`virtual ~Base(){};`

- 纯虚函数和抽象类

纯虚函数: 没有函数体的虚函数。
`virtual void Print( ) = 0 ; //纯虚函数`
抽象类: 包含纯虚函数的类。其只能作为基类来派生新类使用，不能创建抽象类的对象
在抽象类中：
1. 在成员函数内可以调用纯虚函数
2. 在 构造函数/析构函数 内部不能调用纯虚函数


- C++中多态的实现原理

当类中声明虚函数时，编译器会在类中生成一个虚函数表
虚函数表是一个存储类成员函数指针的数据结构
虚函数表是由编译器自动生成与维护的
virtual成员函数会被编译器放入虚函数表中
存在虚函数时，每个对象中都有一个指向虚函数表的指针(vptr指针)
多态的函数调用语句被编译成一系列根据基类指针所指向的（或基类引用所引用的）对象中存放的虚函数表的地址，在虚函数表中查找虚函数地址，并调用虚函数的指令。。


# OC 多态
Objective-c 是动态语言，所以它具有动态类型和动态绑定的特性。Objective-c系统总是跟踪对象所属的类。对于类型的判断和方法的确定都是在运行时进行。
Objecive-c使用类对象的形式来实现运行多态，每个对象都保存其类对象的地址，类对象中保存了类的基本信息。类对象是进行动态创建（反射），动态识别，消息传递等机制的基础。
例子：
```objective-c
  Printer.h
  @interface Printer : NSObject
  - (void)print;  // 简单的方法print
  @end;

  Printer.m
  #import "Printer.h"
  @implementation Printer
  - (void)print {
  NSLog(@"打印机打印纸张")；
  }
  @end
```
创建一个子类，
```objective-c
  ColorPrinter.h
  #import "printer.h"
  @interface ColorPrinter : Printer
  - (void)print;
  @end

  ColorPrinter.m
  #import "ColorPrinter.h"
  @implementation ColorPrinter
  - (void)print{
  NSLog(@"彩色打印机")；
  }
  @end
```
另一个子类
```objective-c
 BlackPrinter.h
  #import "Printer.h"
  @interface BlackPrinter : Printer
  - (void)print;
  @end
  BlackPringer.m
  @implementation  BlackPrinter
  - (void)print{
  NSLog(@"黑白打印机")；
  }
  @end
```
定义一个Person类来操作具体的打印机
```objective-c
  Person.h
  #import "ColorPrinter.h"
  #import "BlackPrinter.h"
  // 扩展性不高， 当我们需要添加一个新的打印机的时候， 还要定义对应的方法 
  // 所以这时候就可以使用*多态*
  @interface Person ： Nsbject {
  NSString *_name;
  };
   //  - (void) printWithColor:(ColorPrinter *)colorPrint;
   //  - (void) printWithBlack:(BlackPriter *)blackPrint;
   - (void) doPrint:(Printer *)printer;
  @end    

  Person.m
  #import "Person.h"
  @implementation Person
  // - (void) printWithColor:(ColorPringter *)colorPrinter{[colorPrint print];}
  //  - (void) printWithBlack:(BlackPrinter *)blackPrint{[blackPrint print];}
  - (void) doPrint: (Printer *)printer{
  [printer print];
  }
  @end
```
main
```objective-c
    #import "Person.h"
    #import "BlackPrinter.h"
    #import "ColorPrinter.h"

    int main(int argc, const char* argv[]){
    @autoreleasepool {
     Person *person = [[Person alloc] init];

     // 多态定义  注意差别
      ColorPrinter *colorPrint = [[ColorPrinter alloc] init];
      BlackPrinter *blackPrint = [[BlackPrinter alloc] init];
     [person doPrint:colorPrinter];
     [person doPrint:blackPrinter];
     }
 }
```

对不同对象发送相同消息得到不同行为。