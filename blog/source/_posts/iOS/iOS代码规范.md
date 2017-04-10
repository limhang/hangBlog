---
title: iOS代码规范
date: 2017-04-10 16:56:29
tags: iOS
categories: iOS
---

# 缘由：记录iOS代码书写规范，不定期更新

<!--more-->

# 一、viewController代码结构
* 属性：@property(nonatomic,strong)UIButton *confirmButton
* VC生命周期：#pragma mark - life cycle
* 系统控件代理：#pragma mark - UITableViewDelegate
* 自定义代理：#pragma mark - CustomDelegate
* 事件响应：#pragma mark - event response
* 私有方法(一般都是时间控件外观小处理，也可以放在分类中)：#pragma mark - private methods
* setter和getter方法：#pragma mark - setters and getters

注：控件添加到view上面都是在viewDidLoad中，使用[self.view addSubview:self.firstLabel];调用getter方法

[很好的架构资料](http://casatwy.com/iosying-yong-jia-gou-tan-viewceng-de-zu-zhi-he-diao-yong-fang-an.html)