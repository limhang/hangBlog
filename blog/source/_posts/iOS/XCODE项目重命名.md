---
title: XCODE项目重命名
date: 2017-07-18 17:17:59
tags: iOS
categories: iOS
---
# 缘由：重命名xcode项目，这里涉及到很多的依赖和关联，所以其实很容易出错的，在此记录一下

<!--more-->

## step 1--修改项目名
打开xcode项目，修改右侧中的name，详细见下图：
![xcode_rename01](http://ok2nitkry.bkt.clouddn.com/xcode_rename01.png)
填写完成之后，点击其他位置，这个时候，系统会给一个弹窗，点击弹窗中的rename按钮

## step 2--Rename the Scheme
点击左侧栏的【edit schemes】位于【stop】按钮旁边，点击下方的manager schemes，修改schemes,**点击一下，选中，然后回车修改名字**，关闭xcode

如果使用pod，这里需要多加一步，回到文件夹中，修改OLD.xcworkspace to NEW.xcworkspace

## step 3--修改文件夹，所有旧文件名
这里主要修改，oldNameUItests，oldnaemTests，oldname这3个文件夹的名字

打开oldname.xcodeproj【注意是这个文件，不是pod生成的那个文件】

然后依次对那3个文件进行修改名字的操作，和修改路径的操作，具体位置见下图
![xcode_rename](http://ok2nitkry.bkt.clouddn.com/xcode_rename02.png)

## step 4--修改plist文件
点击Build Settings，修改plist文件路径和bundleid名字

## step 5--修改pod相关
删除所有与pod相关文件，重新pod install

打开【SoilDetector.xcworkspace】,删除红色失去关联的SoilDetector.xcworkspace文件

在Build phases中，点击Link Binary With Libraries,删除old name的pod文件

command + shift + k ，然后run，ok！
