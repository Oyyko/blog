---
title: C++ Note 1
date: 2021-01-05
tags: [Cpp]
enableLaTeX: true
---

本文是我的C++笔记的第一篇
My First C++ Note;

<!--more-->


## 一些例子
### 1
```cpp
#include <iostream>

using namespace std;

int main()
{
    string x{"222333"};
    char *y = "222333";
    string xx{x, 3};
    string yy{y, 3};
    cout << xx << endl;
    cout << yy << endl;
}
```
### 2
```cpp
void pp(int x)
{
    cout<<"int";
}

void pp(int* x)
{
    cout<<"int*";
}


int main()
{
    pp(nullptr);// 输出int*
    pp(NULL); // 有歧义，编译失败
}
``` 

## 一些疑问
```cpp
int s[2][2]{
    {1, 2}, {3, 4}};
for (int *i : s)
{
    for (int j{}; j < 2; ++j)
    {
        cout << i[j];
    }
}
```
这段代码为何生效？s的类型不应该是int(*)[2]吗?
解释：s的类型为int[2][2],但是非常容易decay,因此大多数时候会提示为int(*)[2]，例如你用char x{s};试图从报错中得到s的类型的时候就会提示为int(*)[2].此处从s中取出的应该是int[2],然后被decay为int*
追问：那有没有办法让i的类型为int[2]
解答：写成auto& i 或者 int(&x)[2]即可
完整代码为
```cpp
int s[2][2]{
    {1, 2}, {3, 4}};
for (int(&i)[2] : s)
{
    for (int j : i)
    {
        cout << j;
    }
}
for (auto &i : s)
{
    for (auto j : i)
    {
        cout << j;
    }
}
for (int j : (*s))
{
    cout << j;
}
```