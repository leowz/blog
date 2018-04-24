---
title: OC编程全解读书笔记
date: 2016-07-06 11:07:51
tags:
---
### 一、面向对象编程
OC是接口和实现分开写的语言，类公开给外部的，关于如何使用这个类的信息叫做接口（interface）而内部实现和私有方法私有变量则外部不可见（implemetation）。

### 二、类的定义
`className* object1 = [className new];
className* object2 = [[className alloc] init];`

new 等于 alloc＋ init 用alloc 和 init 显示调用可以调用不同的初始化方法用处更广泛。

##### clang编译、连接、执行程序
OC用clang编译链接代码，gcc无法使用ARC等不适合OC。
- 编译：`$ clang -c file1.m file2.m`
- 链接：`$ clang -o nameOfExe file1.o file2.o -framework Foundation`;
- 运行：`$ ./nameOfExe`;
<!--more-->
### 三、类的实现
接口声明：
```objective-c
@interface 类名: 父类名
{
     字段；
}
方法；
@end
```
- 所有的OC编译指令都以@开头以便和C区分。
- OC命名习惯：驼峰命名法，类首字母大写，方法首字母小写。

###### boolean
bool：C++标准数据类型 为true或false；
BOOL： OC或微软定义的别名数据类型，OC中char的别名，为0和非0；
Boolean：java基本数据类型，为true或false；

类实现：
```objective-c
@implementation 类名
方法实现

@end
```
- 实现部分不需要再次声明父类
- 实现部分包含了接口部分声明的所有方法的实现
- 要实现私有变量和私有方法
     - 私有变量在实现的 .m文件内定义一个@interface 定义私有变量
     - 私有方法，在接口里没有声明而实现里有的方法即为私有方法

###### ＃import & ＃include & @class
- ＃include为c引用头文件方法
-  ＃ import 优于include import可以解决重复引入头文件文件，import会做引用检查 ，如果有依赖引用关系则编译报错
- @class 只告诉编译器后面要用某个类而不关心其他的，一般放在@interface中用来解决头文件循环引用问题，如果接口中有用到自定义类用@class可以提高编译速度，但如果有使用自定义类的具体成员或方法，则要引入头文件。

###### 初始化方法
- 子类中初始化方法一般有以下逻辑
```objective-c
  - (id)init
  {
       self = [super init];
       if (self != nil){
           ….
       }
  return self;
  }
```

### 四、动态绑定
##### 类类型
```objective-c
Volume *v1,*v2; // 对象指针，存放在栈中
v1 = [[Volume alloc] initWithMin:0 max:10 step:1]; // alloc返回在堆中的对象地址，与指针关联
v2=v1;
[v1 up];//给v1发送消息
```
##### 空指针
     - nil：空的对象
     - NULL：空的基本数据类型，有时当0使用。
当给空指针发送消息，运行时不会出错但也不会做任何事情。

##### 访问器
oc不能从外部之间访问和修改实列对象的属性，定义setter和getter方法来实现访问和修改。
     - (id) variable; // getter方法
     - (void) setVariable: (id)newValue;//setter方法
使用getter和setter方法可以降低耦合，即如果类的结构发生变化，只要改变访问方法，外部不需要改变。

##### 作用域可见性修饰符
- @private：只有本类内可访问，@implementation实现中定义变量的默认属性
- @protected：本类和子类可访问，接口中默认实列变量属性
- @public
- @package:在框架内如public，在框架外如private

| @private | @protected | @public      
--- | --- | --- |---
同一个类|  yes| yes     |   yes
同一类 ->  |yes  |yes       |  yes
子类          | no     |yes     | yes
子类 ->     |no     |no        | yes
任意          | no     |no      | yes


##### 类对象
oc把类作为对象看待，认为类自身也是有行为的。类的变量和方法被称为类变量和类方法，OC中只有类方法没有类变量。OC中类对象也称为工厂facotry，类方法也称为方法工厂factory method。

类方法用于创建实例对象，通过对对象发送alloc消息实现。alloc定义在NSObject中。

class对象类型：OC定义了Class类型来表示类对象，所有类对象都是Class类型。

