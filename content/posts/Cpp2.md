---
title: C++ Note 2
date: 2021-11-28
tags: [Cpp]
---

本文是我的C++笔记的第二篇
My Second C++ Note;

<!--more-->
## vector.clear()
`vector.clear()`并不释放数组，其会调用`data[0]...data[size-1]`的析构函数。
`vector`在析构的时候会先调用`clear()`然后释放数组
例如
```cpp
    vector<int> x{1, 2, 4};
    cout << x.capacity() << x.size() << endl; // 3 3
    x.push_back(10);
    cout << x.capacity() << x.size() << endl; // 6 4
    x.clear();
    cout << x.capacity() << x.size() << endl; // 6 0
```

```cpp

class World
{
public:
    World()
    {
        cout << "World()!\n";
    }
    ~World()
    {
        cout << "~World()!\n";
    }
};

int main()
{
    vector<World> v(3);
    cout << "AAA\n";
    v.clear();
    cout << "BBB\n";
}
```
这个会输出
```bash
World()!
World()!
World()!
AAA
~World()!
~World()!
~World()!
BBB

```

## clear and minimize
### Before C++11
```cpp
vector<int> v{1,2,3,4,5};
vector<int> temp;
v.swap(temp);
// to make the capacity and size of v to be both 0
```

### After C++11
```cpp
vector<int> v{1,2,3,4,5};
v.clear();
v.shrink_to_fit();// to make the capacity == size
```

## partition()
按照规则进行进行分组
返回分出的第二组的首位的迭代器

例如
```cpp
vector<int> v{1, 2, 3, 4, 5, 6, 7};
    partition(v.begin(), v.end(), [](int x)
              { return x % 2 == 0; });
    pr(v);
```

得到
6 2 4 3 5 1 7
前面一组都是符合的
后面一组都是不符合的
可能会打乱顺序

对于不想打乱顺序的，使用`stable_partition()`

## partial_sort()
```cpp
void partial_sort (RandomAccessIterator first,
                   RandomAccessIterator middle,
                   RandomAccessIterator last);
```
partial_sort() 会将 [first, last) 范围内最小（或最大）的 middle-first 个元素移动到 [first, middle) 区域中，并对这部分元素做升序（或降序）排序。

partial_sort() 函数：
容器支持的迭代器类型必须为随机访问迭代器。这意味着，partial_sort() 函数只适用于 array、vector、deque 这 3 个容器。
当选用默认的升序排序规则时，容器中存储的元素类型必须支持 <小于运算符；同样，如果选用标准库提供的其它排序规则，元素类型也必须支持该规则底层实现所用的比较运算符；
partial_sort() 函数在实现过程中，需要交换某些元素的存储位置。因此，如果容器中存储的是自定义的类对象，则该类的内部必须提供移动构造函数和移动赋值运算符。

## partial_sort_copy()
partial_sort_copy() 函数的功能和 partial_sort() 类似，唯一的区别在于，前者不会对原有数据做任何变动，而是先将选定的部分元素拷贝到另外指定的数组或容器中，然后再对这部分元素进行排序。
```cpp
RandomAccessIterator partial_sort_copy (
                       InputIterator first,
                       InputIterator last,
                       RandomAccessIterator result_first,
                       RandomAccessIterator result_last);
```
partial_sort_copy() 函数会将 [first, last) 范围内最小（或最大）的 result_last-result_first 个元素复制到 [result_first, result_last) 区域中，并对该区域的元素做升序（或降序）排序。


值得一提的是，[first, last] 中的这 2 个迭代器类型仅限定为输入迭代器，这意味着相比 partial_sort() 函数，partial_sort_copy() 函数放宽了对存储原有数据的容器类型的限制。换句话说，partial_sort_copy() 函数还支持对 list 容器或者 forward_list 容器中存储的元素进行“部分排序”，而 partial_sort() 函数不行。

但是，介于 result_first 和 result_last 仍为随机访问迭代器，因此 [result_first, result_last) 指定的区域仍仅限于普通数组和部分类型的容器，这和 partial_sort() 函数对容器的要求是一样的。

## nth_element()
nth_element is a partial sorting algorithm that rearranges elements in [first, last) such that:

* The element pointed at by nth is changed to whatever element would occur in that position if [first, last) were sorted.
* All of the elements before this new nth element are less than or equal to the elements after the new nth element.


简单的理解 nth_element() 函数的功能，当采用默认的升序排序规则（std::less<T>）时，该函数可以从某个序列中找到第 n 小的元素 K，并将 K 移动到序列中第 n 的位置处。不仅如此，整个序列经过 nth_element() 函数处理后，所有位于 K 之前的元素都比 K 小，所有位于 K 之后的元素都比 K 大。

## is_sorted()
```cpp
//判断 [first, last) 区域内的数据是否符合 std::less<T> 排序规则，即是否为升序序列
bool is_sorted (ForwardIterator first, ForwardIterator last);
```
## is_sorted_until()
和 is_sorted() 函数相比，is_sorted_until() 函数不仅能检测出某个序列是否有序，还会返回一个正向迭代器，该迭代器指向的是当前序列中第一个破坏有序状态的元素。

