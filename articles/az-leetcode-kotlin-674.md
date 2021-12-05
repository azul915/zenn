---
title: "LeetCode 674. Longest Continuous Increasing Subsequence" # 記事のタイトル
emoji: "🙃" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/longest-continuous-increasing-subsequence/

~~~txt
Given an unsorted array of integers nums, return the length of the longest continuous increasing subsequence (i.e. subarray). The subsequence must be strictly increasing.

A continuous increasing subsequence is defined by two indices l and r (l < r) such that it is [nums[l], nums[l + 1], ..., nums[r - 1], nums[r]] and for each l <= i < r, nums[i] < nums[i + 1].
~~~

ソートされていない整数配列numsが与えられるので、一番長く連続して増加している部分配列の長さを求める問題
同値の整数が並んでいる場合は部分配列としてみなされない

# Code

~~~kotlin
class Solution {
    fun findLengthOfLCIS(nums: IntArray): Int {
        var subSeqLen = 1
        var tmp = 1
        for (idx in 1..nums.lastIndex) {
            if (nums[idx -1] < nums[idx]) {
                tmp++
                if (subSeqLen < tmp) subSeqLen = tmp
            } else {
                tmp = 1
            }
        }
        return subSeqLen
    }
}
~~~

解答後に別解などを調べる際に初めて知ったが、CSにおいて顕著な問題として扱われる「LIS問題」とは定義している部分配列の条件が違っており、
今回は、部分配列における隣接するどの2要素も、`subArray[i-1] < subArray[i]`となっていればよい


# Profile

- Runtime: 200 ms
- Memory Usage: 38.6 MB

# Submission

- https://leetcode.com/submissions/detail/597213718/