class方法：NSObject有一个class实列方法，用于返回对象所输的类对象。

```objective-c
     Class theClass = flag ? [Volume class] : [MuteVolume class];//对两个类发送类方法消息，返回的类对象被theClass指向
```
##### 静态变量
C的函数和在函数外部定义的变量作用域为整个源程序。如果给他们加上static修饰符，则其作用域变为在其所在文件内有效，同一程序的其它文件则不能使用它。

##### 常量const与指针用法
- 指针常量：int * const p = &a;// p指向的地址为常量，即p内的地址是不可以改变的，但地址存的内容是可以改变的。
- 常量指针：const int* p1 = &b;//p1指向的内容为常量，即本例为指向const int 即指向的内容不可变,但p1指向的地址可以改变。

##### 初始化方法的返回值类型(id)
在继承存在的情况下，要尽可能避免使用静态类型把代码写死。当初始化方法返回类型为 id时，子类也可以原封不动的使用这个父类的初始化方法而返回到子类的类型。

### 五、内存管理
oc提供自动引用计数，手动引用计数和垃圾回收(iOS不支持)三种方式。

##### 引用计数
- retain：引用计数增加1，一定要显示的发生retain来增加引用计数，光使一个指针指向对象不会增加引用计数。
- release：计数减少1
- dealloc：释放内存空间，当一个对象引用计数为0时，系统自动调用dealloc消息释放内存，一般不允许在程序内直接调用dealloc方法。

#####  自动释放
autorelease 必须和NAAutoreleasePool对象(非ARC)一起使用
```objective-c
id pool = [[NSAutoreleasePool alloc] init];
…...

[pool release];
```
ARC 使用@autoreleasPool｛｝块

##### 临时对象
在类方法中不以init开头，而以生成对象的类型开头的方法为生成临时对象的方法，通过这种方法创建的对象直接加入内部自动释放池，不需要关注如何销毁，如：` + (id) stringWithUTF8String: (const char *) bytes `
这种函数时便利构造函数，为调用其它构造函数来生成对象。

##### ARC
编译器会根据赋值操作、初始化、生命周期等因素，自动加入retain、release代码。
- ARC有效程序不能调用引用计数相关方法。retain、release、autorelease等
- ARC禁止使用NSAutoReleasePool，而使用@autoreleasepool来管理自动释放池。
```objective-c
     @autoreleasepool{

     }
```
##### 强引用弱引用
```objective-c
_ _ weak id temp;
_ _strong NSObject *obj;
NSObject * _ _ weak e, * _ _ weak f;
```
这种用于修饰指针类型变量的修饰符叫做生命周期修饰符，除strong、weak以外还有autoreleasing、unsafe和unretained等。

### 六、垃圾回收

##### 跟集合
全局变量和静态变量、栈内临时变量对象称为根集合。被根集合引用或被根集合引用对象引用的对象不可回收。
只要是从根集合出发无法达到的对象都属于垃圾回收的目标。

##### 垃圾收集器
运行自适应，当程序运行分配内存超过一定量则自动被触发。触发后先找到所有不用的对象，给这些对象发送`finalize`消息。等所有对象响应了消息后，才释放这些对象。我们可以通过给NSGarbageCollector发送`collectIfNeeded`消息来启动垃圾收集器。

##### drain
`- (void) drain`
- 在引用计数内存管理中drain方法和release方法具有同样功能即释放自动释放池。
- 在垃圾回收内存管理中，drain方法表示申请垃圾回收，与collectIfNeeded方法有相同功能。

### 七、属性
访问方法的目标一般被称为属性，属性可以用点操作符访问方法，只要定义了访问方法就可以用点操作符。

##### 显示声明
`@property (指令) int hitPoint` ：声明属性，一行可多个
`@synthesize hitPoint`：在实现中可以自动生成getter和setter方法 一行可声明多个        
`@dynamic`:告诉编译器自动合成无效。

##### 原子性
原子性是多线程的一个概念
原子的（atomic）：多线程环境下访问属性是安全的，在执行过程中不会被打断。
非原子的（nonatomic）：访问方法在执行过程中可以被打断。缺省情况下访问方法是原子的。

