---
title: "LeetCode 1790. Check if One String Swap Can Make Strings Equal" # 記事のタイトル
emoji: "😢" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/check-if-one-string-swap-can-make-strings-equal/

~~~txt
You are given two strings s1 and s2 of equal length. A string swap is an operation where you choose two indices in a string (not necessarily different) and swap the characters at these indices.

Return true if it is possible to make both strings equal by performing at most one string swap on exactly one of the strings. Otherwise, return false.
~~~

同じ長さの2種の文字列s1とs2が与えられるので、最大1回の「string swap」をして両方の文字列について等しければtrueを等しくなければfalseを返す問題。
「string swap」とは、2つのインデックスを選んだとき(異なるインデックスである必要はない)、それらの2つのインデックスを添字とする要素について文字の交換をすることである。

# Code

~~~kotlin
class Solution {
    fun areAlmostEqual(s1: String, s2: String): Boolean {
        val list = mutableListOf<Int>()
        for (idx in s1.indices) {
            if (s1[idx] != s2[idx]) list.add(idx)
        }
        if (list.size == 0) return true
        if (list.size != 2) return false
        return s1[list[0]] == s2[list[1]] && s1[list[1]] == s2[list[0]]
    }
}
~~~

s1,s2について、同じインデックスを要素とする文字が同じか異なるかをチェックしながらリストに詰めて、全てチェックした後に以下についてチェックをすれば「string swap」して同じ文字となるかどうかを確認すればよい。

- リストのサイズは0か2となるはずであり、0のときはs1とs2が等しいのでtrueで返して良い。
- リストのサイズが2であるときに、リストの0番目の1番目がs1とs2のインデックスについて、交換すると等しくなる。

計算量はO(n)で、対数時間への改善はちょっと思いつかない。

# Profile

- Runtime: 230 ms
- Memory Usage: 33.9 MB

# Submission

- https://leetcode.com/submissions/detail/610455916/
