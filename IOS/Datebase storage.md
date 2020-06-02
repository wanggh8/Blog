---
title: 数据库设计
date: 2019-05-16 21:31:12
tags: IOS
---

## Document 相关操作

<!--more-->

```objective-c
//获得document
+(NSString *)documentsPath {
NSArray *paths = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);

return [paths objectAtIndex:0];
}

//读取工程文件
+(NSString *) ProductPath:(NSString*)filename{
    NSString *path = [[NSBundlemainBundle] pathForResource:filename ofType:@""];

    return  path;
}

//获得document文件路径，名字方便记忆
+(NSString *) DocumentPath:(NSString *)filename {
NSString *documentsPath = [self documentsPath];
    // NSLog(@"documentsPath=%@",documentsPath);
return [documentsPath stringByAppendingPathComponent:filename];
}

//获得document文件路径
+(NSString *)fullpathOfFilename:(NSString *)filename {
NSString *documentsPath = [self documentsPath];
   // NSLog(@"documentsPath=%@",documentsPath);
return [documentsPath stringByAppendingPathComponent:filename];
}

//写入文件沙盒位置NSDictionary
+(void)saveNSDictionaryForDocument:(NSDictionary *)list  FileUrl:(NSString*) FileUrl  {
    NSString *f = [self fullpathOfFilename:FileUrl];
[list writeToFile:f atomically:YES];
}

//写入文件存放到工程位置NSDictionary
+(void)saveNSDictionaryForProduct:(NSDictionary *)list  FileUrl:(NSString*) FileUrl  {
    NSString *ProductPath =[[NSBundlemainBundle]  resourcePath];
    NSString *f=[ProductPath stringByAppendingPathComponent:FileUrl];
[list writeToFile:f atomically:YES];
}

//写入文件 Array 工程
+(void)saveOrderArrayListProduct:(NSMutableArray *)list  FileUrl :(NSString*) FileUrl {
    NSString *ProductPath =[[NSBundlemainBundle]  resourcePath];
		NSString *f=[ProductPath stringByAppendingPathComponent:FileUrl];
[list writeToFile:f atomically:YES];
}

//写入文件 Array 沙盒
+(void)saveOrderArrayList:(NSMutableArray *)list  FileUrl :(NSString*) FileUrl {
NSString *f = [self fullpathOfFilename:FileUrl];
[list writeToFile:f atomically:YES];
}

//加载文件沙盒NSDictionary
+(NSDictionary *)loadNSDictionaryForDocument  : (NSString*) FileUrl {
NSString *f = [self fullpathOfFilename:FileUrl];
NSDictionary *list = [ [NSDictionaryalloc] initWithContentsOfFile:f];
return [list autorelease];
}

//加载文件工程位置NSDictionary
+(NSDictionary *)loadNSDictionaryForProduct   : (NSString*) FileUrl {
		NSString *f = [self ProductPath:FileUrl];
		NSDictionary *list =[NSDictionarydictionaryWithContentsOfFile:f];
return list;
}

//加载文件沙盒NSArray
+(NSArray *)loadArrayList   : (NSString*) FileUrl {

 	NSString *f = [self fullpathOfFilename:FileUrl];
	NSArray *list = [NSArray  arrayWithContentsOfFile:f];
return list;
}

//加载文件工程位置NSArray
+(NSArray *)loadArrayListProduct   : (NSString*) FileUrl {
		NSString *f = [self ProductPath:FileUrl];
		NSArray *list = [NSArray  arrayWithContentsOfFile:f];
return list;
}

//拷贝文件到沙盒
+(int) CopyFileToDocument:(NSString*)FileName{
		NSString *appFileName =[self fullpathOfFilename:FileName];
		NSFileManager *fm = [NSFileManagerdefaultManager];  
		//判断沙盒下是否存在 
    BOOL isExist = [fm fileExistsAtPath:appFileName];  
    if (!isExist)   //不存在，把工程的文件复制document目录下
    {  
        //获取工程中文件
        NSString *backupDbPath = [[NSBundle mainBundle]  
                                  pathForResource:FileName  
                                  ofType:@""];    
       	//这一步实现数据库的添加，  
      	// 通过NSFileManager 对象的复制属性，把工程中数据库的路径复制到应用程序的路径上  
        BOOL cp = [fm copyItemAtPath:backupDbPath toPath:appFileName error:nil];  
        return cp;
    } else {
        return  -1; //已经存在
    } 
}

//判断文件是否存在
+(BOOL) FileIsExists:(NSString*) checkFile{
    if([[NSFileManagerdefaultManager]fileExistsAtPath:checkFile])
    {
        return true;
    }
    return  false;
}
```

