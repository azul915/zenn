---
title: "LeetCode 844. Backspace String Compare" # 記事のタイトル
emoji: "😙" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/backspace-string-compare/

~~~txt
Given two strings s and t, return true if they are equal when both are typed into empty text editors. '#' means a backspace character.

Note that after backspacing an empty text, the text will continue empty.
~~~

文字列s, tがテキストエディタに入力される文字として同値がどうかを判定する問題
`#`がbackspace、つまり手前の文字の削除を意味しているという条件が与えられている。

# Code

~~~kotlin
class Solution {
    fun backspaceCompare(s: String, t: String): Boolean {

        fun inputResult(input: String): String {
            var stack = mutableListOf<Char>()

            for (ch in input) {
                if (ch == '#') {
                    if (stack.size > 0) stack.removeAt(stack.lastIndex)
                    continue
                }
                stack.add(ch)
            }
            return stack.joinToString("")

        }

        return inputResult(s) == inputResult(t)
    }

}
~~~

stackに詰めながら、backspaceとして`#`が来たときにstackの先頭にいる要素をpopすればよい
ただし、1文字目から`#`が入力される可能性があるので、不正なindexにアクセスしないようにstackのsizeは確認して、stackが空のときはなにもせずにやり過ごすこと
計算量はtとsのそれぞれの長さの和で線形にかかるので $O(n)$
対数時間で完了できるような方法は特になさそう

# Profile

- Runtime: 243 ms
- Memory Usage: 36.5 MB

# Submission
- https://leetcode.com/submissions/detail/599161450/