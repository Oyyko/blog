---
title: C++ Note 4
date: 2022-07-07
tags: [Cpp]
enableLaTeX: true
---
C++ 笔记4

<!--more-->
## 引用与重载
```cpp
#include <iostream>
using namespace std;
void f(double x)
{
    cout << "DOUBLE" << endl;
}

void f(int &x)
{
    cout << "INT&" << endl;
}

int main()
{
    f(2);
}
```
这段代码会输出DOUBLE 原因在于 2是右值 不能用于初始化一个`int&` 则只能选择第一个版本的函数重载

## 重载的其他规则
`T`和`const T`同样
`T*`和`const T*`不一样 但是和`T* const `一样
`T&`和`const T&`不一样

## 模版实现数组求平均值