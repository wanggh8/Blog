---
title: IOS Objective-C Manual
date: 2019-03-21 20:31:12
tags: IOS
---

Object-C通常写作Objective-C或者Obj-C或OC，是根据C语言所衍生出来的语言，继承了C语言的特性，是扩充C的面向对象编程语言。它主要使用于Mac OS X和IOS系统。Objective-C是非常“实际”的语言。它使用一个用C写成、很小的运行库，只会令应用程序的大小增加很小。目前apple绝大多数采用的是Object-C。

<!--more--> 

Objective-C代码的文件扩展名

- .h头文件。头文件包含类，类型，函数和常数的声明。
- .m源代码文件。这是典型的源代码文件扩展名，可以包含Objective-C和C代码。 
- .mm源代码文件。带有这种扩展名的源代码文件，除了可以包含Objective-C和C代码以外还可以包含C++代码。仅在你的Objective-C代码中确实需要使用C++类或者特性的时候才用这种扩展名。 
- #import选项和#include选项完全相同，＃import只是它可以确保相同的文件只会被包含一次。

## 基础

### 程序结构

1. 预处理程序命令 `#import <头文件> `
2. 接口 `@interface name:NSObject /n内容/n @end `
3. 实现  `@implement name /n内容/n @end`
4. 方法 `- (返回类型)name; -(返回类型)name{}`
5. 变量 
6. 声明和表达
7. 注释 `/*...*/或//`

### 基本语法

#### 关键字

`auto` `else` `long` `switch` `break` `enum` `register` `typedef` `case` `extern` `return` `union` `char` `float` `shorsst` `unsigned` `const` `for` `signed` `void` `continue` `goto` `sizeof` `volatile` `default` `if` `static` `while` `do` `int` `struct` `_Packed` `double` `protocol` `interface` `implementation` `NSObject` `NSInteger` `NSNumber` `CGFloat` `property` `nonatomic` `retain` `strong` `weak` `unsafe_unretained` `readwrite` `readonly` 

#### 基本类型与框架

- 标量类型（非对象）：int、float、char、BOOL、NSInteger、NSUInteger、CGFloat
- 基本类型有多种方式转化为引用类型
- NSString和基本类型的互转
- NSNumber和基本类型的互转
- NSValue和复杂结构体的互转
- NSString和CString的互转
- 字符编码
- NSData
- NSMutableData
- NSNumber
- NSArray/NSMutableArray
- NSDictionary/NSMutableDictionary
- NSSet/NSMutableSet
- NSOrderedSet/NSMutableOrderedSet

**Foundation框架**
  定义了Objective-C类的基础层。 除了提供一组有用的原始对象类之外，它还引入了几个定义Objective-C语言未涵盖的功能的范例。使用`#import <Foundation/NSString.h>`之类的东西来导入Objective-C类，为了避免手写导入的类太多，使用`#import <Foundation/Foundation.h>`导入

1. 提供一小组基本实用程序类。
2. 通过为解除分配等事项引入一致的约定，使软件开发更容易。
3. 支持Unicode字符串，对象持久性和对象分发。
4. 提供一定程度的操作系统独立性以增强可移植性。

**数据存储**
NSArray，NSDictionary和NSSet为Objective-C任何类的对象提供存储。

**文本和字符串**
NSCharacterSet表示NSString和NSScanner类使用的各种字符分组。NSString类表示文本字符串，并提供搜索，组合和比较字符串的方法。 NSScanner对象用于扫描NSString对象中的数字和单词。

**日期和时间**
NSDate，NSTimeZone和NSCalendar类存储时间和日期并表示日历信息。它们提供了计算日期和时间差异的方法。它们与NSLocale一起提供了以多种格式显示日期和时间以及根据世界中的位置调整时间和日期的方法。

**异常处理**
异常处理用于处理意外情况，它在Objective-C中提供NSException类对象。

**文件处理**
文件处理是在NSFileManager类的帮助下完成的。

**URL加载系统**
一组提供对常见Internet协议访问的类和协议。

#### 方法

一般定义形式：

```objective-c
-(return_type)method_name:(argumentType1 )argumentName1 joiningArgument2:(argumentType2 )argumentName2 ...joiningArgumentn:(argumentTypen )argumentNamen {body of the function}
```