### 八、NSObject
根类相当于运行时系统的一个接口。继承了NSObject的所有类都可以使用运行时系统的功能。
有很大方法具体参考书。

##### 函数指针
IMP：implementation的缩写，一个OC函数指针，IMP定义为：
`typedef id (*IMP)(id,SEL,….); `

函数指针指向函数代码首地址。函数指针可以复制给变量或数组，可以以作为气体函数的参数。

- 声明一个函数指针：
`int (*p)(double,int *);`

- 指向一个定义好的函数
`int (*p)(double,int *)  = &funcA ; ` & 可省略

- 调用指针
`x = (*p)(1.42,&error);`也可以像调用函数一样使用p(1,42,&error);

可以使用typedef来定义一个函数指针类型：
`typedef int (*t_fuc)(double,int *);`
其中 t_func 为新的类型名，可以用`t_func p`形式来声明指针变量。

### 九、Foundation常用类
  对数据区分可变和不可变类，原因是从实现复杂度方面考虑。如果数据结构是可变的那么开始就应该设计好操作对应的数据结构和方法。
- 可变对象的生成
可变类是其不可变类的子类，所以可变类可以当做不可变对象的实列变量来用，于此相反不可变对象不能当做可变对象来用。
用不可变对象创建可变副本用方法`- (id) mutableCopy`

##### NSString 主要方法
###### Unicode编码：
- -(id) initWithUTF8String: (const char *) bytes:用以UTF－8编码的NULL结束的C字符串中复制信息生成NSString对象。
- -(__ strong const char *) UTF8String : 返回以UTF8编码NULL结尾的C字符串指针。
- -(NSUInteger) length:返回NSString中Unicode字符个数。
- -(unichar) characterAtIndex:(NSUInteger) index:返回索引位置的字符。
- -(id) initWithCharacters : (const unichar *) characters length:(NSUInterger ) length:用characters指针指向的长为length的字符串初始化并返回一个NSString对象。
- -(void) getCharacters: (unichar *) buffer range:(NSRange) aRange:把NSString的范围为aRange的字符串写入buffer里。

