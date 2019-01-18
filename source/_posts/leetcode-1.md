---
title: Leetcode-[Container with most water]
date: 2017-05-14 22:00:00
tags: [找工作]
categories: [Leetcode, Array]
---

# Problem Definition
Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container and n is at least 2.

# Solution
- container的定义在这里很迷惑，数组里每一个数字代表的一个bar，这个bar就是container的边界，而container的体积则是由两个container的边界的中的最小值和两个边界之间的距离决定的, 这意味着中间的bar是忽略不记的。。
- 还有一个题的container则是相反的，中间的bar不仅要作为边界，还要作为容器的底的。。。。
- 弄清楚概念之后，直观的想法是DP，我最一开始的想法是把容器氛围两类
    - 一类是左边比右边高，一类是右边比左边高
    - 这样两轮遍历数组，每次找比之前更高的
    - 但是这样的问题是另一边的边界不好找
- 转换思路后，觉得two pointer可能可行，这样两边的边界都可以确定
- 虽然直觉上觉得这样能得到最优解，但是怎么证明呢
    - 首先我们可以得到的一个解是min(a[0], a[n-1]) * (n-1)
    - 那么如果a[0] < a[n-1]的话，那么剩下的最优解一定在a[1:]中

```[python] 
class Solution:
    def findContainerWithMostWater(self, bars):
        n = len(bars)
        maxWater = (n-1) * min(bars[0], bars[-1])
        if bars[0] < bars[-1]:
            return max(maxWater, self.findContainerWithMostWater(bars[1:]))
        else:
            return max(maxWater, self.findContainerWithMostWater(bars[:-1]))    
```
