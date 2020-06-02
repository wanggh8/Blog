---
title:  基础框架
date: 2019-04-11 22:31:12
tags: IOS
---

## 数据存储

数据存储及检索是任何程序中最重要的一个操作

<!--more--> 

### NSArray和NSMutableArray

`NSArray`用于保存不可变对象数组，`NSMutableArray`用于保存可变对象数组。
`Mutablility`有助于在运行时更改预分配数组中的数组，但如果使用`NSArray`，只替换现有数组，并且不能更改现有数组的内容。

`NSArray`的重要方法如下 -

- `alloc/initWithObjects` − 用于使用对象初始化数组。
- `objectAtIndex` − 返回指定索引处的对象。
- `count` − 返回对象数量。

`NSMutableArray`继承自`NSArray`，因此`NSArray`的所有实例方法都可在`NSMutableArray`中使用

`NSMutableArray`的重要方法如下 -

- `removeAllObjects` − 清空数组。
- `addObject` − 在数组的末尾插入给定对象。
- `removeObjectAtIndex` − 这用于删除`objectAt`指定的索引处的对象。
- `exchangeObjectAtIndex:withObjectAtIndex` − 在给定索引处交换数组中的对象。
- `replaceObjectAtIndex:withObject` − 用Object替换索引处的对象。

```objective-c
// 访问元素
    NSArray * arr1 = @[@"one",@"two",@"three"];

    //用下标运算符访问某个元素

    NSLog(@"arr[1] is %@", arr1[1]);
    //调方法访问某个元素

    NSLog(@"objectAtIndex:1  == %@", [arr1 objectAtIndex:1]);
    //获取元素数量

    NSLog(@"arr1的元素个数为: %lu", [arr1 count]);

    //遍历数组对象

    //1,  用循环和下标运算符遍历

    NSUInteger i;

    for (i=0; i<[arr1 count]; i++) {

        NSLog(@"arr[%lu]=%@", i,arr1[i]);

    }

    //2,  用循环和方法objectAtIndex： 完成遍历

    for (i=0; i<[arr1 count]; i++) {

        NSLog(@"[arr1 objectAtIndex:%lu] == %@", (unsigned long)i, [arr1 objectAtIndex:i]);

    }

    //3,  快速枚举法

    for( id obj in arr1)    // for(int i=0;i<10;i++)

    {

        NSLog(@"快速枚举法：obj=[%@]",obj);

    }

    //4,枚举器法,  首先认识一种枚举器对象

    //               枚举器对象        这个方法是获取一个数组的枚举器

    //                                  正序枚举器

    NSEnumerator * enumerator =[arr1 objectEnumerator];

    id obj;

    while (obj = [enumerator nextObject]) {

        NSLog(@"正序枚举器法遍历: obj == %@", obj);

    }

    //                                     逆序枚举器

    NSEnumerator *enumerator2 = [arr1 reverseObjectEnumerator];

   

    while (obj = [enumerator2 nextObject]) {

        NSLog(@"逆序枚举器法遍历: obj == %@", obj);

    }

**********************************************

//数组排序

 // 要回调的函数

NSInteger compareTwoObjects(id obj1, id obj2, void * args)

{

//    struct student * stu = args;

//    printf("args:%s\n",args);

    //1,比较字符串大小

//    return [obj1 compare:obj2];

    //2, 比较字符串的长度

    return [obj1 length]-[obj2 length]; 

    //3, 将字符串转成整型，再比较整型的大小

//    return [obj1 integerValue]-[obj2 integerValue];

}

  NSArray *array = [[ NSArray alloc]initWithObjects:@"123",@"4567",@"34",@"111", @"x",nil];

    //  对数组的元素进行排序: 传进来的是一个回调函数，用于处理比较细节

   NSArray * newArray =  [array sortedArrayUsingFunction: compareTwoObjects context:"testString"];
```

### NSDictionary和NSMutableDictionary

`NSDictionary`用于保存对象的不可变字典，`NSMutableDictionary`用于保存对象的可变字典。

`NSDictionary`的一些重要方法如下 -

- `alloc/initWithObjectsAndKeys` − 使用指定的值和键集构造的条目初始化新分配的字典。
- `valueForKey` − 返回与给定键关联的值。
- `count` − 返回字典中的项目数。

