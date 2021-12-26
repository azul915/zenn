---
title: "LeetCode 405. Convert a Number to Hexadecimal" # 記事のタイトル
emoji: "😞" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: false # 公開設定（falseにすると下書き）
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
制約に-2^{31} <= num <= 2^{31} -1
問題文の箇所でも言及したとおり、`Integer.toHexString(num)`はできないが、

# Profile

# Submission

