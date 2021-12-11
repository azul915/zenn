---
title: "LeetCode 1360. Number of Days Between Two Dates" # 記事のタイトル
emoji: "😍" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/number-of-days-between-two-dates/

~~~txt
Write a program to count the number of days between two dates.

The two dates are given as strings, their format is YYYY-MM-DD as shown in the examples.
~~~

YYYY-MM-DD形式で2つの日付が与えられるので差分を求めるプログラムを書きなさいという問題

# Code

~~~kotlin
import java.time.LocalDate
import java.time.format.DateTimeformatter
import java.time.temporal.ChronoUnit
import kotlin.math.abs

class Solution {
    fun daysBetweenDates(date1: String, date2: String): Int {
        
        val ld1 = LocalDate.parse(date1, DateTimeFormatter.ISO_DATE)
        val ld2 = LocalDate.parse(date2, DateTimeFormatter.ISO_DATE)
        val diff = ChronoUnit.DAYS.between(ld1, ld2)

        return abs(diff.toInt())
    }
}
~~~

これも作問の意図がわからず。APIを使って解いた。
date1とdate2についてどちらが過去、未来に日付になるかが決まっていないので、とりあえず差分を計算して、絶対値を取る。
例えば30分でうるう年を考慮しながら、自前で実装するのは時間が足りなさすぎる気がする。
ググれない状況でこういった問題出してくるところあるのかな...

# Profile

- Runtime: 176 ms
- Memory Usage: 35.8 MB

# Submission
- https://leetcode.com/submissions/detail/600114780/
