---
title: "LeetCode 231. Power of Two" # 記事のタイトル
emoji: "" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/power-of-two/

~~~txt
Given an integer n, return true if it is a power of two. Otherwise, return false.

An integer n is a power of two, if there exists an integer x such that n == 2^x.
~~~

# Code

~~~kotlin
fun isPowerOfTwo(n: Int): Boolean {
    if (n < 1 || 1073741824 < n) return false
    if (n == 1) return true
    var cur = 1
    var shifted = 2
    while (shifted <= n) {
        shifted = cur shl 1
        if (shifted == n) return true
        cur = shifted
    }
    return false
}
~~~

基数が2なのでシフト演算でnの値を増やしながら確かめていくと、普通に *=2するよりも 60%ほど速くなった。

# Profile
    - Runtime: 148 ms
    - Memory Usage: 33.3 MB

# Submission
- https://leetcode.com/submissions/detail/700828027/
