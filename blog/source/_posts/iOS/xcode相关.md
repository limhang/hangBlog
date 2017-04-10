---
title: xcode相关
date: 2017-04-10 16:36:35
tags: xcode
categories: iOS
---

# 缘由：在此总结归纳xcode的使用技巧和xcode编译相关内容

<!--more-->

# 一、xcode基本使用
## 1-1、xcode中创建文件夹
如果在xcode工程中new group，只是在视觉效果上分好了几个文件夹，方便分类管理，但在finder中并不会创建新的文件夹，在硬盘目录还是所有文件都并列在一个文件夹内，更恶心的是当你重新打开工程后，会发现刚才new的group已经不见了。那应该怎样建立文件夹呢？ 

**正确的方法是：**在finder找到把工程，新建一个文件夹aa，然后在xcode里面－－右键－－add files to "xxx"－－找到把文件夹aa－－完成，以后若要创建文件，在aa文件夹－－new file即可把文件添加进来，以后在包那里新建文件自然在这个包内。

## 1-2、xcode快捷键
**1、打开了2个工程项目，怎么在2个工程项目中切换**

Q：'command' + '~'

**2、在.m和.h文件之间快速切换**

Q：'command' + 'ctrl' + '上/下'

**3、折叠代码操作**

Q：‘command’ + 'option' + '左/右'

## 1-3、iOS系统升级后，xcode适配
iOS新系统发布后，xcode需要在添加支持文件，步骤如下：

```
（1）Xcode右击－选项－在Finder显示

（2）右击Xcode－显示包内容

（3）Contents-->Developer-->Platforms-->iPhoneOS.platform-->DeviceSupport

（4）网络下载的文件放入上述位置，完成
```

## 1-4、xcode真机调试错误
**一、账号已绑定最大ID数**

**解决办法：**说明开发账号已绑定超过5台手机，目前解决办法就是重新注册一个账号。

**使用注意：**所有自己跑的app都可以使用同一个bundleID，前提是使用相同的开发者账号，运行的BuddleID不要随便的更改

## 1-5、xcode8上架
**xcode8上架与7有较多不一样的地方**，目前整理不是很详细，[参考](http://www.jianshu.com/p/0d5c6ac48732)。

# 二、xcode编译器
## 2-1、背景
### 2-1-1、GCC编译器
GCC（GNU Compiler Collection，GNU编译器套装），是一套由 GNU 开发的编程语言编译器。它是一套以 GPL 及 LGPL 许可证所发行的自由软件，也是 GNU计划的关键部分，亦是自由的类Unix及苹果电脑 Mac OS X 操作系统的标准编译器。

GCC 原名为 GNU C 语言编译器，因为它原本只能处理 C语言。GCC 很快地扩展，变得可处理 C++。之后也变得可处理 Fortran、Pascal、Objective-C、Java, 以及 Ada与其他语言。

### 2-1-2、LLVM编译器
Apple（包括中后期的NeXT）一直使用GCC作为官方的编译器。GCC作为开源世界的编译器标准一直做得不错，但Apple对编译工具会提出更高的要求。

一方面，是Apple对Objective-C语言（甚至后来对C语言）新增很多特性，但GCC开发者并不买Apple的帐——不给实现，因此索性后来两者分成两条分支分别开发，这也造成Apple的编译器版本远落后于GCC的官方版本。另一方面，GCC的代码耦合度太高，不好独立，而且越是后期的版本，代码质量越差，但Apple想做的很多功能（比如更好的IDE支持）需要模块化的方式来调用GCC，但GCC一直不给做。甚至最近，《GCC运行环境豁免条款 （英文版）》从根本上限制了LLVM-GCC的开发。

### 2-1-3、clang编译器
Apple吸收Chris Lattner的目的要比改进GCC代码优化宏大得多——GCC系统庞大而笨重，而Apple大量使用的Objective-C在GCC中优先级很低。此外GCC作为一个纯粹的编译系统，与IDE配合得很差。加之许可证方面的要求，Apple无法使用LLVM 继续改进GCC的代码质量。于是，Apple决定从零开始写 C、C++、Objective-C语言的前端 Clang，完全替代掉GCC。

## 2-2、应用
### 2-2-1、@synthesize,@property和@dynamic区别和联系
@synthesize告诉编译器，自动生成@property的setter和getter方法，@dynamic告诉编译器，@property的setter和getter不用自动生成，会在子类实现，或者运行时的时候生成

### 2-2-2、三者和编译器的纠葛
早先的xcode编译器使用的是GCC，所以用户需要手动的写@synthesize someThing = _something;后期使用clang编译器后，这个步骤在声明属性的时候，编译器自动就生成了，最早期的时候，属性的setter和getter都需要用户自己手动生成

### 2-2-3、常用的属性声明方法
早期的属性和成员变量声明是，头文件中interface{NSString * _name;},@property NSString *name;，实现文件中implement,@synthesize name = _name;这样成员变量就和属性绑定在一起了，可以直接使用下划线的成员变量，如果将@synthesize name = _name;改为@synthesize name;这样相当于@synthesize name = name;成员变量就是name了，这样是很不好的写法，在clang编译器中，不用在写@synthesize的语法了，只有当需要更改成员变量名字的时候，才需要写@synthesize的语法

二者还有一个小区别，如果使用老版的写法，在interface{}大括号中写，成员变量，这样子类可以直接使用_xxx访问到父类的成员变量，如果使用现在的写法，直接声明属性的话，需要使用self.xxx才能访问到父类定义的成员变量
