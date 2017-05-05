---
title: iOS基础汇总
date: 2017-04-24 15:39:03
tags: iOS
categories: iOS
---

缘由：好久都没有写原生iOS应用了，很多的东西都忘的差不多了，之前也有做过零零散散的笔记，这里对这次开发做个记录，汇总下iOS开发的基础知识，这样就不怕以后又忘记了。

<!--more-->

零、list about what i need
0-0、

一、常用技巧知识
1-1、xcode使用相关
1-1-1、建立新的文件夹

* 1.$cd 到你要创建文件夹的目录。
* 2.终端输入$ mkdir View。
* 3.将创建好的文件夹拖入工程点Copy就OK了。
**注意：**蓝色文件夹（folder）一般作为资源文件夹使用，与黄色文件夹（groups）的主要区别是不参与编译，所以在上述步骤中，如果是代码文件就选择groups的。


二、网络请求
1-1、YTKNetwork
1-1-1、基本使用

1-1-2、上传图片数组
需求：上传一个图片数组，与服务端的kw是：img，数组元素是NSData
代码：
```objectivec
- (AFConstructingBlock)constructingBodyBlock {
    return ^(id<AFMultipartFormData> formData) {
        for (int i = 0;i < _img.count; i++){
            NSData *data = UIImageJPEGRepresentation(_img[i], 0.5);
            if ((float)data.length/1024 > 1000) {
                data = UIImageJPEGRepresentation(_img[i], 1024*1000.0/(float)data.length);
            }
            NSString *name = [NSString stringWithFormat:@"image%d.png",i];
            NSString *type = @"image/jpeg";
            [formData appendPartWithFileData:data name:@"img" fileName:name mimeType:type];
        }
    };
}
```

三、时间处理
1-1、零时区，东八区时间转换
需求：服务器传给客户端的是零时区，我们处在东8区，所以客服端需要显示的时间要在零时区上加8小时。服务器返回的格式为：xxxx-xx-xx xx-xx-xx，那么：
代码：
```objectivec
//将服务器返回的字符串转为nsdate
+ (NSDate *)stringToDate: (NSString *)string {

    NSAssert(string, @"Parameter 'string' should not be nil");
    static NSDateFormatter *_dateFormatter = nil;
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        _dateFormatter = [[NSDateFormatter alloc] init];
    });
    [_dateFormatter setDateFormat: @"yyyy-MM-dd HH:mm:ss +0000"];
    NSDate *destDate= [_dateFormatter dateFromString: string];
    return destDate;
}

//将任意类型的nsdate转换为东8区的nsdate
+(NSDate *)getNowDateFromatAnDate:(NSDate *)anyDate
{
    //设置源日期时区
    NSTimeZone* sourceTimeZone = [NSTimeZone timeZoneWithAbbreviation:@"UTC"];//或GMT
    //设置转换后的目标日期时区
    NSTimeZone* destinationTimeZone = [NSTimeZone localTimeZone];
    //得到源日期与世界标准时间的偏移量
    NSInteger sourceGMTOffset = [sourceTimeZone secondsFromGMTForDate:anyDate];
    //目标日期与本地时区的偏移量
    NSInteger destinationGMTOffset = [destinationTimeZone secondsFromGMTForDate:anyDate];
    //得到时间偏移量的差值
    NSTimeInterval interval = destinationGMTOffset - sourceGMTOffset;
    //转为现在时间
    NSDate* destinationDateNow = [[NSDate alloc] initWithTimeInterval:interval sinceDate:anyDate];
    return destinationDateNow;
}

//将nsdate转换为字符串xxxx-xx-xx xx-xx-xx
+ (NSString *)dateToString: (NSDate *)date {
    
    NSAssert(date, @"Parameter 'date' should not be nil");
    
    NSCalendar *calendar = [NSCalendar currentCalendar];
    NSUInteger unitFlags = NSCalendarUnitYear | NSCalendarUnitMonth | NSCalendarUnitDay | NSCalendarUnitHour | NSCalendarUnitMinute | NSCalendarUnitSecond;
    NSDateComponents *dateComponent = [calendar components:unitFlags fromDate: date];
    NSInteger year = [dateComponent year];
    NSInteger month = [dateComponent month];
    NSInteger day = [dateComponent day];
    NSInteger hour = [dateComponent hour];
    NSInteger minute = [dateComponent minute];

    NSString *dateStr = [NSString stringWithFormat:@"%d-%d-%d %d:%d",(int)year,(int)month,(int)day,(int)hour,(int)minute];
    
    return dateStr;
}
```

三、UI相关
3-1、label控件
* 计算label的宽度
1、获取多行文字size的方法
```objectivec
- (CGRect)boundingRectWithSize:(CGSize)size options:(NSStringDrawingOptions)options attributes:(NSDictionary *)attributes context:(NSStringDrawingContext *)context NS_AVAILABLE_IOS(7_0);

//使用方法
CGSize titleSize = [testString boundingRectWithSize:CGSizeMake(200, MAXFLOAT) options:NSStringDrawingUsesLineFragmentOrigin attributes:@{NSFontAttributeName:[UIFont systemFontOfSize:14]} context:nil].size;

```

2、获取单行文字size的方法
```objectivec
NSString *testString = @"我要测试一下文字内容的长度哦,不要一定非常准确,但是也可能非常正确,我就是用来测试文字行数才弄这么多字数在这里,呵呵,赶紧运行看看结果吧,不过你要记录一下单行计算下的size数值,后面会用来做比对的.";
// 计算一下14号字体的情况下,size的结果
CGSize size =[testString sizeWithAttributes:@{NSFontAttributeName:[UIFont systemFontOfSize:14]}];
```

