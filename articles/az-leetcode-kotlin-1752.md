---
title: "LeetCode 1752. Check if Array Is Sorted and Rotated" # 記事のタイトル
emoji: "😟" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/check-if-array-is-sorted-and-rotated/

~~~txt
Given an array nums, return true if the array was originally sorted in non-decreasing order, then rotated some number of positions (including zero). Otherwise, return false.

There may be duplicates in the original array.

Note: An array A rotated by x positions results in an array B of the same length such that A[i] == B[(i+x) % A.length], where % is the modulo operation.
~~~

与えられた整数配列numsについて、昇順ソートで、任意の位置からローテーションしていればtrueを返し、そうでなければfalseを返す問題

# Code

~~~kotlin
class Solution {
    fun check(nums: IntArray): Boolean {

        var cnt = 0
        for (idx in 1..nums.lastIndex) {
            if (nums[idx] < nums[idx-1]) cnt++
        }
        if (nums[0] < nums[nums.lastIndex]) cnt++

        return cnt < 2
    }
}
~~~

1周舐めてローテーションしているかどうかを調べるので、任意の地点に対する直前の地点の値が基本的に小さいことを調べながら以下を不正なパターンとしてチェックすればよい
- 2回以上任意の地点に対する直前の地点の値が大きくなってはいけない(0回の時、綺麗な昇順ソートであるはず。1回のときローテーションであるはず。)
- 0番目の値より末尾要素の値が大きくなることがあっても1回まで 

# Profile

- Runtime: 160 ms
- Memory Usage: 35.9 MB

# Submission
- https://leetcode.com/submissions/detail/610359294/