## ios单例模式

 		单例模式主要实现唯一实例，存活于整个程序范围内，一般存储用户信息经常用到单例，比如用户密码，密码在登录界面用一次，在修改密码界面用一次，而使用单例，就能保证密码唯一实例。如果不用单例模式，init 两个的实例的堆栈地址不一样，所以存放的数据的位置也不一样，当其中一个数据改变，另一个数据依然不变。

```objective-c
#ifndef Singleton_h
#define Singleton_h

@interface Singleton : NSObject
@property (nonatomic, copy) NSString *pass;
+ (Singleton *) sharedInstance;

@end
```

```objective-c
#import <Foundation/Foundation.h>
#import "Singleton.h"

@implementation Singleton
static id sharedSingleton = nil;
+ (id)allocWithZone:(struct _NSZone *)zone
{
if (sharedSingleton == nil) {
static dispatch_once_t onceToken;
dispatch_once(&onceToken, ^{
sharedSingleton = [super allocWithZone:zone];    
});
}
return sharedSingleton;
}

- (id)init
{
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        sharedSingleton = [super init];
    });
    return sharedSingleton;
}

+ (instancetype)sharedInstance
{
return [[self alloc] init];
}
+ (id)copyWithZone:(struct _NSZone *)zone
{
return sharedSingleton;
}
+ (id)mutableCopyWithZone:(struct _NSZone *)zone
{
return sharedSingleton;
}

@end
```

```objective-c
+(UserDao *)sharedUserDao;
@implementation UserDao

static UserDao *userFmdb = nil;
+(instancetype)allocWithZone:(struct _NSZone *)zone{
    
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        
        if (userFmdb == nil) {
            userFmdb = [[UserDao alloc] init];
            //创建数据库
            [userFmdb createDataBase];
            //创建表
            [userFmdb createTable];
        }
        
    });
    
    return userFmdb;
    
}

+(UserDao *)shareduserDao{
    
    return [[self alloc]init];
    
}

-(id)copyWithZone:(NSZone *)zone
{
    return userFmdb;
}
-(id)mutableCopyWithZone:(NSZone *)zone
{
    return userFmdb;
}

```



## iOS 图片本地存储、本地获取、本地删除

```objective-c
//将图片保存到本地

\+ (void)SaveImageToLocal:(UIImage*)image Keys:(NSString*)key {

    

    //首先,需要获取沙盒路径

    NSString *picPath=[NSString stringWithFormat:@"%@/Documents/%@.png",NSHomeDirectory(),key];

    

    NSLog(@"将图片保存到本地  %@",picPath);

    

    BOOL isHaveImage = [self LocalHaveImage:key];

    if (isHaveImage) {

        NSLog(@"本地已经保存该图片、无需再次存储...");

        return ;

    }

    

    NSData *imgData = UIImageJPEGRepresentation(image,0.5);

    [imgData writeToFile:picPath atomically:YES];    

}


//从本地获取图片

\+ (UIImage*)GetImageFromLocal:(NSString*)key {    

    if ([JKBlankTool isBlankString:key]) {
        return nil;
    }
    

    //读取本地图片非resource

    NSString *picPath=[NSString stringWithFormat:@"%@/Documents/%@.png",NSHomeDirectory(),key];

    NSLog(@"获取图片   %@",picPath); 

    UIImage *img=[[UIImage alloc]initWithContentsOfFile:picPath];

    return img;   

}

//本地是否有图片

\+ (BOOL)LocalHaveImage:(NSString*)key {
  

    if ([JKBlankTool isBlankString:key]) {
        return NO;
    }
  
    //读取本地图片非resource

   NSString *picPath=[NSString stringWithFormat:@"%@/Documents/%@.png",NSHomeDirectory(),key];  

   NSLog(@"查询是否存在 %@",picPath);
   

    UIImage *img=[[UIImage alloc]initWithContentsOfFile:picPath];   

   if (img) {

        return YES;
    }

    return NO;  

}


//将图片从本地删除

\+ (void)RemoveImageToLocalKeys:(NSString*)key {

    

    NSString *picPath=[NSString stringWithFormat:@"%@/Documents/%@.png",NSHomeDirectory(),key];

    NSLog(@"将图片从本地删除  %@",picPath);

    [[NSFileManager defaultManager] removeItemAtPath:picPath error:nil];

}
```

