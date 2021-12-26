---
title: "LeetCode 1752. Check if Array Is Sorted and Rotated" # 記事のタイトル
emoji: "😟" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/check-if-array-is-sorted-and-rotated/

~~~txt
Given an array nums, return true if the array was originally sorted in non-decreasing order, then rotated some number of positions (including zero). Otherwise, return false.

There may be duplicates in the original array.

Note: An array A rotated by x positions results in an array B of the same length such that A[i] == B[(i+x) % A.length], where % is the modulo operation.
~~~

与えられた整数配列numsに、昇順ソート

# Code

~~~kotlin
class Solution {
    fun check(nums: IntArray): Boolean {

        var cnt = 0
        for (idx in 1..nums.lastIndex) {
            if (nums[idx] < nums[idx-1]) {
                cnt++
                if (1 < cnt) return false
            }
            if (idx == nums.lastIndex && 0 < cnt && nums[0] < nums[idx]) return false
        }
        return true
    }
}
~~~

# Profile

- Runtime: 160 ms
- Memory Usage: 35.7 MB

# Submission
- https://leetcode.com/submissions/detail/607492498/