###### 编码转换：C风格或字节类型的字符串和NSString之间可以相互转换。
- -(id) initWithCString: (const char *) nullTerminatedCString encoding: (NSStringEncodeing) encoding :用C字符串初始化一个NSString对象，C风格字符串编码方式为encoding。
- -(__strong const char *) cStringUsingEncoding:(NSStringEncoding) encoding :把一个NSString用encoding编码返回一个C风格字符串。
- -(id ) initWithData: (NSData *) data encoding: (NSStringEncoding) encoding : 用NSData 中的二进制初始化NSString对象。 data中二机制编码为encoding返回NS     `     ｀String对象编码是Unicode。
- -(BOOL) canBeConvertedToEncoding: (NSStringEncoding) encoding : 测试NSString能否转换为encoding编码。

###### 生成指定格式的字符串
- -(id) initWithFormat: (NSString *) format,… : 生成NSString 用类似C中printf()的方式。

###### NSString 比较方法
- -(NSComparisonResult) compare:(NSString *) aString : 触发 - compare:options:range:locale: 函数且options 为nil，range字符串全部范围，locale为nil。结果返回NSComparsonResult 包括NSOrderedAscending 、Same、Descending，比较词汇值大小。
- -(NSComparisonResult) caseInsensitiveCompare:(NSString *) aString :不区分大小写的比较
- -(NSComparisonResult) localizedStandardCompare: (NSString *) aString :按照Mac的Finder排序规则排序。
- -(BOOL) isEqualToString: (NSString *) aString :比较两个String是否相等。
- -(BOOL) hasPrefix: (NSString *) aString : 检查是否以aString开头用-hasSuffix:判断是否以字符串结尾。

###### 为字符串追加内容
- -(NSString *) stringByAppendingString: (NSString *) aString: 在接收者字符串后加上aString，返回新字符串。
- -(NSString *) stringByAppendingFormat: (NSString *) format: 接字符串并返回新字符串，接的字符串格式由format指定。

###### 截取字符串
- -(NSString *) substringToIndex:(NSUInteger) anIndex :截取从头到index的字符串返回新的copy
- -(NSString *) substringFromIndex: (NSUInteger) anIndex:  截取从index到结尾字符串
- -(NSString *) substringWithRange: (NSRange) aRange: 返回新字符串，为接收字符串range的位子的子字符串

###### 检索置换
- -(NSRange) rangeOfString: (NSString *) aString:查找aString如果存在返回其位置range，否则返回NSNotFound
- -(NSString *) stringByReplacingCharactersInRange: (NSRange) range withString: (NSString *) replacement: 替换range内容为replacement
- -(NSString *) stringByReplacingOccurrencesOfString: (NSString *) target withString: (NSString *) replacement:替换target为replacement

###### 大小写
- -lowercaseString,-uppercaseString：用于将字符串全部转化为大小写
- -capitalizedString: 将所有单词首字母大写，其它字母小写

###### 数值转换
- -doubleValue, -floatValue, -intValue, -integerValue, -boolValue： 返回对应类型值

###### 路径处理
- -(NSString *) lastPathComponent: 文件路径中最后一部分
- -(NSString *) stringByAppendingPathComponent: (NSString *) aStr : 将aStr加到字符串末尾返回
- -(NSString *) stringByDeletingLastPathComponent: 删除最后一个部分
- -(NSString *) pathExtension : 返回文件扩展名
- -(NSString *) stringByAppendingPathExtension: (NSString *) aStr , -(NSString *) stringByDeletingPathExtension :操作扩展名
- -(BOOL) isAbsolutePath : 判断是否是绝对路径
- +(BSString *) pathWithComponents: (NSArray *) components:使用components中的元素来构建路径，自动添加路径分隔符“／”
- -(NSArray *) pathComponents : 将路径的各个部分放入数组中。

###### 文件输入输出
- -(id) initWithContentsOfFile: (NSString *) path encoding: (NSStringEncoding) enc error: (NSError **) error : path中的内容来初始化一个NSString，文件编码enc。读取文件失败会释放调用者,并返回nil的同时将错误信息写入error。
- -(Bool) writeToFile: (NSString *) path atomically: (BOOL) useAuxiliaryFile encoding: (NSStringEncoding) enc error:(NSError **) error : 将字符串写入以path为路径的文件中，useAuxiliaryfile为Yes则先新建临时文件写入，这样可以不损坏原文件。

##### NSData(Cocoa对2进制的封装，包括指向2进制的C指针和2进制数据长度)
- -(id) initWithBytes: (const void *) bytes length: (NSUInteger) length:复制以bytes开头、长度为length的数据。
- -(id) initWithData: (NSData *) aData: 用指定的NSData对象aData来创建一个新NSData对象。
- -(NSUInteger) length:返回数据长度
- -(const void *) bytes:返回数据C指针
- -(void) getBytes:(void *) buffer length:(NSUInteger) length, -(NSData *) subdataWithRange: (NSRange) range:取得部分data数据
- -(BOOL) isEqualToData : (id) anObject:比较恋歌NSData是否一样
- -(NSString *) description:返回ASC2格式字符串，采用NSData属性列表格式。
- -(id) initWithContentsOfFile:(NSString *) path options: (NSUInteger) mask error: (NSError **) errorPtr:从path指定文件读入二进制数据，mask用于指定是否使用虚拟内存。
- -(BOOL) writeToFile: (NSString *) path atomically:(BOOL) flag:将二进制写入path指定的文件中，flag用于写安全操作。

#####  数组 NSArray,NSMutableArray
- +(id) array:返回一个空的数组对象，NSMutableArray常用这个方法。
- +(id) arrayWithObject:(id) anObject : 返回只包含一个anObject元素的数组。
- -(id) initWithObjects: (id) firstObj,…. : 返回包含objects的数组。
- -(id) initWithArray:(NSArray *) anArray: 用anArray初始化一个新数组。
- -(NSUInteger) count:数组元素个数
- -(NSUInteger) indexOfObject: (id) anObject : 查看是否有与anObject相等的元素，如有返回索引，否则NSNotFound。
- -(id) objectAtIndex: (NSUInteger) index: 返回index处的元素。
- -(id) lastObject, -(id) firstObject: 访问元素。
- -(void) getObjects: (id __unsafe_unretained []) aBuffer range:(NSRange) aRange:将aRange指定范围内对象复制到aBuffer指定的C语言缓冲区。只复制指针，引用计数不发生变化。
- -(NSArray *) subarrayWithRange: (NSRange) range: 抽取原数组一部分生成新的子数组。
- -(BOOL) isEqualToArray: (id) anObject : 比较两个数组是否一致。
- -(id) firstObjectCommonWithArray: (NSArray *) otherArray : 返回第一个两个数组相同的元素。

###### 排序
- -(NSArray *) sortedArrayUsingSelector:(SEL) comparator, -(NSArray *) sortedArrayUsingFunction: (NSInteger(*) (id,id,void *))comparator context: (void *)context :排序方法，第一个用数列内元素自身有的排序方法，如果自己设计要用范畴（category）第二个用函数指针，只要把函数名称传入就可以了。比较方法要返回NSCompareResult用于数组排序，排序结果是之前数组元素引用的数组，没有拷贝。NSCompareResult是NSUInteger的别名，三个量为-1,0,1。
###### 给数组各元素发消息
- -(void) makeObjectsPerformSelector:(SEL) aSelector:
- -(void) makeObjectsPerformSelector:(SEL) aSelector withObject: (id) anObj:给每一个元素发消息带一个参数
###### 返回枚举器
- -(NSEnumerator *) objectEnumerator, -(NSEnumerator *) reverseObjectEnumerator

##### 枚举器NSEnumerator
用来遍历集合类中元素对象的抽象类，除了for..in 语法外其也可以便利，不能创建实列，只能用集合对象返回的枚举对象。
- -(id) nextObject:用来遍历每个集合元素。
- -(NSArray *) allObjects:返回集合中未被便利的元素的数组。
使用枚举器的例子：

```objective-c
NSArray *myarray;
id obj;
NSEnumerator *enumerator;
enumerator = [myarray objectEnumerator];
while((obj = [enumerator nextObject]) != nil){
/* operations*/
}
```

##### NSSet NSMutableSet
- +(id) set :返回一个临时的空的集合对象。
- -(id) initWithArray: (NSArray *) array: 用array中元素初始化一个集合，相同元素只保留一个。
- -(NSArray *) allObjects:将所有元素以数组形势返回。
- -(BOOL) containsObject: (id) anObject:判断是否有指定元素。
- -(BOOL) intersectsSet: (NSSet *) otherSet:判断是否有共同元素
- -(void) unionSet: (NSSet *) otherSet:将otherSet融合到当前MutableSet中去。

###### NSDictionary NSMutableDictionary
- +(id) dictionaryWithObject: (id) anObject forKeys: (id) aKey :返回一个词典对象，只有一个键值对。
- -(id) initWithObjects: (NSArray *) objects forKeys: (NSArray *) keys:用两个数组初始化。
- -(id) initWithObjectsAndKeys: (id) object, (id) key,… : 用值和键初始化。
- -(id) objectForKey: (id) aKey:访问对应值
- -(NSArray *) allKeys: 返回一个包含所有对象键的数组。
- -(NSEnumerator *) keyEnumerator:返回枚举器

- -(void) setObject: (id) anObject forKey: (id) aKey:向可变字典中添加元素。
- -(void) addEntriesFromDictionary: (NSDictionary *) otherDic:将otherDic中数据添加到当前字典中，有重复以otherDic为准
- -(void) removeObjectForKey: (id) aKey:删除一个项
- -(void) removeObjectsForKeys: (NSArray *) keyArray:删除keyArray中的项
- -(void) removeAllObjects:删除全部

##### 包裹类
###### NSNumber
包裹C基本数据类型
- -(id) initWith元素类型：(类型) value：初始化方法
- -(类型) 类型Value: 获取值的访问方法。

###### NSValue
用于包裹指针，结构体等数据。
- -(id) initWithBytes: (const void *) value objCType: (const char *) type:包裹结构体方法 objCType用@encode(结构体)取得。＃

###### NSNull
包裹null

##### NSURL
- -(id) initWithString: (NSStrirng *) URLString :初始化方法。
- -(BOOL) getResourcceValue: (out id *) value forKey: (NSString *) key error: (out NSError **) error:获取NSURL表示文件的属性，如大小、修改时间，权限等。

### 十、范畴
##### 范畴
实现某个类一部分方法的模块叫做范畴或类别(category)，包括接口文件和类文件实现，但范畴不能声明实列变量。其声明语法如下：

```objective-c

