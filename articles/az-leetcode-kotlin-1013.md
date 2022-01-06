---
title: "LeetCode 1013. Partition Array Into Three Parts With Equal Sum" # 記事のタイトル
emoji: "😆" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/partition-array-into-three-parts-with-equal-sum/

~~~txt
Given an array of integers arr, return true if we can partition the array into three non-empty parts with equal sums.

Formally, we can partition the array if we can find indexes i + 1 < j with (arr[0] + arr[1] + ... + arr[i] == arr[i + 1] + arr[i + 2] + ... + arr[j - 1] == arr[j] + arr[j + 1] + ... + arr[arr.length - 1])
~~~

整数配列arrが与えられるので、それぞれの合計を等しく、空でない3つのパートに分けることができるときにtrueを、そうでないときにfalseを返す問題。

# Code

~~~kotlin
class Solution {
    fun canThreePartsEqualSum(arr: IntArray): Boolean {
        val sum = arr.sum()
        if (sum %3 != 0) return false
        val ot = sum /3
        var partitions = 0
        var acc = 0
        for (idx in arr.indices) {
            acc += arr[idx]
            if (acc == ot) {
                partitions++
                acc = 0
            }
        }
        return 2 < partitions
    }
}
~~~

3つのパートに分けたとき和が等しくなる(これを3分の1和と呼ぶものとする)ので暫定の和を集計しながら、ちょうど3分の1和となった回数をカウント。3回以上あればよい。

# Profile
- Runtime: 416 ms
- Memory Usage: 69.7 MB

# Submission
- https://leetcode.com/submissions/detail/610860514/