```cpp
ForwardIterator is_sorted_until (ForwardIterator first, ForwardIterator last);
```
## merge()
merge() 函数用于将 2 个有序序列合并为 1 个有序序列，前提是这 2 个有序序列的排序规则相同（要么都是升序，要么都是降序）。并且最终借助该函数获得的新有序序列，其排序规则也和这 2 个有序序列相同。
```cpp
OutputIterator merge (InputIterator1 first1, InputIterator1 last1,
                      InputIterator2 first2, InputIterator2 last2,
                      OutputIterator result);
```
## inplace_merge()
事实上，当 2 个有序序列存储在同一个数组或容器中时，如果想将它们合并为 1 个有序序列，除了使用 merge() 函数，更推荐使用 inplace_merge() 函数。
```cpp
//默认采用升序的排序规则
void inplace_merge (BidirectionalIterator first, BidirectionalIterator middle,
                    BidirectionalIterator last);
```
其中，first、middle 和 last 都为双向迭代器，[first, middle) 和 [middle, last) 各表示一个有序序列。

## find() find_if()
```cpp
InputIterator find (InputIterator first, InputIterator last, const T& val);
```
find() 函数本质上是一个模板函数，用于在指定范围内查找和目标元素值相等的第一个元素。
另外，该函数会返回一个输入迭代器，当 find() 函数查找成功时，其指向的是在 [first, last) 区域内查找到的第一个目标元素；如果查找失败，则该迭代器的指向和 last 相同。
值得一提的是，find() 函数的底层实现，其实就是用`==`运算符将 val 和 [first, last) 区域内的元素逐个进行比对。这也就意味着，[first, last) 区域内的元素必须支持`==`运算符。

---
和 find() 函数相同，find_if() 函数也用于在指定区域内执行查找操作。不同的是，前者需要明确指定要查找的元素的值，而后者则允许自定义查找规则。
```cpp
InputIterator find_if (InputIterator first, InputIterator last, UnaryPredicate pred);
```

## find_end()与search()
序列A: 1,2,3,7,8,9,1,2,3,4
序列B: 1,2,3 

find_end(A,B)返回B在A里面最后一次出现的位置
search(A,B)返回B在A里面第一次出现的位置

## all_of(), any_of(), none_of()
algorithm 头文件中定义了 3 种算法，用来检查在算法应用到序列中的元素上时，什么时候使谓词返回 true。这些算法的前两个参数是定义谓词应用范围的输入迭代器；第三个参数指定了谓词。检查元素是否能让谓词返回 true 似乎很简单，但它却是十分有用的。

## count(),count_if()
返回序列中
count(): 等于val的个数
count_if(): 满足pred的个数

## C++中的map,filter,reduce
分别等价于transform,copy_if,accumulate

用法：
#### copy_if
```cpp
vector<int> v{1, 2, 3, 4, 5, 6, 7};
vector<int> vv;
copy_if(begin(v), end(v), inserter(vv,begin(vv)), [](auto &x)
            { return x % 2 == 0; });
pr(vv);
```
注意：这里使用`inserter`来创建了插入迭代器，从而实现向v2里面插入元素的功能

类似的
```cpp
    vector<int> v{1, 2, 3};
    vector<int> cc{11, 22, 33, 44};
    copy_if(v.begin(), v.end(), back_inserter(cc), [](auto &x)
            { return x % 2 == 0; });
    pr(cc);// 11 22 33 44 2
```
这里使用`back_inserter`向后面追加元素
### transform 
对容器里面的每个元素都做一次操作
### accumulate
迭代所有元素并做类似求和等操作
```cpp
    vector<int> v{1, 2, 3};
    cout << accumulate(v.begin(), v.end(), v[0], [](auto &x, auto &y)
                       { return x * y; });//will print 6
```

注意:当我们把accumulate作用在monoid上面的时候，要把第三个参数，即初始值设置为幺元.这有时候比较烦人。
那么，当我们已经确定了所处理的容器不会是空的的时候，可以这么写
```cpp
vector<int> v{1, 2, 3, 4};
    cout << accumulate(next(begin(v)), end(v), v[0], [](auto &x, auto &y)
                       { return x + y; });
```
也就是把初始值设置为`v[0]`之后再累加剩余的元素即可。

## unique
unique() 算法可以在序列中原地移除重复的元素，这就要求被处理的序列必须是正向迭代器所指定的。在移除重复元素后，它会返回一个正向迭代器作为新序列的结束迭代器。可以提供一个函数对象作为可选的第三个参数，这个参数会定义一个用来代替 == 比较元素的方法。

## for_each()
```cpp
    vector<int> v{1, 2, 3};
    for_each(v.begin(), v.end(), [](auto &x)
             { x = x * 2; });
    pr(v);//2 4 6

    vector<int> vv{1, 2, 3};
    for_each(vv.begin(), vv.end(), [](auto x)
             { x = x * 2; });
    pr(v);//1 2 3
```