@interface 类名 ( 范畴名 )
／＊ 方法＊／
@end

@implementation 类名 （范畴名）
／＊实现＊／
@end
```
- oc中吧关系紧密的方法们作为一个范畴来分类。把类按照范畴分类，和C中把某些函数保存在同一个文件中相似。把实现方法互相依赖或公用同一个局部变量的方法定义为范畴，则类中依赖性较高的一部分会被归纳出来，开发也会容易。

##### 关联引用
 通过关联引用可以为已经存在的对象增加实列变量，这个功能叫做关联引用。与范畴组合起来可以实现对类的动态扩展。
- void objc_setAssociatedObject(id object, void *key, id value, object_AssociationPolicy policy):为对象object添加以key指定的地址作为关键字、以value为值的关联引用。
- id objc_getAssociatedObject(id object, void *key): 返回object以key为关键字的对象。
- void objc_removeAssociatedObjects(id object) : 断开object的所有关联。

##### 类扩展（extension）
类扩展声明可以扩展变量和方法，扩展中声明的实列变量只能在引入类主接口可扩展声明的范畴中使用。声明和范畴相似，只是圆括号之际没有文本，如下：

```objective-c
@interface Card (){
     BOOL flag;
}
- (BOOL) hasSameSuit;
@end
```
##### 可变参数方法的定义 OC同C／C＋＋
类似printf() 可变参数函数定义方法，OC与C和C＋＋都是相同的，首先有如下限制：

- 参数猎豹中不能只有可变参数
- 可变参数必须出现在参数列表最后
- 可变参数类型必须由程序来管理

 定义可变参数函数时先引入头文件stdarg.h。用...表示可变参数。获取可变参数前需要定义一个va_list类型变量。代码如下：

```c

