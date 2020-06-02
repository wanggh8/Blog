---
title: UI编程
date: 2019-03-27 22:31:12
tags: IOS
---

## 常用控件

<!--more-->

### UILabel

```objective-c
// 创建对象
UILabel *label = [[UILabel alloc] init];
// frame
label.frame = CGRectMake(100, 100, 300, 75);
// 颜色
label.backgroundColor = [UIColor redColor];
// 内容
label.text = @"dashuaibi";
// 对齐方式
label.textAlignment =  NSTextAlignmentLeft;
// 字体大小
label.font = [UIFont systemFontOfSize:20.f];
label.font = [UIFont boldSystemFontOfSize:20.f];
label.font = [UIFont italicSystemFontOfSize:20.f];
// 文字颜色
label.textColor = [UIColor purpleColor];
// 设置阴影
label.shadowColor = [UIColor blueColor];
label.shadowOffset = CGSizeMake(-2, 1);
// 行数 0为自动换行
label.numberOfLines = 0;
// 显示模式
label.lineBreakMode = NSLineBreakByWordWrapping;

    UILabel *label = [[UILabel alloc] initWithFrame:CGRectMake(20, 100, 300, 100)];
    label.text = @"UILabel是继承自UIView的一个显示控件，基本是除了UIView之外使用最广泛的控件";
    label.numberOfLines = 3;
    label.textAlignment = NSTextAlignmentCenter;
    label.font = [UIFont boldSystemFontOfSize:16];
    label.textColor = [UIColor blueColor];
    label.backgroundColor = [UIColor grayColor];
    [label sizeToFit];
    [self.view addSubview:label];
		//超文本
    UILabel *label = [[UILabel alloc] initWithFrame:CGRectMake(20, 100, 300, 100)];
    UIFont *bigFont = [UIFont boldSystemFontOfSize:36];
    UIFont *smallFont = [UIFont boldSystemFontOfSize:bigFont.pointSize / 2];
    NSMutableAttributedString *attributeString6 = [[NSMutableAttributedString alloc] init];
    NSAttributedString *string1 = [[NSAttributedString alloc] initWithString:@"￥"
                                                                  attributes:@{NSFontAttributeName:smallFont,
                                                                               NSForegroundColorAttributeName:[UIColor blackColor],
                                                                               NSBaselineOffsetAttributeName:@(0)}];
    NSAttributedString *string2 = [[NSAttributedString alloc] initWithString:@"9"
                                                                  attributes:@{NSFontAttributeName:bigFont,
                                                                               NSForegroundColorAttributeName:[UIColor blueColor],
                                                                               NSBaselineOffsetAttributeName:@(0)}];
    NSAttributedString *string3 = [[NSAttributedString alloc] initWithString:@".99"
                                                                  attributes:@{NSFontAttributeName:smallFont,
                                                                               NSForegroundColorAttributeName:[UIColor blackColor],
                                                                               NSBaselineOffsetAttributeName : @(0)}];
    [attributeString6 appendAttributedString:string1];
    [attributeString6 appendAttributedString:string2];
    [attributeString6 appendAttributedString:string3];
    label.attributedText = attributeString6;
    [self.view addSubview:label];
```

### UIImageView

