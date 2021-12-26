---
title: "LeetCode 405. Convert a Number to Hexadecimal" # 記事のタイトル
emoji: "😞" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/convert-a-number-to-hexadecimal/

~~~txt
Given an integer num, return a string representing its hexadecimal representation.

For negative integers, two's complement method is used.

All the letters in the answer string should be lowercase characters, and there should not be any leading zeros in the answer except for the zero itself.

Note:You are not allowed to use any build-in library method to directly solve this problem.
~~~

注意しないといけないのは、`You are not allowed to use any build-in library method to directly solve this problem.`である。問題を直接解くために、ライブラリを使ってはいけないという意味である。なので、JVM言語で言う、`Integer.toHexString(num)`
や、Pythonの`hex(num)`のようなことはできない。


# Code

~~~kotlin
import kotlin.math.pow

class Solution {
    fun toHex(num: Int): String {
        val convMap = mapOf<Int, String>(10 to "a", 11 to "b", 12 to "c", 13 to "d", 14 to "e", 15 to "f")
        val bs = Integer.toBinaryString(num) 
        val binStr = when (bs.length % 4) {
            0 -> bs
            1 -> "000$bs"
            2 -> "00$bs"
            else -> "0$bs"
        }
        var ans = ""
        var acc = 0

        for (idx in binStr.lastIndex downTo 0) {
            /* binStr[idx].toString().toDouble() : 0.0か1.0(各桁のbit値を計算のためにDoubleに変換) */
            /* 2.0.pow((binStr.lastIndex -idx) %4) : 基数2と重みの掛算, 4ビットずつ処理をするためlastIndexからの距離と4の剰余で重みが出せる */
            val tmp = binStr[idx].toString().toDouble()*2.0.pow((binStr.lastIndex -idx) %4)
            acc += tmp.toInt()
            if ((binStr.lastIndex -idx) %4 == 3) {

                val degit = if (acc > 9) convMap[acc] else acc.toString()
                ans = "$degit$ans"
                acc = 0
            }
        }
        return ans
    }
}
~~~

10進数->16進数は、与えられた10進数を16で割り続けてその剰余を下の位から並べることで、変換できるが、
制約に-2^{31} <= num <= 2^{31} -1とある中で、`For negative integers, two's complement method is used.`とあるように、
負の整数には2の補数表現が適用されるため、負の整数のときは、2進数の補数表現にしてから、再び10進数に戻す作業が必要になるため、あまりやりたくない。

問題文の箇所でも言及したとおり、`Integer.toHexString(num)`はできないが、2進数表現にすることは禁じられていないと受け取れるので、遠慮なく負の整数も`Integer.toBinaryString(num)`に入れて、unsignedな2進数にしてから、4ビットずつ10進数に戻す(ただし、10以上15以下についてはa~fでの表現)作業をすればよい。

4ビットずつ処理するときに、上位1~4ビットのロジックがシンプルになるように0埋めを行っている。

# Profile
- Runtime: 160 ms
- Memory Usage: 35.7 MB

# Submission

- https://leetcode.com/submissions/detail/606838075/
