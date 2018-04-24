---
title: UBiOnlineTest
date: 2016-11-16 21:09:04
tags: 育碧面试网测(C++)
---

`16 Nov 2016`

在巴黎一年多了，去年由于学校安排不用实习，今年就不行了。所以，年底之前就在忙活实习的事情。之前偶尔发现育碧巴黎总部要招实习生，当然我自己也是非常想在育碧这样的公司工作的，因为她的名气？超酷的工作环境？高水平的同事？可能都有些，于是就精心准备了下简历、动机信，投了游戏编程(game play programmer)。这个职位听起来也蛮不错的，但是c++开发。由于我c++在本科一年级学的，现在基本就和没学一样，所以就开始重新复习准备了一阵。

一段时间之后，投的简历给我回复说要安排网测，今天早上测试。早上，美女HR在指定时间给我发来了题目，三道120分钟。一看题，整体感觉，给的题目不算是特别难，应该是基本。但由于我还是对c++不是很熟练，做的很差。

但首先她推荐但我没有用VS编译器，我用clang编译，所以搞清楚她给的文件的格式用了不少时间。其次，我承认对文件输入输出虽然知道，但基本没有自己写过，用起来很蹩脚。还有STL中好多函数还是不清楚，以及她给的题目中用了闭包，这个我完全没想到。所以，虽然对题目都有想法，现实是没有完全实现一道。

现在把题目拿来，给出做法及知识点总结。个人还是蛮喜欢育碧这个游戏公司，相信我准备好的话下次再投应该不会有问题。
<!--more-->
# data-reader
**题目：**
请你设计一个文件数据读取写入软件。输入数据有不同的版本，和不同的数据结构。输出的数据有统一的版本和结构。

从输入文件中导入数据。每一组数据由只包含#符开头的一行标记开始，之后的行包含这样的字段“键：值”
多行的键值对无序出现，如果一个旧的版本中有值不存在，则值为0，新版本则改为None。

版本0.2: 在Version: 0.2之后
X: entier
Y: entier
Label: chaîne de caractères
Rank: chaîne de caractères

版本0.1: 在Version: 0.1之后
X: entier
Y: entier
Label: chaîne de caractères
Color: chaîne de caractères

## code
这里先给出一个不完善的做法：这个作法无法把值为0的改为None
```c++
struct inItem
{
    string lin0;
    string linV;
    string linX;
    string linY;
    string linL;
    string linR;
    bool input(string str){
        if(str.find("#") != -1 ) return true; // 或 includes(str.begin(),str.end(),"#") ！= str.end() 来判断是一样的
        if(str.find("X:") != -1) linX = str;
        if(str.find("Y:") != -1) linY = str;
        if(str.find("Label:") != -1) linL = str;
        if(str.find("Rank:") != -1) linR = str; 
        return false; 
    }
    void reset(){
        lin0 = "#";
        linV = "Version: 0.2";
        linX = "X: None";
        linY = "Y: None";
        linL = "Label: None";
        linR = "Rank: None";
    }
};

void writeItem(ofstream & o,inItem i){
        o<<i.lin0<<endl;
        o<<i.linV<<endl;
        o<<i.linX<<endl;
        o<<i.linY<<endl;
        o<<i.linL<<endl;
        o<<i.linR<<endl;
}
void process(const vector<string> & input_content)
{
    vector<string> output_content;
    ofstream outFile("out.txt",ios::out);
    bool canWrite = false;
    inItem item;
    item.reset();
    ostream_iterator<string> output(outFile,"\n");
    for_each(input_content.begin(), input_content.end(),
            [&](string line){
                if(item.input(line)){ //find #
                    if(canWrite){
                        writeItem(outFile,item);
                        item.reset();
                    }else{
                            canWrite = true;
                        };  
                }else{
                    item.input(line);
                };
            });

    writeItem(outFile,item);
    outFile.close();

}
```

# c++11 闭包(Liambda Function)

```c++
[ capture ] ( params ) mutable exception attribute -> ret { body }  (1) 
[ capture ] ( params ) -> ret { body }  (2) 
[ capture ] ( params ) { body } (3) 
[ capture ] { body }    (4) 
```

1. lambda-introducer： 定义引用自由变量的方式,指定哪些在函数声明处的作用域中可见的符号将在函数体内可见.

[] // 没有定义任何变量。使用未定义变量会导致错误。

[x, &y] // x 以传值方式传入(默认)，y 以引用方式传入。

[&] // 任何被使用到的外部变量皆隐式地以引用方式加以使用。

[=] // 任何被使用到的外部变量皆隐式地以传值方式加以使用。

[&, x] // x 显示地以传值方式加以使用。其余变量以引用方式加以使用。

[=, &z] // z 显示地以引用方式加以使用。其余变量以传值方式加以使用。


2. lambda-parameter-declaration-list：

参数列表。但是参数不可以有默认值，不可以使用变长参数，不可以有unamed arguments

3. mutable-specification 

使得传值引入的变量可以修改。这个修改因为是修改的外部变量的拷贝，因此并不会影响它本来的值

4. exception-specification：

throw()该函数不能抛出异常。如果抛出异常，编译器将报warning C4297。 throw(...) 可以抛出异常。throw(type)可以抛出type的异常

5. lambda-return-type-clause：

如果仅有0/1个return的话可以省略。返回值可以是lambda表达式。

```c++
auto g = [](int x) -> function<int (int)> { return [=](int y) { return x + y; }; }; 

auto h = [](const function<int (int)>& f, int z) { return f(z) + 1; }; 

```

## 例子：

- 一般用法最后用括号传递参数值
```c++
    int x = 100,y=200,z=300;
    cout << [ ](double a,double b) { return a + b; } (1.2,2.5) << endl; //(1.2,2.5) 为a，b的参数值
    auto ff = [=,&y,&z](int n) {
    cout <<x << endl; y++; z++;
    return n*n;
    };
    cout << ff(15) << endl;
    cout << y << "," << z << endl;
```

- 闭包类似于函数指针用法，这个列子参数为引用类型
```c++
    vector<int> a { 1,2,3,4};
    int total = 0;
    for_each(a.begin(),a.end(),[&](int & x) {total += x; x*=2;}); cout << total << endl; //输出 10 
    for_each(a.begin(),a.end(),[ ](int x) { cout << x << " ";}); return 0;
```

- 闭包实现斐波纳切数列
```c++
function<int(int)> fib = [&fib](int n){ return n <= 2 ? 1 : fib(n-1) + fib(n-2);};
cout << fib(5) << endl; //输出5
//function<int(int)> 表示返回值为 int, 有一个int参数的函数 std::function 是一个类模版可以对函数(普通函数、成员函数)、lambda表达式、std::bind的绑定表达式、函数对象等进行封装。std::function的实例可以对这些封装的目标进行存储、复制和调用等操作
```