```objective-c
//初始化
UIImage *image = [UIImage imageNamed:@"test.png"];
UIImageView  *imageView=[[UIImageView alloc] initWithImage:image];                       
imageView.frame = CGRectMake(100, 200, 120, 120);
 
//设置图片 UIImage
 
//第一种：
[imageView setImage:[UIImage imageNamed:@"1.jpeg"]];
 
//第二种：
NSString *filePath=[[NSBundle mainBundle] pathForResource:@"1" ofType:@"jpeg"];
UIImage *images=[UIImage imageWithContentsOfFile:filePath];
[imageView setImage:images]; 
 
//第三种：
NSData *data=[NSData dataWithContentsOfFile:filePath];
UIImage *image2=[UIImage imageWithData:data];
[imageView setImage:image2];

		UIImage *image = [UIImage imageNamed:@"test.png"];
    {
        UIImageView *imageView = [[UIImageView alloc] initWithImage:image];
        imageView.frame = CGRectMake(20, 100, 200, 200);
        //    imageView.contentMode = UIViewContentModeScaleAspectFill;
        imageView.clipsToBounds = YES;
        imageView.backgroundColor = [UIColor grayColor];
        [self.view addSubview:imageView];
    }
    
    {
        UIImageView *imageView = [[UIImageView alloc] initWithImage:image];
        imageView.frame = CGRectMake(20, 100+200+10, 200, 200);
        imageView.contentMode = UIViewContentModeScaleAspectFit;
        imageView.clipsToBounds = YES;
        imageView.backgroundColor = [UIColor grayColor];
        [self.view addSubview:imageView];
    }
    
    {
        UIImageView *imageView = [[UIImageView alloc] initWithImage:image];
        imageView.frame = CGRectMake(20, 100+200+10 + 200 + 10, 200, 200);
        imageView.contentMode = UIViewContentModeScaleAspectFill;
        imageView.clipsToBounds = YES;
        imageView.backgroundColor = [UIColor grayColor];
        [self.view addSubview:imageView];
    }
```

#### 图片资源加载规格

- test.png 一倍屏加载
- test@2x.png 二倍屏加载
- test@3x.png 三倍屏加载

优先查找相应的规格，否则查找最接近规格的资源

#### contentMode 内容布局方式

