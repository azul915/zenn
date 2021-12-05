---
title: "LeetCode 26. Remove Duplicates from Sorted Array" # 記事のタイトル
emoji: "🥳" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/remove-duplicates-from-sorted-array

~~~txt
Given an integer array nums sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same.

Since it is impossible to change the length of the array in some languages, you must instead have the result be placed in the first part of the array nums. More formally, if there are k elements after removing the duplicates, then the first k elements of nums should hold the final result. It does not matter what you leave beyond the first k elements.

Return k after placing the final result in the first k slots of nums.

Do not allocate extra space for another array. You must do this by modifying the input array in-place with O(1) extra memory.
~~~

昇順な整数配列numsが与えられるので、重複を排除する問題。ただし、remove the duplicates **in-place** について気をつけたい。
というのも、Sinceに後続する箇所をよく読むと、言語によって配列の長さが変えることができないので、重複を取り除く作業において先頭から要素を詰めて行く必要があることが書かれているので、Setにいれて長さを取るようなことはしてはいけない。

# Code

~~~kotlin
class Solution {
    fun removeDuplicates(nums: IntArray): Int {
        var now = 0
        for (idx in 1..nums.lastIndex) {
            if (nums[now] < nums[idx]) {
                now++
                nums[now] = nums[idx]
            }
        }
        return now + 1
    }
}
~~~

下図は与えられるarray numsが[0,0,1,1,1,2,2,3,3,4]のときに作業をトレースしたもの

インデックス0から重複がないように数字を並べていく必要があるので、位置を管理する役目をnowに与える

あとはarray numsを探索していき、より大きな数字が出現したら、nowの右隣に格納するためにインクリメントして、そこに見つけた数字を格納することの繰り返しである


![](/images/az-leet-code-kotlin-26/remove-duplicates-from-sorted-array.drawio.png)

# Profile

- Runtime: 248 ms
- Memory Usage: 41.7 MB

# Submission
- https://leetcode.com/submissions/detail/596460705/
