---
title: Leetcode-[3Sum Closest] 
date: 2017-05-15 22:00:00
tags: [找工作]
categories: [Leetcode]
---

# Problem Definition
Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

# Solution
```[python]
class Solution(object):
    def threeSumClosest(self, nums, target):
        nums.sort()
        lenght = len(nums)
        closestSum = nums[0] + nums[1] + nums[2]
        smallestDiff = target - closestSum 
        for i in xrange(length-2):
            left = ind+1
            right  = length-1
            while left < right:
                sum = nums[i] + nums[left] + nums[right]
                diff = target-sum
                if diff == 0:
                    return target
                elif diff > 0:
                    right = right -1
                else:
                    left = left + 1
                if abs(smallestDiff) < abs(diff):
                    smallestDiff = diff
                    closestSum = sum
        return closestSum
```
