---
title: "LeetCode 989. Add to Array-Form of Integer" # 記事のタイトル
emoji: "🥶" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/add-to-array-form-of-integer/

~~~txt
The array-form of an integer num is an array representing its digits in left to right order.

For example, for num = 1321, the array form is [1,3,2,1].
Given num, the array-form of an integer, and an integer k, return the array-form of the integer num + k.
~~~

配列の形で与えられる数値(各桁左から右へ順番に並んだ通り)numとそのままの数値として与えられるkの和(num + k)を配列で表現した形で返しなさいという問題

# Code

~~~kotlin
class Solution {
    fun addToArrayForm(num: IntArray, k: Int): List<Int> {
        var cur = k
        val ans = mutableListOf<Int>()
        var i = num.size-1
        while (0 <= i || 0 < cur) {
            if (0 <= i) {
                cur += num[i]
            }
            ans.add(cur % 10)
            cur /= 10
            i--
        }
        return ans.reversed()
    }
}
~~~

num: Intが32bitを超えることがあるので、筆算のような形を取るのが素直。
BigDecimalで計算して、List<Int>に変換するのも正解は導き出せるが、めちゃくちゃ遅いのでナシ。
とはいえ、通常の筆算のごとく、ansに対して右詰めしていくと一番左の桁の処理が面倒なので左詰めしていって反転が楽。

`cur += num[i]`で各桁の和を取り、`cur%10`でその位に残す桁、`cur/=10`で桁上りを計算して次の桁のcurとして計算する。
num.size < "$k".sizeのときのために、if (0 <= i) { cur += num[i] }とする。curはIntなので、剰余を算出したり、割ったあとに自分自身に代入してもオーバーフローする心配はいらない。

# Profile

- Runtime: 398 ms
- Memory Usage: 39.2 MB

# Submission
- https://leetcode.com/submissions/detail/699965117/