- **返回类型**- 方法可以返回值。`return_type`是函数返回的值的数据类型。 某些方法执行所需的操作而不返回值。 在这种情况下，`return_type`是关键字`void`。
- **方法名称**- 这是方法的名称。方法名称和参数列表一起构成方法签名。
- **参数**- 调用函数时，将值传递给参数。参数列表指的是方法的参数的类型，顺序和数量。参数是可选的。**按值调用：**将参数的实际值复制到函数的形式参数中，对函数内部参数所做的更改不会对参数产生影响。 **按引用调用**将参数的地址复制到形式参数中。在函数内部，该地址用于访问调用中使用的实际参数。对参数所做的更改会影响参数。
- **连接参数**- 一个连接的参数是让它更易于阅读并在调用时清楚地表达它。
- **方法体**- 方法体包含一组语句用于定义方法的作用。

#### 代码块

块定义了一个将数据与相关行为相结合的对象，块是Objective-C对象，因此它们可以添加到`NSArray`或`NSDictionary`等集合中。 它们还能够从封闭范围中捕获值，使其类似于其他编程语言中的闭包或`lambda`。一般形式：

```objective-c
<returntype> (^blockname)(list of arguments);//定义
//实现
<returntype> (^blockname)(list of arguments) = ^(arguments){ body; };//有参数
void (^theBlock)(void) = ^{ printf("Hello Blocks!\n"); };//无参数
//例子
double (^multiplyTwoValues)(double, double) = 
   ^(double firstValue, double secondValue) {
      return firstValue * secondValue;
   };
```

- block属性
- block用于循环
- block用于异步任务
- block常见的内存泄露场景

#### 数组、字符串、指针

```objective-c
//数组声明
type arrayName [ arraySize ];
type name[size1][size2]...[sizeN];//多维数组
//初始化数组
double balance[5] = {1000.0, 2.0, 3.4, 17.0, 50.0};
double balance[] = {1000.0, 2.0, 3.4, 17.0, 50.0};
//访问数组
double salary = balance[9];
//快速枚举遍历数组
for (classType variable in collectionObject ) { 
  statements 
}
//快速枚举向后
for (classType variable in [collectionObject reverseObjectEnumerator] ) { 
  statements 
}
//指针递增递减
ptr++ ptr--;
//指针比较 如果p1和p2指向彼此相关的变量，例如同一数组的元素，则可以有意义地比较指针
ptr <= &var[MAX - 1]
//字符串 字符串使用NSString表示，其子类NSMutableString提供了几种创建字符串对象的方法。 创建字符串对象的最简单方法是使用Objective-C的标识符：@""来构造
NSString *greeting = @"Hello";

- (NSString *)capitalizedString;
返回接收者的大写字母表示。
- (unichar)characterAtIndex:(NSUInteger)index;
返回给定数组位置的字符。
- (double)doubleValue;
以double形式返回接收者文本的浮点值。
- (float)floatValue;
以float形式返回接收者文本的浮点值
- (BOOL)isEqualToString:(NSString *)aString;
返回一个布尔值，该值使用基于Unicode的文字比较指示给定字符串是否等于接收者。
- (NSUInteger)length;
返回接收者中的Unicode字符数。 
stringByAppendingFormat
连接str1和str2 
```

#### 结构体

```objective-c
struct [structure tag] {
   member definition;
   member definition;
   ...
   member definition;
} [one or more structure variables];
```

#### 错误处理

```objective-c
NSString *domain = @"com.yiibai.MyApplication.ErrorDomain";
NSString *desc = NSLocalizedString(@"Unable to complete the process", @"");
NSDictionary *userInfo = @{ NSLocalizedDescriptionKey : desc };
NSError *error = [NSError errorWithDomain:domain code:-101 userInfo:userInfo];
```

## 高级部分

### 类与对象

#### Objective-C特征

- 类定义在两个不同的部分，即`@interface`和`@implementation`。
- 几乎所有东西都是对象的形式。
- 对象接收消息，对象通常称为接收者。
- 对象包含实例变量。
- 对象和实例变量具有范围。
- 类隐藏对象的实现。
- 属性用于提供用于其他类对此类实例变量的访问。

#### 类定义实现

类定义以关键字`@interface`开头，后跟接口(类)名称; 和一个由一对花括号括起来的类体。 在Objective-C中，所有类都派生自名为`NSObject`的基类。 它是所有Objective-C类的超类。 它提供了内存分配和初始化等基本方法。 