va_list pvar;
va_start(pvar, 可变参数前一个变量名);
f = va_arg(pvar , 类型名 );
va_end(pvar );
```
va_start() 为访问可变参数进行准备，va_arg()获取参数值，va_end()关闭paar指针。

### 十一、抽象类、类簇

- OC在语法上没有特别的机制来区分抽象类。

- 类簇就是定义相同的接口并提供相同功能的一组类的集合。类簇有一个机制，可以从多个已存在的类中挑选出最适合当前场景的类并启用。Foundation框架的类基本都是类簇。

### 十二、协议

表示对象作用和行为的方法集合体称为协议。
协议声明：
```objective-c
@protocol 协议名
/* 内容*/

@end
```
协议的采用:
```objective-c
@interface 类名 ：父类名 <协议名>｛
／＊内容＊／
｝

@end
```
协议继承：
```objective-c
@protocol 协议1 <协议2>
／＊内容＊／
@end
```
指定协议类型声明：
`id <S> obj;`

协议适用性检查:

- -(BOOL) conformsToProtocol: (Protocol *) aProtocol: 接收者与aProtocol指定协议适用是返回，YES

必选功能和可选功能：
在函数前用@optional 和@required表示可选和必选。

### 十三、对象的复制和存储
- 指针复制：变量A指向一个对象，把变量A复制到变量B时，只是指针赋相同值，是对象代入到变量。
- 浅复制：将变量A指向的对象复制一份，让B指向复制对象。不继续递归复制，对象内的实列变量用指针共享。
- 深复制：使B指向的对象递归的复制A对象的所有内容。

##### 归档
Foundation框架中，可以把相互关联的多个对象归档为二进制文件，还能将对象的关系从二进制文件中还原回来。这个过程称为归档和解档。
可以使用NSKeyedArchiver和NSKeyedUnarchiver完成归档和解档，他们都是NSCoder的子类。
所有可以归档的对象都必须要适用于协议NSCoding。

```objective-c
@protocol NSCoding
- (void) encodeWithCoder: (NSCoder *) aCoder;
- (id) initWithCoder: (NSCoder *) aDecoder;

