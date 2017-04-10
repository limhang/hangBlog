---
title: 使用clang分析oc源码
date: 2017-04-10 16:48:11
tags: iOS
categories: iOS
---

# 缘由：如果要深入理解oc，分析其运行时源码是非常有必要的，在此给出分析方式，和必要的基础知识

<!--more-->

# 一、分析方法
使用clang编译器，查看oc代码
指令：clang -rewrite-objc xxx.m

# 二、基础知识
## 2-1、理解结构体和结构体指针
结构体：
```
struct 结构体名{
    结构体所包含的变量或数组
};
```
既然结构体是一种数据类型，那么就可以用它来定义变量。例如：
struct stu stu1, stu2;

你也可以在定义结构体的同时定义结构体变量：
```c
struct stu{
    char *name;  //姓名
    int num;  //学号
    int age;  //年龄
    char group;  //所在学习小组
    float score;  //成绩
} stu1, stu2;
```
如果只需要 stu1、stu2 两个变量，后面不需要再使用结构体名定义其他变量，那么在定义时也可以不给出结构体名，如下所示：
```c
struct{  //没有写 stu
    char *name;  //姓名
    int num;  //学号
    int age;  //年龄
    char group;  //所在学习小组
    float score;  //成绩
} stu1, stu2;
```

结构体指针：
```c
struct stu{
    char *name;  //姓名
    int num;  //学号
    int age;  //年龄
    char group;  //所在小组
    float score;  //成绩
} stu1 = { "Tom", 12, 18, 'A', 136.5 };
//结构体指针
struct stu *pstu = &stu1;
```
也可以在定义结构体的同时定义结构体指针：
```c
struct stu{
    char *name;  //姓名
    int num;  //学号
    int age;  //年龄
    char group;  //所在小组
    float score;  //成绩
} stu1 = { "Tom", 12, 18, 'A', 136.5 }, *pstu = &stu1;
```
注意，结构体变量名和数组名不同，数组名在表达式中会被转换为数组指针，而结构体变量名不会，无论在任何表达式中它表示的都是整个集合本身，要想取得结构体变量的地址，必须在前面加&，所以给 pstu 赋值只能写作：
struct stu *pstu = &stu1;
[参考](http://c.biancheng.net/cpp/html/88.html)

## 2-2、C++构造函数
```c++
class Counter
{
 
public:
    // 类Counter的构造函数
    // 特点：以类名作为函数名，无返回类型
    Counter()
    {
        m_value = 0;
    }
          
private:     
    // 数据成员
  int m_value;
}
```
[参考](http://ticktick.blog.51cto.com/823160/194307)

## 2-3、函数指针
```c
#include <stdio.h>
void say_hello(const char *str);  
void (*fptr)(const char *);  
int main(void)  
{  
    void (*fptr)(const char *) = say_hello;  
    fptr("KingPlesk");  
    return 0;  
}  
  
void say_hello(const char *str)  
{  
    printf("Hello %s\n", str);  
}
```
[参考](http://blog.csdn.net/feigegegegegegegeg/article/details/52469731)