## iOS SQLite存取图片、视频、音频

#### 1、设计数据库

数据库的表的结构：m_data 类型是blob用来存储NSData二进制数据

建表语句：

`CREATE TABLE myTable(m_id integer primary key autoincrement not null,m_name text,m_type text,m_time text,m_data blob)`

### 2、上传图片、视频、音频到数据库

#### 向数据库中添加数据的方法：
```    objective-c
//添加图片、音乐、视频(数据类型、数据名、数据)
-(void)insertWithDataName:(NSString*)dataName DataType:(NSString*)dataType data:(NSData*)data{

    [self openDB];
    NSString *sqlString=@"insert into myTable (m_name ,m_type,m_data,m_time) values (?,?,?,?)";
  
    sqlite3_stmt *stmt=NULL;
      
    int result = sqlite3_prepare(db, sqlString.UTF8String, -1, &stmt, NULL);
    
    if (result==SQLITE_OK) {
        
        NSLog(@"预执行正确");
        
        //绑定参数
        //第1个问号
        sqlite3_bind_text(stmt, 1, dataName.UTF8String, -1, NULL);
        //第2个问号
        sqlite3_bind_text(stmt, 2, dataType.UTF8String, -1, NULL);
        //第3个问号
        sqlite3_bind_blob(stmt, 3, [data bytes], (int)[data length], NULL);
        //第4个问号
        sqlite3_bind_text(stmt, 4, [self getNowDate].UTF8String, -1, NULL);
        
        //执行
        if(sqlite3_step(stmt)==SQLITE_DONE)
            NSLog(@"插入成功");
        else NSLog(@"插入失败");
        
    }
    else NSLog(@"预执行错误");

    sqlite3_finalize(stmt);
    [self closeDB]; 
}
```
#### 调用方法添加数据：
```objective-c
//将数据写入到数据库
-(void)button2Action:(UIButton*)sender{
    NSLog(@"添加数据");

     NSBundle *mainBundle =[NSBundle mainBundle];
    
    NSLog(@"添加 芦田爱菜.png");
    NSString *picPath = [mainBundle pathForResource:@"芦田爱菜" ofType:@"png"];
    NSData *dataPic = [[NSData alloc]initWithContentsOfFile:picPath];
    
    if (![[MyDataBaseHandle shareDatabase]checkWithDataName:@"芦田爱菜" DataType:@"png"]) {
         [[MyDataBaseHandle shareDatabase] insertWithDataName:@"芦田爱菜" DataType:@"png" data:dataPic];
    }
    else {
         [[MyDataBaseHandle shareDatabase] updateWithDataName:@"芦田爱菜" DataType:@"png" data:dataPic];
    }

     NSLog(@"添加 平凡之路-片段.mp3");
    NSString *MP3Path = [mainBundle pathForResource:@"平凡之路-片段" ofType:@"mp3"];
       
    NSData *dataMP3 = [[NSData alloc]initWithContentsOfFile:MP3Path];
     
    if (![[MyDataBaseHandle shareDatabase]checkWithDataName:@"平凡之路-片段" DataType:@"mp3"]) {
        [[MyDataBaseHandle shareDatabase] insertWithDataName:@"平凡之路-片段" DataType:@"mp3" data:dataMP3];
    }
    else {
        [[MyDataBaseHandle shareDatabase] updateWithDataName:@"平凡之路-片段" DataType:@"mp3" data:dataMP3];
    }

   
    NSLog(@"添加 SNH 赵粤.mp4");
     NSString *MP4Path = [mainBundle pathForResource:@"SNH 赵粤" ofType:@"mp4"];
     NSData *dataMP4 = [[NSData alloc]initWithContentsOfFile:MP4Path];
    
    if (![[MyDataBaseHandle shareDatabase]checkWithDataName:@"SNH 赵粤" DataType:@"mp4"]) {
        [[MyDataBaseHandle shareDatabase] insertWithDataName:@"SNH 赵粤" DataType:@"mp4" data:dataMP4];
    }
    else {
        
        [[MyDataBaseHandle shareDatabase] updateWithDataName:@"SNH 赵粤" DataType:@"mp4" data:dataMP4];
    }

}
```