`NSMutableDictionary`继承自`NSDictionary`，因此`NSDictionary`的所有实例方法都可以在`NSMutableDictionary`中使用。`NSMutableDictionary`的重要方法如下 -

- `removeAllObjects` - 清空字典的条目。
- `removeObjectForKey` - 从字典中删除给定键及其关联值。
- `setValue：forKey` - 将给定的键值对添加到字典中。

```objective-c
#import <Foundation/Foundation.h>

int main() {
   NSAutoreleasePool * pool = [[NSAutoreleasePool alloc] init];
   NSDictionary *dictionary = [[NSDictionary alloc] initWithObjectsAndKeys:
   @"string1",@"key1", @"string2",@"key2",@"string3",@"key3",nil];
   NSString *string1 = [dictionary objectForKey:@"key1"];
   NSLog(@"The object for key, key1 in dictionary is %@",string1);

   NSMutableDictionary *mutableDictionary = [[NSMutableDictionary alloc]init];
   [mutableDictionary setValue:@"string" forKey:@"key1"];
   string1 = [mutableDictionary objectForKey:@"key1"];
   NSLog(@"The object for key, key1 in mutableDictionary is %@",string1); 

   [pool drain];
   return 0;
}
```

### NSSet和NSMutableSet

`NSSet`用于保存不可变的一组不同的对象，`NSMutableDictionary`用于保存一组可变的不同对象。`NSSet`的重要方法如下 -

- `alloc/initWithObjects` - 使用从指定的对象列表中获取的成员初始化新分配的集合。
- `allObjects` - 返回包含集合成员的数组，如果集合没有成员，则返回空数组。
- `count` - 返回集合中的成员数量。

`NSMutableSet`继承自`NSSet`类，因此NSSet的所有实例方法都可以在`NSMutableSet`中使用。

`NSMutableSet`的一些重要方法如下 -

- `removeAllObjects` − 清空集合的所有成员。
- `addObject` − 如果给定对象尚未成为成员，则将该对象添加到该集合中。
- `removeObject` − 从集合中删除给定对象。

```objective-c
#import <Foundation/Foundation.h>

int main() {
   NSAutoreleasePool * pool = [[NSAutoreleasePool alloc] init];
   NSSet *set = [[NSSet alloc]
   initWithObjects:@"yii", @"bai",@".com",nil];
   NSArray *setArray = [set allObjects];
   NSLog(@"The objects in set are %@",setArray);

   NSMutableSet *mutableSet = [[NSMutableSet alloc]init];
   [mutableSet addObject:@"yiibai"];
   setArray = [mutableSet allObjects];
   NSLog(@"The objects in mutableSet are %@",setArray);

   [pool drain];
   return 0;
}
```

## 文本和字符串

NSString是最常用的类，用于存储字符串和文本,`NSCharacterSet`表示`NSString`和`NSScanner`类使用的各种字符分组

### NSCharacterSet

`NSCharacterSet`中可用的方法集，它们表示各种字符集。

- `alphanumericCharacterSet` - 返回包含“字母”，“标记”和“数字”类别中的字符的字符集。
- `capitalizedLetterCharacterSet` - 返回包含首字母大写字母类别中字符的字符集。
- `characterSetWithCharactersInString` - 返回包含给定字符串中字符的字符集。
- `characterSetWithRange` - 返回包含给定范围内具有`Unicode`值的字符的字符集。
- `illegalCharacterSet` - 返回一个字符集，其中包含非字符类别中的值或尚未在`Unicode`标准的`3.2`版中定义的值。
- `letterCharacterSet` - 返回包含`Letters`和`Marks`类别中字符的字符集。
- `lowercaseLetterCharacterSet` - 返回包含“小写字母”类别中字符的字符集。
- `newlineCharacterSet` - 返回包含换行符的字符集。
- `punctuationCharacterSet` - 返回包含标点符号类别中字符的字符集。
- `symbolCharacterSet` - 返回包含符号类别中字符的字符集。
- `uppercaseLetterCharacterSet` - 返回包含大写字母和标题字母类别中字符的字符集。
- `whitespaceAndNewlineCharacterSet` - 返回包含Unicode一般类别 `Z*`，`U000A~U000D`和`U0085`的字符集。
- `whitespaceCharacterSet` - 返回仅包含内嵌空白字符空间(`U+0020`)和制表符(`U+0009`)的字符集。