例如：

```objective-c
@interface Box:NSObject {
   //实例变量
   double length;    // Length of a box
   double breadth;   // Breadth of a box
   double height;    // Height of a box
}
@property(nonatomic, readwrite) double height;  // Property
@end
  
@implementation Box
@synthesize height; 

-(id)init {
   self = [super init];
   length = 1.0;
   breadth = 1.0;
   return self;
}

-(double) volume {
   return length*breadth*height;
}
@end
```

#### 类初始化和访问

```objective-c
Box box1 = [[Box alloc]init];
box1.height = 15.0
```

#### 属性

在Objective-C中引入了属性，以确保可以在类外部访问类的实例变量，只有属性才能访问类的实例变量。 实际上，为属性创建了内部`getter`和`setter`方法。

- 属性以`@property`开头，它是一个关键字
- 接下来是访问说明符，它们是非原子，读写或只读，强，不安全或不完整。 这取决于变量的类型。 对于任何指针类型，可以使用`strong`，`unsafe_unretained`或`weak`。 类似地，对于其他类型，可以使用`readwrite`或`readonly`。
- 接下来是变量的数据类型。
- 最后，将属性名称以分号结束。
- 在实现类中添加`synthesize`语句。 但是在最新的`XCode`中，合成部分由`XCode`处理，不需要包含`synthesize`语句。

##### @synthesize

该关键字指定了属性的实例变量名称，并且根据存储语义（readwrite、readonly）系统自动合成setter和getter方法，当然也可以手写来覆盖系统提供的。

##### @dynamic

该关键字告诉编译器不要为我合成setter和getter方法，这些方法将由我自己实现。当然我们可以不实现这在编译阶段不会出现问题，直到运行时才会检查是否实现了setter和getter，如果没有实现就会抛出异常。

#### **属性的特质：**

1.原子性：

　　原子性就是指该属性是否为同步的，OC中大部分属性都是nonatomic（非原子性）的，如果不写nonatomic那么就会是原子性的。理论上来说原子性属性的读写都将会是同步的，但是OC中atomic并不能一定确定属性为同步的，如果真要进行同步操作，还要用更加深层次的同步锁API。而且atomic会很影响效率，所以一般都会写nonatomic。

2.读/写权限：

　　读写为readonly和readwrite两种，前一种在系统只会合成getter方法，而后一种则会同时生成setter和getter。如果属性设置为了readonly属性，那么该属性是不可以修改的。

3.内存管理语义：

　　assign：该方法只会针对“纯量类型”(CGFloat或NSInteger等)的简单赋值操作，id类型也要用assign，所以一般iOS中的代理delegate属性都会用assign来标示，如：

```objective-c
@property (nonatomic, assign)   id <UITableViewDataSource> dataSource;
@property (nonatomic, assign)   id <UITableViewDelegate>   delegate;
```

　　strong: 使用该特性实例变量在赋值时，会释放旧值同时设置新值，对对象产生一个强引用，用MRC来说就是引用计数+1。

　　weak: 属性表明了一种”非拥有关系“，既不释放旧值，也不保留新值。用MRC就是引用计数不变，当指向的对象被释放时，该属性自动被设置为nil。这里多说一点，weak的runtime实现是通过hash表完成的，用变量名做键，一旦发现属性所指的对象被释放了，立刻设置为nil。

　　unsafe_unretained：和weak一样，唯一的区别就是当对象被释放后，该属性不会被设置为nil。所以是unsafe的。

　　copy：和strong类似，不过该属性会被复制一个新的副本。很多时使用copy是为了方式Mutable（可变类型）在我们不知道的情况下修改了属性值，而用copy可以生成一个不可变的副本防止被修改。如果我们自己实现setter方法的话，需要手动copy。

4.方法名：方法名可以修改为我们合成的方法名，可以使存取方法语义更加符合应用场景。　

```objective-c
getter = <name>
setter = <name>
```

### 继承与多态

#### 继承

Objective-C只允许多级继承，即它只能有一个基类但允许多级继承。 Objective-C中的所有类都派生自超类`NSObject`。

```objectivec
@interfacederived-class:base-class
```

如果派生类在接口类中定义，则它可以访问其基类的所有私有成员，但它不能访问在实现文件中定义的私有成员。派生类继承所有基类在声明中方法和变量

