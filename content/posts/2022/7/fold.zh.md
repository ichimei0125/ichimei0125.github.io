---
title: "如何将一串数据拆分成单个数据ーーFold高阶函数简介"
date: 2022-07-23T12:30:17+09:00
draft: false
tags: ["programming"]
---

## 起因
上周的leetcode [weekly contest 302](https://leetcode.com/contest/weekly-contest-302/)里，有这么道题

[2344. Minimum Deletions to Make Array Divisible](https://leetcode.com/contest/weekly-contest-302/problems/minimum-deletions-to-make-array-divisible/)

> You are given two positive integer arrays nums and numsDivide. You can delete any number of elements from nums.
>
> Return the minimum number of deletions such that the smallest element in nums divides all the elements of numsDivide. If this is not possible, return -1.
>
> Note that an integer x divides y if y % x == 0.

例如：

nums = [2,3,2,4,3], numsDivide = [9,6,9,3,15]

答案是2

1. 算出numsDivide里最大公约数，也就是3，
1. 然后找nums里能整除3的也恰好是3
1. 那么为了让3时nums里的最小值，只要删除两个2就行了，所以答案是2

于是在第一步里，就遇到了这么个问题

python的gcd是[math.gcd(*integers)](https://docs.python.org/ja/3/library/math.html#math.gcd), 不能传list

也就是说，

```python
math.gcd(9,6,9,3,15) # OK 
math.gcd(numsDivide) # NG 
```

虽然类似以下的代码也能得到正确的答案, 不过并不优雅，执行起来也费时间

实际上以下代码执行是会出现 *Time Limit Exceeded* 超时的问题

所以有没有什么简洁优雅的方法？

```python
def recursion(arr: List[int], cur: int) -> int:
    if len(arr) == 0:
        return cur
    if len(arr) == 1:
        return math.gcd(arr[0], cur)
    
    tmp = math.gcd(arr[0], cur)
    return recursion(arr[1:], tmp)

numsDivide = [9,6,9,3,15]
recursion(numsDivide[1:], numsDivide[0]) # 结果是3
```

## Fold

于是这里就介绍一个概念Fold, 关于Fold的[wiki](https://zh.wikipedia.org/wiki/Fold_(%E9%AB%98%E9%98%B6%E5%87%BD%E6%95%B0))

简单来说Fold可以将array, list这样的iterable的数据，拆分成单个的数据

那么之前的代码就可以写成如下的代码, 并且也没有*Time Limit Exceeded*问题

```python
import functools
functools.reduce(math.gcd, numsDivide) # 结果是3
```

顺便常用语言的高阶函数如下

|语言|Fold|
|:-|:-|
|c++|[...](https://en.cppreference.com/w/cpp/language/fold)|
|c#|[Aggregate](https://docs.microsoft.com/zh-cn/dotnet/api/system.linq.enumerable.aggregate?view=net-6.0)|
|javascript|[reduce](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)|
|rust|[fold](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.fold)|