---
title: "LeetCode 392. Is Subsequence" # 記事のタイトル
emoji: "😆" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/is-subsequence/

~~~txt
Given two strings s and t, return true if s is a subsequence of t, or false otherwise.

A subsequence of a string is a new string that is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (i.e., "ace" is a subsequence of "abcde" while "aec" is not).
~~~

文字列s, tが与えられるので、sがtの**subsequence**のときtrueをそうでなければfalseを返せという問題。
数学の文脈だと「部分列」として訳されるものに該当するようだが、さすがに一般的な概念ではないため、**subsequence**についての説明が続いている。
英語の定義を丁寧に読む必要はなく、「"ace" is a subsequence of "abcde" while "aec" is not」の例で、subsequenceかどうか検証する文字列sの文字以外を文字列tから消したときにすべて位置が保たれていればOKということがわかる。

# Code

~~~kotlin
class Solution {
    fun isSubsequence(s: String, t: String): Boolean {
        if (s.isEmpty()) return true

        var si = 0
        var ti = 0
        while (si < s.length && ti < t.length) {
            if (s[si] == t[ti]) {
                si++
            }
            ti++

            if (si == s.length) return true
        }
        return false
    }
}
~~~

「can be none」となるように文字列sは空文字だと問答無用で**subsequence**なので、trueを返してよい。
**subsequence**であるとき、文字列sも文字の登場順が文字列tにおいて保証されるはずなのであとは、文字列sとtのインデックスを持つフィールドを用意(si, ti)して、文字列tについて線形探索するためにtiをインクリメントする。
文字列t側を回しつつ、文字列sの文字に一致したとき、siの方をインクリメントして一致した回数をカウントできるようにした。
悪くても文字列tの長さだけの線形探索なので、時間計算量はO(n)...？

二重forで回して、内側のforを途中から始められるように、始点をフィールドとして持たせてみることも試した(図.1)が、これも書いてみると分かるように時間計算量はO(n)...？であるもののRuntimeが254 msで時間をより要してしまう

![](/images/12d3c4883306ba/is-subsequence.drawio.png
)


# Profile

- Runtime: 128 ms
- Memory Usage: 33.7 MB

# Submission

- https://leetcode.com/submissions/detail/590113054/