```objective-c
   NSAutoreleasePool * pool = [[NSAutoreleasePool alloc] init];
   NSString *string = @"....Yii Bai.com.....";
   NSLog(@"Initial String :%@", string);

   NSCharacterSet *characterset = [NSCharacterSet punctuationCharacterSet];
   string = [string stringByTrimmingCharactersInSet:characterset];
   NSLog(@"Final String :%@", string);
```

## 日期和时间

在Objective-C中，`NSDate`和`NSDateFormatter`类用于提供日期和时间的功能。
`NSDateFormatter`是一个帮助程序类，可以很容易地将`NSDate`转换为`NSString`，反之亦然。

显示将`NSDate`转换为`NSString`并返回`NSDate`的简单示例如下所示 -

```objective-c
   NSAutoreleasePool * pool = [[NSAutoreleasePool alloc] init];
   NSDate *date= [NSDate date];
   NSDateFormatter *dateFormatter = [[NSDateFormatter alloc]init];
   [dateFormatter setDateFormat:@"yyyy-MM-dd"];

   NSString *dateString = [dateFormatter stringFromDate:date];
   NSLog(@"Current date is %@",dateString);
   NSDate *newDate = [dateFormatter dateFromString:dateString];
   NSLog(@"NewDate: %@",newDate);
```

## 异常处理

在Objective-C中提供了基础类 - `NSException`用于异常处理。

使用以下块实现异常处理 -

- `@try` - 此块尝试执行一组语句。
- `@catch` - 此块尝试捕获`try`块中的异常。
- `@finally` - 此块包含始终执行的一组语句。

```objective-c
#import <Foundation/Foundation.h>

int main() {
   NSAutoreleasePool * pool = [[NSAutoreleasePool alloc] init];
   NSMutableArray *array = [[NSMutableArray alloc]init];        

   @try  {
      NSString *string = [array objectAtIndex:10];
   } @catch (NSException *exception) {
      NSLog(@"%@ ",exception.name);
      NSLog(@"Reason: %@ ",exception.reason);
   }

   @finally  {
      NSLog(@"@@finaly Always Executes");
   }

   [pool drain];
   return 0;
}
```

## 文件处理

使用`NSFileManager`类进行文件处理

### 文件处理中使用的方法

下面列出了用于访问和操作文件的方法列表。 在这里，必须将`FilePath1`，`FilePath2`和`FilePath`字符串替换为所需的完整文件路径，以获得所需的操作。

#### 1. 检查文件是否存在于路径中

```objectivec
NSFileManager *fileManager = [NSFileManager defaultManager];

//Get documents directory
NSArray *directoryPaths = NSSearchPathForDirectoriesInDomains
(NSDocumentDirectory, NSUserDomainMask, YES);
NSString *documentsDirectoryPath = [directoryPaths objectAtIndex:0];

if ([fileManager fileExistsAtPath:@""] == YES) {
   NSLog(@"File exists");
}
```

#### 2. 比较两个文件内容

```objectivec
if ([fileManager contentsEqualAtPath:@"FilePath1" andPath:@" FilePath2"]) {
   NSLog(@"Same content");
}
```

#### 3. 检查是否可写，可读和可执行

```objectivec
if ([fileManager isWritableFileAtPath:@"FilePath"]) {
   NSLog(@"isWritable");
}

if ([fileManager isReadableFileAtPath:@"FilePath"]) {
   NSLog(@"isReadable");
}

if ( [fileManager isExecutableFileAtPath:@"FilePath"]) {
   NSLog(@"is Executable");
}
```

#### 4. 移动文件

```objectivec
if([fileManager moveItemAtPath:@"FilePath1" 
   toPath:@"FilePath2" error:NULL]) {
      NSLog(@"Moved successfully");
}
```

#### 5. 复制文件

```objectivec
if ([fileManager copyItemAtPath:@"FilePath1" 
   toPath:@"FilePath2"  error:NULL]) {
      NSLog(@"Copied successfully");
   }
```

#### 6. 删除文件

```objectivec
if ([fileManager removeItemAtPath:@"FilePath" error:NULL]) {
   NSLog(@"Removed successfully");
}
```

#### 7. 读取文件

```objectivec
NSData *data = [fileManager contentsAtPath:@"Path"];
```

#### 8. 写入文件

```objectivec
[fileManager createFileAtPath:@"" contents:data attributes:nil];
```

