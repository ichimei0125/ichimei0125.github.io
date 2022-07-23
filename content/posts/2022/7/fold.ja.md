---
title: "シーケンスデータを単独データになる方法ーーフォールドについて"
date: 2022-07-23T12:30:17+09:00
description: "aaa"
draft: false
tags: ["programming"]
---

## 背景
先週のleetcode [weekly contest 302](https://leetcode.com/contest/weekly-contest-302/)の中、下記の質問があります

[2344. Minimum Deletions to Make Array Divisible](https://leetcode.com/contest/weekly-contest-302/problems/minimum-deletions-to-make-array-divisible/)

> You are given two positive integer arrays nums and numsDivide. You can delete any number of elements from nums.
>
> Return the minimum number of deletions such that the smallest element in nums divides all the elements of numsDivide. If this is not possible, return -1.
>
> Note that an integer x divides y if y % x == 0.

例：

nums = [2,3,2,4,3], numsDivide = [9,6,9,3,15]

解は2

1. numsDivideの全て数の最大公約数を算出し、3である
1. numsの中に3を整除できる数を探す。この場合も3である
1. 3を最小値になるため、3より小さい二つの2を削除し、2回の削除を行う必要があるので、答えは2である

ですが、ステップ1に、このような問題がありました

pythonのgcdは[math.gcd(*integers)](https://docs.python.org/ja/3/library/math.html#math.gcd)リストを与えられません

つなり

```python
math.gcd(9,6,9,3,15) # OK 
math.gcd(numsDivide) # NG 
```

下記のようなコードも解けますが、エレガントとは言えませんし、実行時間もかかります

実際下記コードを提出すると *Time Limit Exceeded* になります

なので、何か簡潔、エレガントな方法ありませんでしょうか

```python
def recursion(arr: List[int], cur: int) -> int:
    if len(arr) == 0:
        return cur
    if len(arr) == 1:
        return math.gcd(arr[0], cur)
    
    tmp = math.gcd(arr[0], cur)
    return recursion(arr[1:], tmp)

numsDivide = [9,6,9,3,15]
recursion(numsDivide[1:], numsDivide[0]) # 結果は3
```

## Fold

ここで、フォールドという概念を紹介します。ウィキペディアはこちら[wiki](https://zh.wikipedia.org/wiki/Fold_(%E9%AB%98%E9%98%B6%E5%87%BD%E6%95%B0))

簡単というと、Foldはarray、listのようなシーケンスデータを一つ一つのデータに分解する関数です。

では、上記の再帰コードを下記のようなコードに書き換えます

```python
import functools
functools.reduce(math.gcd, numsDivide) # 結果は3
```

他言語のフォールドは下記になります

|言語|Fold|
|:-|:-|
|c++|[...](https://cpprefjp.github.io/lang/cpp17/folding_expressions.html)|
|c#|[Aggregate](https://docs.microsoft.com/ja-jp/dotnet/api/system.linq.enumerable.aggregate?view=net-6.0)|
|javascript|[reduce](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)|
|rust|[fold](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.fold)|
