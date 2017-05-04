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
```oc
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