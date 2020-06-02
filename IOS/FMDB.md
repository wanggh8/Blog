---
title: iOS开发数据库
date: 2019-05-09 22:31:12
tags: IOS
---

## FMDB

<!--more-->

### 简介

**定义**

- FMDB是iOS平台的开源                            SQLite数据库框架

- FMDB以OC的方式封装了SQLite的C语言API

**优点**

- 使用起来更加面向对象，省去了很多麻烦、冗余的C语言代码
- 对比苹果自带的Core Data框架，更加轻量级和灵活
- 提供了多线程安全的数据库操作方法，有效地防止数据混乱

### 结构

**主要的类**

- `FMDatabase`：一个``FMDatabase`对象就代表一个单独的SQLite数据库用来执行SQL语句
- `FMResultSet`：使用``FMDatabase`执行查询后的结果集
- `FMDatabaseQueue`：用于在多线程中执行多个查询或更新，是线程安全的

### 数据库使用

#### 创建数据库

通过指定SQLite数据库文件路径来创建`FMDatabase`对象

```objective-c
FMDatabase *db = [FMDatabase databaseWithPath:path];

if (![db open]) {

    NSLog(@"数据库打开失败！");

}
```

文件路径有三种情况

- 具体文件路径：如果不存在会自动创建
-  空字符串@""：会在临时目录创建一个空的数据库，当``FMDatabase`连接关闭时，数据库文件也被删除 
- nil：会创建一个内存中临时数据库，当`FMDatabase`连接关闭时，数据库会被销毁

#### 更新数据库

create、drop、insert、update、delete

 使用``executeUpdate`:方法执行更新

```objective-c
- (BOOL)executeUpdate:(NSString*)sql, ...

- (BOOL)executeUpdateWithFormat:(NSString*)format, ...

- (BOOL)executeUpdate:(NSString*)sql withArgumentsInArray:(NSArray *)arguments

[db executeUpdate:@"UPDATE t_student SET age = ? WHERE name = ?;", @20, @"Jack"]
```

#### 查询数据库

```objective-c
- (FMResultSet *)executeQuery:(NSString*)sql, ...

- (FMResultSet *)executeQueryWithFormat:(NSString*)format, ...

- (FMResultSet *)executeQuery:(NSString *)sql withArgumentsInArray:(NSArray *)arguments
    
// 查询数据
FMResultSet *rs = [db executeQuery:@"SELECT * FROM t_student"];

// 遍历结果集
while ([rs next]) {
    NSString *name = [rs stringForColumn:@"name"];
    int age = [rs intForColumn:@"age"];
    double score = [rs doubleForColumn:@"score"];
}
```

### 操作

```objective-c
//1.获取数据库文件的路径
_docPath = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) lastObject];
NSLog(@"%@",_docPath);
mark_student = 1;
//设置数据库名称
NSString *fileName = [_docPath stringByAppendingPathComponent:@"数据库名称.sqlite"];

//2.获取数据库
 _db = [FMDatabase databaseWithPath:fileName];
if ([_db open]) {
    NSLog(@"打开数据库成功");
} else {
    NSLog(@"打开数据库失败");
}

//3.创建表
BOOL result = [_db executeUpdate:@"CREATE TABLE IF NOT EXISTS t_student(表名称) (id integer PRIMARY KEY AUTOINCREMENT, name text NOT NULL, age integer NOT NULL, sex text NOT NULL);"];
if (result) {  
  NSLog(@"创建表成功");
} else {
  NSLog(@"创建表失败");
}

//插入数据
//1.executeUpdate:不确定的参数用？来占位（后面参数必须是oc对象，；代表语句结束）
BOOL result = [_db executeUpdate:@"INSERT INTO t_student(表名) (name, age, sex) VALUES (?,?,?)",name,@(age),sex];
//2.executeUpdateWithForamat：不确定的参数用%@，%d等来占位 （参数为原始数据类型，执行语句不区分大小写）
    BOOL result = [_db executeUpdateWithFormat:@"insert into t_student(表名) (name,age, sex) values (%@,%i,%@)",name,age,sex];
//3.参数是数组的使用方式
    BOOL result = [_db executeUpdate:@"INSERT INTO t_student(表名)(name,age,sex) VALUES  (?,?,?);" withArgumentsInArray:@[name,@(age),sex]];
if (result) {
    NSLog(@"插入成功");
} else {
    NSLog(@"插入失败");
}

//删除数据
//1.不确定的参数用？来占位 （后面参数必须是oc对象,需要将int包装成OC对象）
int idNum = 11;
BOOL result = [_db executeUpdate:@"delete from t_student where id = ?",@(idNum)];
//2.不确定的参数用%@，%d等来占位
//BOOL result = [_db executeUpdateWithFormat:@"delete from t_student where name = %@",@"王二"];
if (result) {
    NSLog(@"删除成功");
} else {
    NSLog(@"删除失败");
}

//修改数据
//修改学生的名字
NSString *newName = @"张三";
NSString *oldName = @"王二";
BOOL result = [_db executeUpdate:@"update t_student set name = ? where name = ?",newName,oldName];
if (result) {
  NSLog(@"修改成功");
} else {
  NSLog(@"修改失败");
}

//查询数据
//查询整个表
FMResultSet * resultSet = [_db executeQuery:@"select * from t_student"];
//根据条件查询
//FMResultSet * resultSet = [_db executeQuery:@"select * from t_student where id < ?", @(4)];
//遍历结果集合
while ([resultSet next]) {
     int idNum = [resultSet intForColumn:@"id"];
     NSString *name = [resultSet objectForColumnName:@"name"];
     int age = [resultSet intForColumn:@"age"];
     NSString *sex = [resultSet objectForColumnName:@"sex"];
     NSLog(@"学号：%@ 姓名：%@ 年龄：%@ 性别：%@",@(idNum),name,@(age),sex);
}

//删除表
//如果表格存在 则销毁
BOOL result = [_db executeUpdate:@"drop table if exists t_student"];
if (result) {
  NSLog(@"删除表成功");
} else {
  NSLog(@"删除表失败");
}

//关闭数据库
BOOL result = [db close];
if(result){
NSLog(@"数据库关闭成功");
}
else {
NSLog(@"数据库关闭失败");
}
```

