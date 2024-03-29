---
title: C++ Note 3
date: 2022-02-22
tags: [Cpp]
---

本文是我的C++笔记的第三篇
My Thrid C++ Note;

<!--more-->

## explict
防止类的构造函数的隐式转换
```cpp
class A
{
private:
    int x;
public:
    A(int x_): x{x_} {}
};

// ...

A a = 3; // YES
A a2{3}; // YES
```
这样是对的

```cpp
class A
{
private:
    int x;
public:
    explict A(int x_): x{x_} {}
};

// ...

A a = 3; // NO
A a{3}; //YES
```

## 析构函数 移动构造 和 emplace_back
下列代码
```cpp
class A
{
private:
    int x;

public:
    A(int xx) : x{xx} {}
    A() : A(0) {}
    ~A() { cout << "goodbye " << x << endl; }
};

int main()
{
    vector<A> v;
    v.emplace_back(A{0});
    v.emplace_back(A{1});
    v.emplace_back(A{2});
    cout << "--------------------------------------\n";

    // OUTPUT
    // goodbye 0
    // goodbye 0
    // goodbye 1
    // goodbye 0
    // goodbye 1
    // goodbye 2
    // --------------------------------------
    // goodbye 0
    // goodbye 1
    // goodbye 2
}
```
其原因在于：
首先:`emplace_back`只有在传入构造参数列表的时候和`push_back`有区别。其他的时候没有区别。
这里相当于先生成临时对象`A{0}`，再拷贝构造到v中。从而执行了一次对于0的析构
之后，因为要再加入`A{1}`,而目前的v的可用空间大小是1
因此需要再向系统申请一块大小为2的空间。之后再把原来的`A{0}`复制过去。再放弃原来的大小为1的地址上的`A{0}`,因此又执行一次对于0的析构。之后构造临时对象A1，再复制到v中。则临时对象析构，造成一次对于1的析构。
之后要加入2. 方法一样。先开辟新内存空间，把原来的对象复制过去。再加入2 即可
最终，3个对象的生命周期均到达终点。则依次析构。

而
```cpp
    vector<A> v;
    v.reserve(10);
    v.emplace_back(A{0});
    v.emplace_back(A{1});
    v.emplace_back(A{2});
```
在提前预留了空间之后，输出为
```
goodbye 0
goodbye 1
goodbye 2
--------------------------------------
goodbye 0
goodbye 1
goodbye 2
```


我们输出capacity即可看到我们的猜测是正确的。
```cpp
    vector<A> v;
    cout << "CAPACITY " << v.capacity() << endl;
    v.emplace_back(A{0});
    cout << "CAPACITY " << v.capacity() << endl;
    v.emplace_back(A{1});
    cout << "CAPACITY " << v.capacity() << endl;
    v.emplace_back(A{2});
    cout << "CAPACITY " << v.capacity() << endl;
    cout << "--------------------------------------\n";
```
得到
```
CAPACITY 0
goodbye 0
CAPACITY 1
goodbye 0
goodbye 1
CAPACITY 2
goodbye 0
goodbye 1
goodbye 2
CAPACITY 4
--------------------------------------
goodbye 0
goodbye 1
goodbye 2

```

结论：
emplace_back以参数列表的形式传入时，不论是否有移动构造函数，都是原地构造，只会调用一次构造函数（只有这一项和push_back有区别，其它都是一样的）

emplace_back以左值对象的形式传入时，不论是否有移动构造函数，都是调用一次拷贝构造函数

emplace_back以右值对象（例如move（左值对象），或者就是右值）的形式传入时
a. 有移动构造函数，调用一次移动构造
b. 没有移动构造函数，调用拷贝构造函数

emplace_back以 Person(“aaa”, “shandong”, 1991) 形式传入时
a. 有移动构造函数，构造临时文件 —> 移动构造 —> 临时文件析构
b. 没有移动构造函数，构造临时文件 —> 拷贝构造 —> 临时文件析构
