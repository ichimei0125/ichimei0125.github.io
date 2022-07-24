---
title: "How to turn iterable to individual data -- about Fold"
date: 2022-07-23T12:30:17+09:00
draft: false
tags: ["programming"]
---

## TL;DR
In last week leetcode [weekly contest 302](https://leetcode.com/contest/weekly-contest-302/), there is a question

[2344. Minimum Deletions to Make Array Divisible](https://leetcode.com/contest/weekly-contest-302/problems/minimum-deletions-to-make-array-divisible/)

> You are given two positive integer arrays nums and numsDivide. You can delete any number of elements from nums.
>
> Return the minimum number of deletions such that the smallest element in nums divides all the elements of numsDivide. If this is not possible, return -1.
>
> Note that an integer x divides y if y % x == 0.

example：

nums = [2,3,2,4,3], numsDivide = [9,6,9,3,15]

answer is 2

1. find the greatest common divisor in numsDivide, which is 3.
1. Then find the nums that can divide by 3, which also happens to be 3.
1. Then, in order to make 3 the minimum value in nums, just delete twice(two 2), so the answer is 2

but at step 1, here is the problem

Parameter type of gcd of python is [math.gcd(*integers)](https://docs.python.org/ja/3/library/math.html#math.gcd), which means cannot pass list to <code>math.gcd()</code>


```python
math.gcd(9,6,9,3,15) # OK 
math.gcd(numsDivide) # NG 
```

Although a code like the one below will give the correct answer, it is not elegant and takes time to execute

In fact, the following code will execute with a *Memory Limit Exceeded* problem

So, it there an elegant way?

```python
def recursion(arr: List[int], cur: int) -> int:
    if len(arr) == 0:
        return cur
    
    tmp = math.gcd(arr[0], cur)
    return recursion(arr[1:], tmp)

numsDivide = [9,6,9,3,15]
recursion(numsDivide[1:], numsDivide[0]) # result is 3
```

## Fold

Here is a introduce about Fold [wiki](https://zh.wikipedia.org/wiki/Fold_(%E9%AB%98%E9%98%B6%E5%87%BD%E6%95%B0))

Fold can split iterable data (like arrays and lists) into individual data.

Then the previous code can be written as follows, and there is no *Memory Limit Exceeded* problem

```python
import functools
functools.reduce(math.gcd, numsDivide) # result is 3
```

Fold in other programming language

|language|Fold|
|:-|:-|
|c++|[...](https://en.cppreference.com/w/cpp/language/fold)|
|c#|[Aggregate](https://docs.microsoft.com/en-us/dotnet/api/system.linq.enumerable.aggregate?view=net-6.0)|
|javascript|[reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)|
|rust|[fold](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.fold)|