- UIViewContentModeScaleToFill 完全填充（拉伸）
- UIViewContentModeScaleAspectFit 等比例填充(不裁剪)
- UIViewContentModeScaleAspectFill 等比例填满(会裁剪）

#### 动态图

- **animationImages** 动画的图片组
- **animationDurations** 动画时长
- **anmationsRepeatCount** 重复次数
- **startAnimating** 开始动画
- **stopAnimating** 停止动画
- **isAnimating**  是否正在动画

```objective-c
		UIImage *image1 = [UIImage imageNamed:@"animation1.jpg"];
    UIImage *image2 = [UIImage imageNamed:@"animation2.jpg"];
    UIImage *image3 = [UIImage imageNamed:@"animation3.jpg"];
    UIImageView *imageView = [[UIImageView alloc] initWithImage:image1];
    imageView.animationImages = @[image1, image2, image3];
    imageView.animationDuration = 2;
    imageView.animationRepeatCount = 100;
    imageView.frame = CGRectMake(20, 100, 200, 200);
    [self.view addSubview:imageView];
    
    [imageView startAnimating];
```

### UIButton

```objective-c
		UISwitch *switchVie2 = [[UISwitch alloc] initWithFrame:CGRectZero];
    switchVie2.center = CGPointMake(200, 100);
    switchVie2.on = YES;
    switchVie2.tintColor = [UIColor blueColor];
    switchVie2.onTintColor = [UIColor grayColor];
    switchVie2.thumbTintColor = [UIColor purpleColor];
    [switchVie2 addTarget:self action:@selector(onSwitchChanged:) forControlEvents:UIControlEventValueChanged];
    [self.view addSubview:switchVie2];

- (void)viewDidLoad {
    [super viewDidLoad];

    //初始化按钮，设置按钮类型
    self.btn = [UIButton buttonWithType:UIButtonTypeSystem];
    /*
     Type:
     UIButtonTypeCustom = 0, // 自定义类型
     UIButtonTypeSystem NS_ENUM_AVAILABLE_IOS(7_0),  // 系统类型
     UIButtonTypeDetailDisclosure,//详细描述样式，圆圈中间加个i
     UIButtonTypeInfoLight, //浅色的详细描述样式
     UIButtonTypeInfoDark,//深色的详细描述样式
     UIButtonTypeContactAdd,//加号样式
     UIButtonTypeRoundedRect = UIButtonTypeSystem,   // 圆角矩形
     */
    
    //设置按钮位置和尺寸
    self.btn.frame = CGRectMake(50, 50, 300, 50);

    //设置按钮文字标题
    [self.btn setTitle:@"我是一个按钮" forState:UIControlStateNormal];
    /*
     State:前三个较为常用
     UIControlStateNormal //正常状态下
     UIControlStateHighlighted //高亮状态下，按钮按下还未抬起的时候
     UIControlStateDisabled  //按钮禁用状态下，不能使用
     UIControlStateSelected  //选中状态下
     UIControlStateApplication //当应用程序标志时
     UIControlStateReserved  //为内部框架预留
     */
    
    //设置按钮文字颜色
    [self.btn setTitleColor:[UIColor orangeColor] forState:UIControlStateNormal];
    
    //设置背景图片（需要注意按钮类型最好为自定义，系统类型的会使图片变暗）
//    [self.btn setImage:[UIImage imageNamed:@"tupian"] forState:UIControlStateNormal];
    
    //设置按钮文字大小
    self.btn.titleLabel.font = [UIFont systemFontOfSize:20];
    
    //设按钮背景颜色
    self.btn.backgroundColor = [UIColor cyanColor];
    
    //设置按钮文字阴影颜色
    [self.btn setTitleShadowColor:[UIColor yellowColor] forState:UIControlStateNormal];
    
    //默认情况下，在按钮被禁用时，图像会被画的颜色深一些。要禁用此功能，可以将这个属性设置为NO
    self.btn.adjustsImageWhenHighlighted = NO;
    
    //默认情况下，按钮在被禁用时，图像会被画的颜色淡一些。要禁用此功能，可以将这个属性设置为NO
    self.btn.adjustsImageWhenDisabled = NO;
    
    //下面的这个属性设置为yes的状态下，按钮按下会发光，这可以用于信息按钮或者有些重要的按钮
    self.btn.showsTouchWhenHighlighted = YES;
    
    //按钮响应点击事件，最常用的方法：第一个参数是目标对象，一般都是self； 第二个参数是一个SEL类型的方法；第三个参数是按钮点击事件
    [self.btn addTarget:self action:@selector(Method) forControlEvents:UIControlEventTouchUpInside];
    
    //将控件添加到当前视图上
    [self.view addSubview:self.btn];
  
   //setBackgroundImage:forState: 设置背景图片
  //setImage:forState: 设置icon（默认左icon右文字,可通过imageEdgeInsets和titleEdgeInsets调整位置）
//enabled和userInteractionEnabled  设置按钮是否禁用
}
//按钮响应事件
-(void)onButtonClick:(UIButton*)sender {
  
}

```

### UISwitch

```objective-c
onTintColor            UIColor          开状态下的颜色
tintColor              UIColor          关状态下的颜色
thumbTintColor          UIColor          滑块颜色
onImage                  UIImage          无效
offImage                UIImage          无效
on（ isOn）              BOOL             isOn是用来获取状态的是get方法，on可以用来设置开关
```

```objective-c
   //2. create switch
    self.mainSwitch = [[UISwitch alloc] initWithFrame:CGRectMake(100, 70, 0, 0)];
    [self.view addSubview:self.mainSwitch];

    //缩小或者放大switch的size
    self.mainSwitch.transform = CGAffineTransformMakeScale(0.5, 0.5);
    self.mainSwitch.layer.anchorPoint = CGPointMake(0, 0.3);

//    self.mainSwitch.onImage = [UIImage imageNamed:@"on.png"];   //无效
//    self.mainSwitch.offImage = [UIImage imageNamed:@"off.png"]; //无效

//    self.mainSwitch.backgroundColor = [UIColor yellowColor];    //它是矩形的

    // 设置开关状态(默认是 关)
//    self.mainSwitch.on = YES;
    [self.mainSwitch setOn:YES animated:true];  //animated

    //判断开关的状态
    if (self.mainSwitch.on) {
        NSLog(@"switch is on");
    } else {
        NSLog(@"switch is off");
    }

    //添加事件监听
    [self.mainSwitch addTarget:self action:@selector(switchAction:) forControlEvents:UIControlEventValueChanged];

    //定制开关颜色UI
    //tintColor 关状态下的背景颜色
    self.mainSwitch.tintColor = [UIColor redColor];
    //onTintColor 开状态下的背景颜色
    self.mainSwitch.onTintColor = [UIColor yellowColor];
    //thumbTintColor 滑块的背景颜色
    self.mainSwitch.thumbTintColor = [UIColor blueColor];
		UISwitch *switchView = [[UISwitch alloc] initWithFrame:CGRectZero];
    switchView.center = CGPointMake(100, 100);
    switchView.tintColor = [UIColor blueColor];
    switchView.onTintColor = [UIColor grayColor];
    switchView.thumbTintColor = [UIColor purpleColor];
    [switchView addTarget:self action:@selector(onSwitchChanged:) forControlEvents:UIControlEventValueChanged];
    [self.view addSubview:switchView];

UISwitch *switchVie2 = [[UISwitch alloc] initWithFrame:CGRectZero];
    switchVie2.center = CGPointMake(200, 100);
    switchVie2.on = YES;
    switchVie2.tintColor = [UIColor blueColor];
    switchVie2.onTintColor = [UIColor grayColor];
    switchVie2.thumbTintColor = [UIColor purpleColor];
    [switchVie2 addTarget:self action:@selector(onSwitchChanged:) forControlEvents:UIControlEventValueChanged];
    [self.view addSubview:switchVie2];
```

### UITextField

#### 常用属性介绍

- placeHolder 占位文本
- returnkeyType 回车类型
- keyboardType  键盘类型
- clearButtonMode 清空按钮模式

```objective-c
		UITextField *textField = [[UITextField alloc] initWithFrame:CGRectMake(20, 100, 200, 30)];
    textField.placeholder = @"输入点什么吧";
    textField.borderStyle = UITextBorderStyleRoundedRect;
    textField.autocorrectionType = UITextAutocorrectionTypeYes;
    textField.returnKeyType = UIReturnKeyDone;
    textField.keyboardType = UIKeyboardTypePhonePad;
    textField.clearButtonMode = UITextFieldViewModeWhileEditing;
    textField.delegate = self;
    [self.view addSubview:textField];
```

```objective-c
@optional

- (BOOL)textFieldShouldBeginEditing:(UITextField *)textField;        // return NO to disallow editing.是否允许开始编辑
- (void)textFieldDidBeginEditing:(UITextField *)textField;           // became first responder是否完成编辑
- (BOOL)textFieldShouldEndEditing:(UITextField *)textField;          // return YES to allow editing to stop and to resign first responder status. NO to disallow the editing session to end是否允许结束编辑
- (void)textFieldDidEndEditing:(UITextField *)textField;             // may be called if forced even if shouldEndEditing returns NO (e.g. view removed from window) or endEditing:YES called完成结束编辑
- (void)textFieldDidEndEditing:(UITextField *)textField reason:(UITextFieldDidEndEditingReason)reason NS_AVAILABLE_IOS(10_0); // if implemented, called in place of textFieldDidEndEditing:

- (BOOL)textField:(UITextField *)textField shouldChangeCharactersInRange:(NSRange)range replacementString:(NSString *)string;   // return NO to not change text是否改变字符串

- (BOOL)textFieldShouldClear:(UITextField *)textField;               // called when clear button pressed. return NO to ignore (no notifications)是否允许清空
- (BOOL)textFieldShouldReturn:(UITextField *)textField;              // called when 'return' key pressed. return NO to ignore.是否允许回车

@end
```

### UITextView

#### 常用属性介绍

- scrollEnabled 是否允许滚动
- textAlignment 字符对齐方式
- text 普通文本
- attributedText 富文本

```objective-c
		UITextView *textView = [[UITextView alloc] initWithFrame:CGRectMake(10, 100, 400, 400)];
    textView.text = @"欢迎来到UITextView的世界！欢迎来到UITextView的世界！欢迎来到UITextView的世界！";
    textView.font = [UIFont systemFontOfSize:20];
    textView.textColor = [UIColor blackColor];
    textView.backgroundColor = [UIColor grayColor];
    textView.scrollEnabled = YES;
    textView.textAlignment = NSTextAlignmentLeft;
    textView.delegate = self;
    [self.view addSubview:textView];
```

```objective-c
@protocol UITextViewDelegate <NSObject, UIScrollViewDelegate>

@optional

- (BOOL)textViewShouldBeginEditing:(UITextView *)textView;//是否允许开始编辑
- (BOOL)textViewShouldEndEditing:(UITextView *)textView;//是否允许结束编辑

- (void)textViewDidBeginEditing:(UITextView *)textView;//完成开始编辑
- (void)textViewDidEndEditing:(UITextView *)textView;//结束开始编辑

- (BOOL)textView:(UITextView *)textView shouldChangeTextInRange:(NSRange)range replacementText:(NSString *)text;//是否允许替换文本
- (void)textViewDidChange:(UITextView *)textView;//文本发生改变

- (void)textViewDidChangeSelection:(UITextView *)textView;//某个范围文本发生改变
```

## UITableView

UITableView是一个列表控件，广泛运用于App的各个界面。其本质是垂直方向滚动的ScrollView。[参考博客](https://www.cnblogs.com/zhenzhen123/p/5071743.html)

UITableView有两种风格：

- UITableViewStylePlain
- UITableViewStyleGrouped

### UITableView结构

- TableHeader（绿色区域）

- Section 分组

- - Header 头部
  - Row 行内容
  - Footer 尾部

- Section 分组

- - Header
  - Row 行内容
  - Footer

- TableFooter (蓝色区域)

### tableView如何展示数据：

- UITableView需要一个数据源(dataSource)来显示数据
- UITableView会向数据源查询一共有多少行数据以及每一行显示什么数据等
- 没有设置数据源的UITableView只是个空壳
- 凡是遵守UITableViewDataSource协议的OC对象，都可以是UITableView的数据源

## Cell的重用原理：

iOS设备的内存有限，如果用UITableView显示成千上万条数据，就需要成千上万个UITableViewCell对象的话，那将会耗尽iOS设备的内存。要解决该问题，需要重用UITableViewCell对象。

**重用原理：**当滚动列表时，部分UITableViewCell会移出窗口，UITableView会将窗口外的UITableViewCell放入一个缓存池中，等待重用。当UITableView要求dataSource返回UITableViewCell时，dataSource会先查看这个缓存池，如果池中有未使用的UITableViewCell，dataSource会用新的数据配置这个UITableViewCell，然后返回给UITableView，重新显示到窗口中，从而避免创建新对象。

### 通过代码自定义cell(cell的高度不一致)：

这里提供的思路并未使用自动布局，是纯手码计算frame。

- 1.新建一个继承自UITableViewCell的类。
- 2.重写initWithStyle:reuseIdentifier:方法
  - 添加所有需要显示的子控件(不需要设置子控件的数据和frame, 子控件要添加到contentView中)
  - 进行子控件一次性的属性设置(有些属性只需要设置一次, 比如字体、固定的图片)
- 3.提供2个模型
  - 数据模型: 存放文字数据、图片数据
  - frame模型: 存放数据模型、所有子控件的frame、cell的高度
- 4.cell拥有一个frame模型(不要直接拥有数据模型)
- 5.重写frame模型属性的setter方法: 在这个方法中设置子控件的显示数据和frame
- 6.frame模型数据的初始化已经采取懒加载的方式(每一个cell对应的frame模型数据只加载一次)

### UITableView的常见属性：

```objective-c
/* tableView的样式 */
@property (nonatomic, readonly) UITableViewStyle style;
// tableView有两种样式
//     UITableViewStylePlain  普通的表格样式
//    UITableViewStyleGrouped  分组模式

/* tableView的数据源 */
@property (nonatomic, weak, nullable) id <UITableViewDataSource> dataSource;
/* tableView的代理 */
@property (nonatomic, weak, nullable) id <UITableViewDelegate> delegate;

#pragma mark - 高度相关
/* tableView每一行的高度，默认为44 */
@property (nonatomic) CGFloat rowHeight;
/* tableView统一的每一组头部的高度 */
@property (nonatomic) CGFloat sectionHeaderHeight;
/* tableView统一的每一组头部的高度 */
@property (nonatomic) CGFloat sectionFooterHeight;
/* 估算的tableView的统一的每一行行高 */
@property (nonatomic) CGFloat estimatedRowHeight;
/* 估算的统一的每一组头部高度，设置估算高度可以优化性能 */
@property (nonatomic) CGFloat estimatedSectionHeaderHeight;
/* 估算的统一的每一组的尾部高度 */
@property (nonatomic) CGFloat estimatedSectionFooterHeight;

/* 总共有多少组 */
@property (nonatomic, readonly) NSInteger numberOfSections;
/* 选中行的indexPath */
@property (nonatomic, readonly, nullable) NSIndexPath *indexPathForSelectedRow;

#pragma mark - 索引条
/* 索引条文字的颜色 */
@property (nonatomic, strong, nullable) UIColor *sectionIndexColor;
/* 索引条的背景色 */
@property (nonatomic, strong, nullable) UIColor *sectionIndexBackgroundColor;

#pragma mark - 分隔线
/* 分隔线的样式 */
@property (nonatomic) UITableViewCellSeparatorStyle separatorStyle;
// 分隔线的样式有：
// UITableViewCellSeparatorStyleNone,  没有分隔线
// UITableViewCellSeparatorStyleSingleLine, 正常分隔线
@property (nonatomic, strong, nullable) UIColor *separatorColor; // 分隔线的颜色

#pragma mark - 头尾部控件
/* tableView的头部控件，只能设置高度，宽度默认填充表格 */
@property (nonatomic, strong, nullable) UIView *tableHeaderView;
/* tableView的尾部控件，只能设置高度，宽度默认填充表格*/
@property (nonatomic, strong, nullable) UIView *tableFooterView;

#pragma mark - 编辑模式
/* 是否是编辑模式，默认是NO */
@property (nonatomic, getter=isEditing) BOOL editing;
/* tableView处在编辑模式的时候是否允许选中行，默认是NO */
@property (nonatomic) BOOL allowsSelectionDuringEditing;
/* tableView处在编辑模式的时候是否允许选中多行数据，默认是NO */
@property (nonatomic) BOOL allowsMultipleSelectionDuringEditing;
```

### UITableViewCell的常见属性：

```objective-c
#pragma mark - Cell内部子控件
/*  默认为nil，imageView是懒加载，用到时才加载 */
@property (nonatomic, readonly, strong, nullable) UIImageView *imageView;
/* 默认为nil，是懒加载，用到时才加载 */
@property (nonatomic, readonly, strong, nullable) UILabel *textLabel;
/* 只有当Cell的样式为UITableViewCellStyleSubtitle时才会显示出来，默认为nil，是懒加载，用到时才加载 */
@property (nonatomic, readonly, strong, nullable) UILabel *detailTextLabel;

/* 当你想要自定义一个cell的时候，子控件最好添加到cell的contentView上，方便编辑表格 */
@property (nonatomic, readonly, strong) UIView *contentView;

/* cell的复用标识 */
@property (nonatomic, readonly, copy, nullable) NSString *reuseIdentifier;

/* cell的选中样式 */
@property (nonatomic) UITableViewCellSelectionStyle   selectionStyle;
/* cell是否是选中状态 */
@property (nonatomic, getter=isSelected) BOOL         selected;

/* cell的编辑样式，默认是UITableViewCellEditingStyleNone */
@property (nonatomic, readonly) UITableViewCellEditingStyle editingStyle;
// cell的编辑样式有：
// UITableViewCellEditingStyleNone,
// UITableViewCellEditingStyleDelete, 删除模式
// UITableViewCellEditingStyleInsert   插入模式

/* 指示器的样式，默认是UITableViewCellAccessoryNone */
@property (nonatomic) UITableViewCellAccessoryType    accessoryType;
// 指示器的样式有：
// UITableViewCellAccessoryNone   不显示指示器
// UITableViewCellAccessoryDisclosureIndicator cell右侧显示一个箭头
// UITableViewCellAccessoryDetailDisclosureButton cell右侧显示一个箭头和一个详情图标
// UITableViewCellAccessoryCheckmark cell右侧显示一个对勾
// UITableViewCellAccessoryDetailButton cell右侧显示一个详情图标

/* cell右侧指示器view，设置了以后，cell的accessoryType就会失效  */
@property (nonatomic, strong, nullable) UIView       *accessoryView;
```

### UITableView的常用方法：

```objective-c
/* 全局刷新 */
- (void)reloadData;
/* 刷新索引条 */
- (void)reloadSectionIndexTitles;

/* 返回第section组有多少行 */
- (NSInteger)numberOfRowsInSection:(NSInteger)section;
/* 返回indexPath对应的那一行的cell */
- (nullable __kindof UITableViewCell *)cellForRowAtIndexPath:(NSIndexPath *)indexPath;

/* 返回第section组的头部view */
- (nullable UITableViewHeaderFooterView *)headerViewForSection:(NSInteger)section;
/* 返回第section组的尾部view */
- (nullable UITableViewHeaderFooterView *)footerViewForSection:(NSInteger)section;

/**
 *  插入数据,刷新数据
 *  @param indexPaths 插入在哪个位置
 *  @param animation  动画效果，枚举UITableViewRowAnimation
 */
- (void)insertRowsAtIndexPaths:(NSArray<NSIndexPath *> *)indexPaths withRowAnimation:(UITableViewRowAnimation)animation;

/**
 *  删除数据
 *  @param indexPaths 删除数据的位置
 *  @param animation  动画效果，枚举UITableViewRowAnimation
 */
- (void)deleteRowsAtIndexPaths:(NSArray<NSIndexPath *> *)indexPaths withRowAnimation:(UITableViewRowAnimation)animation;
// 枚举UITableViewRowAnimation包括：
// UITableViewRowAnimationFade,  淡入淡出
// UITableViewRowAnimationRight,  向右滑动
// UITableViewRowAnimationLeft,  向左滑动
// UITableViewRowAnimationTop,  向上滑动
// UITableViewRowAnimationBottom,  向下滑动
// UITableViewRowAnimationNone,  无动画效果,iOS 3.0以后可用
// UITableViewRowAnimationMiddle,  保持cell的中间，iOS 3.2以后可用
// UITableViewRowAnimationAutomatic  自动选择一个合适的动画效果

/**
 *  刷新某一行的数据
 *  @param indexPaths 刷新数据的位置
 *  @param animation  动画效果，枚举UITableViewRowAnimation
 */
- (void)reloadRowsAtIndexPaths:(NSArray<NSIndexPath *> *)indexPaths withRowAnimation:(UITableViewRowAnimation)animation;

/** 动画开启/关闭编辑模式 */
- (void)setEditing:(BOOL)editing animated:(BOOL)animated;

/** 返回一个带有复用标识的cell */
- (nullable __kindof UITableViewCell *)dequeueReusableCellWithIdentifier:(NSString *)identifier;

/**
 *  使用xib时注册自定义的cell
 *  @param nib        要注册的nib
 *  @param identifier 复用标识
 */
- (void)registerNib:(nullable UINib *)nib forCellReuseIdentifier:(NSString *)identifier;

/**
 *   代码自定义cell时注册自定义的cell
 *  @param cellClass  注册的自定义cell的类型
 *  @param identifier 复用标识
 */
- (void)registerClass:(nullable Class)cellClass forCellReuseIdentifier:(NSString *)identifier;

/** 初始化一个带有复用标识的cell */
- (instancetype)initWithStyle:(UITableViewCellStyle)style reuseIdentifier:(nullable NSString *)reuseIdentifier
```

### UITableViewDataSource中的常用方法:

```objective-c
// 必须实现的方法：
/* 设置第section组有多少行数据 */
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section;
/* 设置每一行cell的内容，每当有一个cell进入视野屏幕的时候就会调用这个方法，所以在这个方法内部进行优化（cell的复用） */
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath;

// 可实现可不实现的方法：
/* 总共有多少组，如果不实现，默认是一组 */
- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView;

/* 第section组的头部标题 */
- (nullable NSString *)tableView:(UITableView *)tableView titleForHeaderInSection:(NSInteger)section;
/* 第section组的尾部标题 */
- (nullable NSString *)tableView:(UITableView *)tableView titleForFooterInSection:(NSInteger)section;

/** 设置索引条 */
- (nullable NSArray<NSString *> *)sectionIndexTitlesForTableView:(UITableView *)tableView;
```

### UITableViewDelegate中的常用方法：

```objective-c
/** 设置每一行cell的高度 */
- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath;
/** 设置第section组的头部高度 */
- (CGFloat)tableView:(UITableView *)tableView heightForHeaderInSection:(NSInteger)section;
/** 设置第section组的尾部高度 */
- (CGFloat)tableView:(UITableView *)tableView heightForFooterInSection:(NSInteger)section;

/** 每一行的估算高度 */
- (CGFloat)tableView:(UITableView *)tableView estimatedHeightForRowAtIndexPath:(NSIndexPath *)indexPath;

/** 设置第section组的headerView */
- (nullable UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section;
/** 设置第section组的footerView */
- (nullable UIView *)tableView:(UITableView *)tableView viewForFooterInSection:(NSInteger)section;

/** 选中了某一行cell的时候就会调用这个方法 */
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath;

/**
 *  设置左滑删除按钮的文字
 *  @param tableView         被编辑的tableView
 *  @param indexPath         indexPath
 *  @return 返回删除按钮显示的文字
 */
- (nullable NSString *)tableView:(UITableView *)tableView titleForDeleteConfirmationButtonForRowAtIndexPath:(NSIndexPath *)indexPath;

/**
 *  创建一个左滑出现按钮的操作（UITableViewRowAction）数组
 *  @param tableView         添加操作的tableView
 *  @param indexPath         indexPath，可以指定每一行有不同的效果
 *  @return 返回一个操作（UITableViewRowAction）数组
 */
- (nullable NSArray<UITableViewRowAction *> *)tableView:(UITableView *)tableView editActionsForRowAtIndexPath:(NSIndexPath *)indexPath;

/**
 *  移动cell
 *  @param tableView            操作的tableView
 *  @param sourceIndexPath      移动的行
 *  @param destinationIndexPath 目标行
 */
- (void)tableView:(UITableView *)tableView moveRowAtIndexPath:(NSIndexPath *)sourceIndexPath toIndexPath:(NSIndexPath *)destinationIndexPath;

/** 根据editingStyle处理是删除还是添加操作，完成删除、插入操作刷新表格 */
- (void)tableView:(UITableView *)tableView commitEditingStyle:(UITableViewCellEditingStyle)editingStyle forRowAtIndexPath:(NSIndexPath *)indexPath;

/**
 *  设置表格编辑模式，不实现默认都是删除
 *  @param tableView 操作的tableView
 *  @param indexPath cell的位置
 *  @return 返回表格编辑样式
 */
- (UITableViewCellEditingStyle)tableView:(UITableView *)tableView editingStyleForRowAtIndexPath:(NSIndexPath *)indexPath;
```

## UIViewController

### UIViewController简介

ViewController是iOS应用程序中重要的部分，是应用程序数据和视图之间的重要桥梁，ViewController管理应用中的众多视图。

#### ViewController分类

- 展示型ViewController
  -  UIViewController 基类，自定义视图
  -  UITableViewController  垂直列表视图
  -  UICollectionViewController  垂直和水平列表视图
  -  UIActivityViewController  系统分享视图
  -  UIAlertController     AlertView和ActionSheet
  -  UISearchController  搜索
  -  UIInputViewController  自定义键盘容器
- 容器型ViewController
  - UINavigationController 导航控制器
  - UITabbarController     选项卡控制器
  - UIPageViewController  多页面控制器（多用于App启动闪屏）
  - UISplitViewController    分割控制器（用于iPad）

### 生命周期各个步骤

- init  初始化
- loadView()  自定义加载视图
- viewDidLoad() 视图加载完成
- viewWillLayoutSubviews() 将要subview布局
- viewDidLayoutSubviews() 完成subview布局
- viewWillAppear(_:) 视图将要显示（过场动画即将开始）
- viewDidAppear(_:) 视图显示结束（过场动画结束）
- viewWillDisappear(_:) 视图将要消失
- viewDidDisappear(_:) 视图消失结束
- didReceiveMemoryWarning() 内存警告（正在显示的界面不会收到）
- dealloc 销毁

## UINavigationController

UINavigationController即导航控制器，用来管理视图控制器。它以栈的形式管理视图控制器，管理视图控制器个数理论上不受限制(实际受内存限制)，push和pop方法来弹入弹出控制器，最多只能显示一个视图控制器，那就是处于栈顶的视图控制器。