#### 多态

Objective-C多态表示对成员函数的调用将导致执行不同的函数，具体取决于调用该函数的对象的类型。在使用多态是，会进行动态检测，以调用真实的对象方法。多态在代码中的体现即父类指针指向子类对象。子类必须重写父类的方法

- 好处：如果函数方法参数中使用的是父类类型，则可以传入父类和子类对象，而不用再去定义多个函数来和相应的类进行匹配了。
- 局限性：父类类型的变量不能直接调用子类特有的方法，如果必须要调用，则必须强制转换为子类特有的方法。

代码例子：

```objective-c
@interface Shape : NSObject {
   CGFloat area;
}

- (void)printArea;
- (void)calculateArea;
@end

@implementation Shape
- (void)printArea {
   NSLog(@"The area is %f", area);
}

- (void)calculateArea {

}

@end

@interface Square : Shape {
   CGFloat length;
}

- (id)initWithSide:(CGFloat)side;
- (void)calculateArea;

@end

@implementation Square
- (id)initWithSide:(CGFloat)side {
   length = side;
   return self;
}

- (void)calculateArea {
   area = length * length;
}

- (void)printArea {
   NSLog(@"The area of square is %f", area);
}

@end

@interface Rectangle : Shape {
   CGFloat length;
   CGFloat breadth;
}

- (id)initWithLength:(CGFloat)rLength andBreadth:(CGFloat)rBreadth;
@end

@implementation Rectangle
- (id)initWithLength:(CGFloat)rLength andBreadth:(CGFloat)rBreadth {
   length = rLength;
   breadth = rBreadth;
   return self;
}

- (void)calculateArea {
   area = length * breadth;
}

@end

int main(int argc, const char * argv[]) {
   NSAutoreleasePool * pool = [[NSAutoreleasePool alloc] init];
   Shape *square = [[Square alloc]initWithSide:10.0];
   [square calculateArea];
   [square printArea];
   Shape *rect = [[Rectangle alloc]
   initWithLength:10.0 andBreadth:5.0];
   [rect calculateArea];
   [rect printArea];        
   [pool drain];
   return 0;
}
//output
2018-11-16 02:02:22.096 main[159689] The area of square is 100.000000
2018-11-16 02:02:22.098 main[159689] The area is 50.000000
```

### 类别、类拓展

#### **类别**Category

常用于给已知的类（Class）增加行为，注意类别会有隐式重名冲突的问题。当你已经封装好了一个类（也可能是系统类、第三方库），不想在改动这个类了，可是随着程序功能的增加需要在类中增加一个方法，这时我们不必修改主类，只需要给你原来的类增加一个分类。将一个大型的类拆分成不同的分类，在不同分类中实现类别声明的方法，这样可以将一个类的实现写到多个.m文件中，方便管理和协同开发。分类中的方法可以只声明，不实现，所以在协议不支持可选方法的时候（协议现在已经支持可选方法），通常把分类作为非正式协议使用。添加属性和成员变量的一种常见的办法是通过runtime.h中objc_getAssociatedObject / objc_setAssociatedObject来访问和生成关联对象。通过这种方法来模拟生成属性。

```objective-c
@interface XYZPerson (XYZPersonNameDisplayAdditions) 
-(NSString *)lastNameFirstNameString; 
@end
```

- 分类中方法的优先级比原来类中的方法高，也就是说，在分类中重写了原来类中的方法，那么分类中的方法会覆盖原来类中的方法
- 分类中只能声明方法，不能添加属性变量，在运行时分类中的方法与主类中的方法没有区别。
- 通常来讲，分类定义在.h文件中，但也可以定义.m文件中，此时分类的方法就变成私有方法

#### 拓展Extension

扩展是分类的一种特殊形式。拓展作用：

1.能为某个类附加额外的属性，成员变量，方法声明
2.一般的将类扩展直接写在.m文件中，而不单独建立类扩展文件
3.一般的私有属性和方法写到类扩展
4.和类别相似，但是小括号里面没有扩展的名字，就像匿名的类别

```objective-c
@interface 主类类名（）
@end
```

扩展通常定义在主类.m文件中，扩展中声明的方法直接在主类.m文件中实现。

**局限性：**

- 不能为任何类声明扩展，仅适用于原始实现源代码的类。