@end
```
编解码方法：
- -(void) encodeObject: (id) objv forKey: (NSString *) key:用字符串作为键编码对象。
- -(void) encodeConditionalObject: (id) obj forKey: (NSString *) key: 需要时编码。
- -(void) decodeObjectForKey: (NSString *) key:还原编码对象。

##### 属性列表
属性列表(property list) 是Cocoa中表示或保存信息的标准数据形式。可以将数组、词典、字符串、数据保存在文件中。

- ASCII 字符串属性列表：
对NSArray和NSDictionary发生description消息获得NSString中的ASCII属性列表
对 NSString发送 ` - (id) propertyList ` 消息返回对应对象结构

- XML 格式属性列表
对NSArray或NSDictionary实列使用方法`writeToFile:atomically:`可以获得XML格式属性列表
使用方法`initWithContentsOfFile:`从属性列表中恢复数据

属性列表转换：
- +(NSData *) dataWithPropertyList: (id) plist format: (NSPropertyListFormat) format options: (NSPropertyListWriteOptions) opt error: (NSError **) error: 参数plist是要转换的对象（数组、字典）将他转换为指定属性列表格式(XML或二进制)。
- +(id) propertyListWithData: (NSData *) data options: (NSPropertyListReadOptions) opt format: (NSPropertyListFormat *) format error: (NSError **) error :从存储者任意种格式的属性列表的数据对象中将对象结构复原。

### 十四、块对象、闭包
- 声明和定义：
```objective-c
void (^b) (int) = ^{参数}{实现}；
```
闭包块的声明和函数指针使用相同的书写方法，只是函数指针声明使用＊，块对象使用^。

- 类型声明：
用typedef声明类型简化代码
```objective-c
typedef int (^myBlockType) (int);

void func (myBlockType block){
     /*...*/
}
```
- 块对象中的变量行为

- 块句法中，除块句法内部的局部变量和形参外，还有块句法位置处可以访问的变量。其中有外部变量和局部变量，即捕捉到的附近的变量。
- 块内部可以访问外部变量和静态变量，也可以直接改变变量值。
- 在书写块句法时，局部变量会被保存起来所以即使之后局部变量值改变块对象也不知道，变量值可以读取不能改变，变量不能为数组，否则报错。

- 和函数指针的区别：
使用函数指针，需要不同函数应对不同功能，为给函数传递附加信息，还要用多余的参数，不能使代码简单独立。
闭包块有捕获上文变量的功能，所以可以少传递不传递参数，使用灵活。

- 块的循环引用
在ARC内存管理下当块变量出现循环强引用时会发生内存泄漏，使用` _ _ weak Logger *weakLog = logger;` weak修饰符表示若引用来解决循环强引用问题。

##### 一些实用块的方法
NSArray中有一个排序方法：
- -(NSArray *) sortedArrayUsingComparator: (NSComparator) cmptr; NSComparator 是一个闭包的别名`typedef NSComparisonResult (^NSComparator) (id obj1, id obj2);`
- -(NSUInteger) indexOfObjectPassingTest: (BOOL (^) (id obj, NSUInteger idx, BOOL *stop)) predicate:实现从数组返回满足块对象指定条件的最初元素索引。当stop为YES是可以打断循环。
NSDictionary中遍历方法：
- -(void) enumerateKeysAndObjectsUsingBlock: (void (^) (id key, id obj , BOOL *stop)) block:遍历键值对象，操作。

#### 块的值捕获
我们把闭包引用，读取外部变量称为捕获。
- 块内部可以识别块内部的变量和块当前位置可以访问的外部变量。
- 块对象内部可以访问全局变量和静态变量，可以改变变量值。
- 虽然可以在内部访问块外部的自动变量值，但不可以给变量赋值。



