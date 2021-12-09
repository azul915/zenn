---
title: "LeetCode 504. Base7" # 記事のタイトル
emoji: "😆" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/base-7/

~~~txt
Given an integer num, return a string of its base 7 representation.
~~~

10進数 -> 7進数(文字列)の問題

# Code

~~~kotlin
import kotlin.math.pow
import kotlin.math.abs

class Solution {
    fun convertToBase7(num: Int): String {
        var target = num
        var base7List = mutableListOf<Int>()

        do {
            val remainder = if (target /7 == 0) target %7 else abs(target %7)
            base7List.add(remainder)
            target = target /7
        } while (target != 0)

        return base7List.reversed().joinToString("")
    }
}
~~~

~~~kotlin
class Solution {
    fun convertToBase7(num: Int): String {

        if (num < 0) return "-${convertToBase7(-num)}"
        if (num < 7) return "$num"
        return "${convertToBase7(num /7)}${num %7}"
    }
}
~~~

10進数からn進数への問題は、10進数が0になるまで基数で割り続けて、毎回算出される剰余を新しいほうから大きな桁として読み上げればよい。

そのシュミレーションをした形。
変換する10進数numがnum < 0のときに、n進数にしたときも0より小さくなるはずなので、基本的に絶対値をとりながら、最後に7で割り切れるときだけ、0より小さい状態でリストに入れると、逆から読み上げたときに0より小さな値として出力される。

2つ目の再帰を自分で思いつくのは難しい...

# Profile
- 1つ目
    - Runtime: 184 ms
    - Memory Usage: 36.1 MB
- 2つ目
    - Runtime: 144 ms
    - Memory Usage: 33.5 MB

# Submission
- https://leetcode.com/submissions/detail/599347748/
- https://leetcode.com/submissions/detail/599369037/