- 扩展是添加仅特定于类的私有方法和私有变量。

- 扩展内部声明的任何方法或变量即使对于继承的类也是不可访问的。

#### **类别和扩展的区别**

- 需不需要源码
- 能不能增加属性
- 分类是不可以声明实例变量，通常是公开的，文件名是：主类名+分类名.h
- 扩展是可以声明实例变量，是私有的，文件名为：主类名_扩展标识.h，在主类的.m文件中#import该头文件

### 协议

protocol协议的**基本用途**：

1. 可以用来声明一大堆方法（不能声明成员变量）
2. 只要某个类遵守了这个协议，就相当于拥有了这个协议中的所有方法声明。
3. 只要父类遵守了某个协议，就相当于子类也遵守了。

注意：协议内仅仅写方法声明，不能写实现，不能写成员变量

对协议的简单理解：

1. protocol声明的方法可以交给任何类去实现。

2. protocol的作用仅仅就是声明方法，所以新建协议就是.h文件。

3. @protocol关键字表示声明协议，同样以@end结尾。例如：@protocol  MyProtocol@end

4. @protocol声明的方法要交给类去实现，即类遵守协议。也就是说只要类遵守了这个协议，就相当于拥有了这个协议内的所有方法声明。（协议仅仅用来声明方法，以交给多个类去实现（去遵守））

5. 协议与分类一样只能写方法，不能声明成员变量。但是和分类不同的是协议只能写方法声明，分类是给某个类扩充一些方法

6. 协议遵守协议

   （1）一个类可以继承别的类，同样协议也可以遵守协议。

   （2）一个协议可以遵守多个协议，多个协议之间用逗号隔开

   （3）一个协议遵守了其他协议，就相当于拥有了其他协议中的所有方法声明。

```objective-c
1> 类遵守协议
@interface 类名：父类名 <协议名称1，协议名称2>

@end;
2> 协议遵守协议
@protocol 协议名称 <其他协议名称1，其他协议名称2>
//方法声明列表,,,,
@end
3> 协议中方法声明的关键字
	1>@required（默认） 要求实现，若没实现，将出现警告。
	2>@optional    不要求实现，实不实现都不会有警告。

```

### 内存管理

Objective-C内存管理技术大致可分为两类 - 

- “手动保留或释放”或MRR
- “自动参考计数”或ARC

在MRR中，通过跟踪自己的对象来明确管理内存。这是使用一个称为引用计数的模型实现的，`Foundation`类`NSObject`与运行时环境一起提供。MRR和ARC之间的唯一区别是保留和释放，前者是手动处理，而后者则自动处理。

#### MRR基本规则

- 拥有创建的任何对象：使用名称以“alloc”，“new”，“copy”或“mutableCopy”开头的方法创建对象
- 使用`retain`获取对象的所有权：通常保证接收到的对象在接收到的方法中保持有效，并且该方法也可以安全地将对象返回给它的调用者。在两种情况下使用`retain`-
  - 在访问器方法或`init`方法的实现中，获取想要存储为对象属性值的对象的所有权。
  - 防止对象因某些其他操作的副作用而失效。
- 当不再需要它时，必须放弃对拥有的对象的所有权：通过向对象发送释放消息或自动释放消息来放弃对象的所有权。 因此，在Cocoa术语中，放弃对象的所有权通常被称为“释放”对象。
- 不得放弃不拥有的对象的所有权。

#### “自动参考计数”或ARC

在自动引用计数或ARC中，系统使用与MRR相同的引用计数系统，但它在编译时为我们插入适当的内存管理方法调用。 强烈建议将ARC用于新项目。 如果使用ARC，通常不需要理解本文档中描述的底层实现，尽管在某些情况下它可能会有所帮助。 

如上所述，在ARC中，不需要添加`release`和`retain`方法，因为编译器会对此进行处理。 实际上，Objective-C的基本过程仍然是相同的。 它在内部使用保留和释放操作，使开发人员更容易编码而无需担心这些操作，这将减少写入的代码量和内存泄漏的可能性。

还有另一个原则叫做垃圾收集，它在Mac OS-X中与MRR一起使用，但由于它在OS-X Mountain Lion中的弃用，它还没有与MRR一起讨论过。 此外，iOS对象从未拥有垃圾收集功能。 使用ARC，OS-X中也没有使用垃圾收集。



