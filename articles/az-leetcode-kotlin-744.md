---
title: "LeetCode 744. Find Smallest Letter Greater Than Target" # 記事のタイトル
emoji: "😟" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/find-smallest-letter-greater-than-target/

~~~txt
Given a characters array letters that is sorted in non-decreasing order and a character target, return the smallest character in the array that is larger than target.

Note that the letters wrap around.

For example, if target == 'z' and letters == ['a', 'b'], the answer is 'a'.
~~~

昇順ソートされた文字配列lettersと文字列targetが与えられたときに、文字列targetよりアルファベット順に後に出てくるもので、文字配列lettersの中ではアルファベット順に最初に出てくるものを求める問題

# Code

~~~kotlin
class Solution {
    fun nextGreatestLetter(letters: CharArray, target: Char): Char {

        var left = 0
        var right = letters.lastIndex

        while (left <= right) {
            val middle = left +(right -left)/2
            if (letters[middle] > target) {
                right = middle -1
            } else {
                left = middle +1
            }
        }
        return letters[left%letters.size]
    }
}
~~~

~~~kotlin
class Solution {
    fun nextGreatestLetter(letters: CharArray, target: Char): Char {

        for (char in 'a'..'z') {
            if (char == target) {
                for (idx in letters.indices) {

                    if (char < letters[idx]) return letters[idx]
                    if (idx == letters.lastIndex) return letters[0]
                }
            }
        }
        return Char.MAX_VALUE
    }
}
~~~

ソートされている配列からtargetを探索するときは基本的には2分探索で指数時間で探しに行く。

後者の方が理論上計算量が大きくなる想定だが、実際にはこちらのほうが早かった。
制約が`2 <= letters.length <= 10^4`だったから？


# Profile

- 1つ目
	- Runtime: 216 ms ($O(logn))
	- Memory Usage: 38.3 MB
 
- 2つ目
	- Runtime: 204 ms ($O(n^2))
	- Memory Usage: 38.2 MB

# Submission
- https://leetcode.com/submissions/detail/606048949/
- https://leetcode.com/submissions/detail/606037599/
