---
title: "LeetCode 628. Maximum Product of Three Numbers" # 記事のタイトル
emoji: "😝" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/maximum-product-of-three-numbers/

~~~txt
Given an integer array nums, find three numbers whose product is maximum and return the maximum product.
~~~

整数配列numsが与えられるので、3つ選んだときの積の最大値を求める問題

# Code

~~~kotlin
import kotlin.math.max

class Solution {
    fun maximumProduct(nums: IntArray): Int {
        var min1 = Int.MAX_VALUE
        var min2 = Int.MAX_VALUE
        var max1 = Int.MIN_VALUE
        var max2 = Int.MIN_VALUE
        var max3 = Int.MIN_VALUE

        for (num in nums) {
            when {
                num <= min1 -> {
                    min2 = min1
                    min1 = num
                }
                num <= min2 -> {
                    min2 = num
                }
            }
            when {
                num >= max3 -> {
                    max1 = max2
                    max2 = max3
                    max3 = num
                }
                num >= max2 -> {
                    max1 = max2
                    max2 = num
                }
                num >= max1 -> {
                    max1 = num
                }
            }
        }
        return max(min1 * min2 * max3, max1 * max2 * max3)
    }
}
~~~

~~~kotlin
import kotlin.math.max

class Solution {
    fun maximumProduct(nums: IntArray): Int {
            val sorted = nums.sorted()
            return max(sorted[nums.lastIndex -2] * sorted[nums.lastIndex -1] * sorted[nums.lastIndex], sorted[0] * sorted[1] * sorted[nums.lastIndex])
    }
}
~~~

積が最大となるのは以下の2パターンの可能性があるので、両方とも計算して大きな数を返せば良い
- `1番大きな数` * `2番目に大きな数` * `3番目に大きな数`
- `1番小さな数` * `2番目に小さな数` * `1番大きな数`

それぞれの数を線形探索しながら更新して、全て舐めた後に、格納されている値で計算するのが1つ目であるが、
1番大きな数、2番目に大きな数、3番目に大きな数、1番小さな数、2番目に小さな数を知る方法として、
「ソート -> 該当箇所抽出」がある。
こちらの方が、この問題の性質を生かしていて好みである。

# Profile

- 1つ目
    - Runtime: 252 ms
    - Memory Usage: 39.3 MB

- 2つ目
    - Runtime: 384 ms
    - Memory Usage: 39.7 MB


# Submission

- https://leetcode.com/submissions/detail/599874039/
- https://leetcode.com/submissions/detail/599832797/