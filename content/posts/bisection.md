---
title: 二分总结
date: 2021-11-30T15:20:51+08:00
tags: [Algorithm, Leetcode]
---

##　Leetcode 704 二分查找target

这种是在一个元素各不相同的有序数组里面找一个等于target的数
解法：

```cpp
class Solution
{
public:
    int search(vector<int> &nums, int target)
    {
        int l{0};
        int r = nums.size() - 1;
        int mid;
        while (l <= r)
        {
            mid = (l + (r - l) / 2);
            if (nums[mid] == target)
            {
                return mid;
            }
            if (target < nums[mid])
            {
                r = mid - 1;
            }
            else
            {
                l = mid + 1;
            }
        }
        return -1;
    }
};
```

此时，分三种情况。注意到，在不等于的时候，r和l都对mid有排斥

### 排斥性

在二分问题中，mid能不能等于l或者r称为排斥性

### 二分模版

常见的二分情景是:
对于01数组，找到里面的第一个1出现的位置(**重点：这里我们认为1一定会出现，后面的部分会讨论1有可能不出现的情况**)
00011111
这符合自然界的规律，某个实验，在参数小于某个阈值的时候，都是失败的。大于等于这个阈值的时候，都是成功的。

这种时候需要我们认真考虑排斥性
对于先0后1,一定会出现1的实验，代码为:

```cpp
vector<int> v{0, 0, 0, 1, 1, 1};
    auto yes = [&](int mid)
    { return v[mid] == 1; };
    size_t l{0}, r{v.size() - 1};
    int mid;
    while (l < r)
    {
        mid = l + (r - l) / 2;
        if (yes(mid))
            r = mid;
        else
            l = mid + 1;
    }
    cout << l;
```

原因是：
我们之所以设置 `while(l<r)`因为我们实际上是弄了一个闭区间 `[l,r]`
规定真正的答案只能出现在这个闭区间里面
也就是说区间定义为:答案可能出现的位置
这样，当我们把l和r缩减到重合的时候，答案就被我们抓住了。

缩减的方法：注意如果mid取了1，那么它可能是答案，也可能在答案的右边，因此设置 `r=mid`.但如果mid取0，那么答案一定在mid右边,那么设置 `l=mid+1`

### 有可能找不到答案的情况

注意到：按照我们上面闭区间的思考模式，一开始就默认了答案在 `[l,r]`里面
如果采用 `[,)`左闭右开的方式，也许有别的处理方法。
但这里我的处理是
加入

```cpp
if(yes(l))return l;
else return -1;
```

这虽然丑陋，但是有效。

例如

```cpp
int main()
{
    vector<int> v{0, 0, 0};
    auto yes = [&](int mid)
    { return v[mid] == 1; };
    size_t l{0}, r{v.size() - 1};
    int mid;
    while (l < r)
    {
        mid = l + (r - l) / 2;
        if (yes(mid))
            r = mid;
        else
            l = mid + 1;
    }
    if (yes(l))
        cout << l;
    else
        cout << -1;
}
```

确实有效

### LOWER_BOUND 和 UPPER_BOUND

对于v={0,1,2,3}
lower_bound(v,2)得到2
upper_bound(v,2)得到3

对于 1,1,2,2,2,3,3,3
l(v,2)=2
u(v,2)=5

l(v,1)=0
r(v,1)=2

l(v,0)=0
r(v,0)=0

l(v,3)=5
r(v,3)=6

l(v,4)=6
r(v,4)=6

对于1,1,3,3,5,5,7,7

lower_bound(4)=4
upper (4) =4

---

lower_bound:
Returns an iterator pointing to the first element in the range [first, last) that is **not less** than (i.e. greater or equal to) value, **or last if no such element is found**.

upper_bound:
Returns an iterator pointing to the first element in the range [first, last) that is **greater** than value, **or last if no such element is found**.

区别在于lower_bound是大于等于的第一个
upper_bound是大于的第一个

### equal_range

返回 `pair<Itr,Itr>` 第一个指向区域里面第一个等于val的元素，第二个指向区域里面第一个大于val的元素
也就是左闭右开的 `[,)`区间
