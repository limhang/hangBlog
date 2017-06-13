---
title: Objc的block中weakSelf和strongSelf用法
date: 2017-06-13 14:11:04
tags: iOS
categories: iOS
---

# 缘由：weakself和strongself用法解析
<!--more-->
# 一、weakSelf和strongSelf
Objective C 的 Block 是一个很实用的语法，特别是与GCD结合使用，可以很方便地实现并发、异步任务。但是，如果使用不当，Block 也会引起一些循环引用问题(retain cycle)—— Block 会 retain ‘self’，而 ‘self‘ 又 retain 了 Block。因为在 ObjC 中，直接调用一个实例变量，会被编译器处理成 ‘self->theVar’，’self’ 是一个 strong 类型的变量，引用计数会加 1，于是，self retains queue， queue retains block，block retains self。

Apple 官方的建议是，传进 Block 之前，把 ‘self’ 转换成 weak automatic 的变量，这样在 Block 中就不会出现对 self 的强引用。如果在 Block 执行完成之前，self 被释放了，weakSelf 也会变为 nil。

eg:
```objectivec
__weak __typeof__(self) weakSelf = self;
    dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
    [weakSelf doSomething];
});
```

clang 的文档表示，在 doSomething 内，weakSelf 不会被释放。但，下面的情况除外：
```objectivec
__weak __typeof__(self) weakSelf = self;
    dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
    [weakSelf doSomething];
    [weakSelf doOtherThing];
});
```

在 doSomething 中，weakSelf 不会变成 nil，不过在 doSomething 执行完成，调用第二个方法 doOtherThing 的时候，weakSelf 有可能被释放，于是，strongSelf 就派上用场了：
```objectivec
__weak __typeof__(self) weakSelf = self;
    dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
    __strong __typeof(self) strongSelf = weakSelf;
    [strongSelf doSomething];
    [strongSelf doOtherThing];
});
```
__strong 确保在 Block 内，strongSelf 不会被释放。

故：
* 在 Block 内如果需要访问 self 的方法、变量，建议使用 weakSelf。
* 如果在 Block 内需要多次 访问 self，则需要使用 strongSelf。

# 二、weakify and strongify
这就是我们在大多数 Objective-C 代码中看到的 weak/strong dance , 这样的写法运行起来没有问题，但是还是有不足之处。当有了新的需求，添加新的特性，block的定义会变得越来越长，越来越复杂，仍然会有可能在 block 中使用 self . 我们对此无法发觉 - 编译器的提示帮助只在一些简单的例子下。这就是 weakify 和 strongify 宏发挥作用的时候了。


@weakify 和 @strongify 的 Original implementation 比较复杂，因为他们接受多个参数。为了使分析简单一点，我们引用自己的版本，只接受一个参数:

```objectivec
#define weakify(var) __weak typeof(var) AHKWeak_##var = var;
#define strongify(var) \
_Pragma("clang diagnostic push") \
_Pragma("clang diagnostic ignored \"-Wshadow\"") \
__strong typeof(var) var = AHKWeak_##var; \
_Pragma("clang diagnostic pop")
```

看下我们对这个宏的使用:
```objectivec
Model *model = [Model new];
weakify(self);
model.dataChanged = ^(NSString *title) {
    strongify(self);
    self.label.text = tile;
};
self.model = model;
```

以上代码可以翻译为:
```objectivec
Model *model = [Model new];
__weak typeof(self) AHKWeak_self = self;
model.dataChanged = ^(NSString *title) {
    __strong typeof(self) self = AHKWeak_self;
    self.label.text = tile;
};
self.model = model;
```

# 参考
[weakSelf和strongSelf参考](http://blog.lessfun.com/blog/2014/11/22/when-should-use-weakself-and-strongself-in-objc-block/)

[weakify and strongify](http://devshen.github.io/2015/06/04/Why_you_should_start_using@weakify_and%20@strongify_macros/)