### 3、下载图片、视频、音频，并存入本地文件夹

#### 从数据库获取数据的方法：
```objective-c
//查
-(NSData *)selectWithDataName:(NSString*)dataName DataType:(NSString*)dataType{
    
    [self openDB];
    NSData * data = nil;
    NSString *sqlString=@"select m_data from myTable where m_name=? and m_type = ?";
    
    //2.伴随指针
    sqlite3_stmt *stmt=NULL;
    
    //3.预执行
    int result = sqlite3_prepare(db, sqlString.UTF8String, -1, &stmt, NULL);
    
    if (result==SQLITE_OK) {
        
        NSLog(@"语句正确");
        
        //4.绑定参数
        sqlite3_bind_text(stmt, 1, dataName.UTF8String, -1, NULL);
         sqlite3_bind_text(stmt, 2, dataType.UTF8String, -1, NULL);
        
        //5.执行(N次,因为有可能有N条数据符合条件)//循环判断是否还有符合查询条件的数据
        while (sqlite3_step(stmt)==SQLITE_ROW) {
            
           data =[[NSData alloc] initWithBytes:sqlite3_column_blob(stmt, 0) length: sqlite3_column_bytes(stmt, 0)];
            
        }
        
    }
    else NSLog(@"语句错误");
    
    //6.关闭伴随指针
    sqlite3_finalize(stmt);
  [self closeDB];
    return data;
}
```

#### 调用方法，并将数据存储到本地文件夹：
```objective-c
//从数据库获取数据-并存储到本地文件夹
-(void)button3Action:(UIButton*)sender{

    NSString * destDateString =[self getNowDate];

    NSLog(@"搜索数据");

    ////获取png图片并写到本地
    NSData * data2=[[MyDataBaseHandle shareDatabase] selectWithDataName:@"芦田爱菜" DataType:@"png"];
       
    UIImage *image2 = [UIImage imageWithData:data2];
    [self.myIMV setImage:image2];
    
    NSString  *filePic=  [self.filesPath stringByAppendingPathComponent:[NSString stringWithFormat:@"/%@芦田爱菜.png",destDateString]];
    [data2 writeToFile:filePic atomically:YES];


    //获取Mp3并写到本地
   NSData * data3=[[MyDataBaseHandle shareDatabase] selectWithDataName:@"平凡之路-片段" DataType:@"mp3"];
    
    NSString  *filemp3=  [self.filesPath stringByAppendingPathComponent:[NSString stringWithFormat:@"/%@平凡之路-片段.mp3",destDateString]];
    [data3 writeToFile:filemp3 atomically:YES];

 
    //获取Mp4并写到本地
    NSData * data4=[[MyDataBaseHandle shareDatabase] selectWithDataName:@"SNH 赵粤" DataType:@"mp4"];
    
    NSString  *filemp4=  [self.filesPath stringByAppendingPathComponent:[NSString stringWithFormat:@"/%@SNH 赵粤.mp4",destDateString]];
    
    [data4 writeToFile:filemp4 atomically:YES];

}
```