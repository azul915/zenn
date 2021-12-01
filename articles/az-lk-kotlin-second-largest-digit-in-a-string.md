---
title: "LeetCode 1796. Second Largest Digit in a String" # 記事のタイトル
emoji: "🤔" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/second-largest-digit-in-a-string/

~~~txt
Given an alphanumeric string s, return the second largest numerical digit that appears in s, or -1 if it does not exist.

An alphanumeric string is a string consisting of lowercase English letters and digits.
~~~

英数字の文字列sが与えられるのので、2番目に大きな数字を返し、存在しない場合は-1を返しなさいという問題

# Code

~~~kotlin
class Solution {
    fun secondHighest(s: String): Int {

        val ss = s.filter { it in '0'..'9' }.map { it.toString().toInt() }.toSet()
        if (ss.size < 2) return -1
        return ss.sortedDescending()[1]
    }
}
~~~

~~~kotlin
class Solution {
    fun secondHighest(s: String): Int {
        var largest = -1
        var second = -2

        val ssets = s.toSet()
        for (digit in ssets) {
            digit.toString().toIntOrNull()?.let {
                when {
                    it > largest -> {
                        second = largest
                        largest = it
                    }
                    it == largest -> {}
                    it in second until largest -> {
                        second = it
                    }
                    else -> {}
                }
            }
        }
        return if (largest == -1 || second == -2) -1 else second
    }
}
~~~~

情報を漁っても、
あまりアルゴリズム的なアプローチが光るような問題ではなかったので、個人的には1つ目ので十分な気がしている...
Setにはしたほうがいいと思う

# Profile

- 1つ目
    - Runtime: 204 ms
    - Memory Usage: 37.5 MB
- 2つ目
    - Runtime: 172 ms
    - Memory Usage: 35.8 MB

# Submission
- https://leetcode.com/submissions/detail/595470081/
- https://leetcode.com/submissions/detail/595466089/
