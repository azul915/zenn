---
title: "LeetCode 67. Add Binary" # 記事のタイトル
emoji: "🙆" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

~~~txt
Given two binary strings a and b, return their sum as a binary string.
~~~

a, b2つの2進数文字列が与えられるので、和を2進数文字列で返しなさいという問題

# Code

~~~kt
import java.math.BigInteger

class Solution {
    fun addBinary(a: String, b: String): String {
        return BigInteger(a, 2).add(BigInteger(b, 2)).toString(2)
    }
}
~~~

~~~kt
import kotlin.math.max

class Solution {
    fun addBinary(a: String, b: String): String {
    
        val maxLength = max(a.length, b.length) + 1
        val ansArray = Array<Int>(maxLength) { 0 }
        var carried = 0
        for (dist in 1..maxLength) {
            val aidx = a.length - dist
            val bidx = b.length - dist
            val degitA = if (aidx < 0) '0' else a[aidx]
            val degitB = if (bidx < 0) '0' else b[bidx]
            when {
                degitA == '0' && degitB == '0' -> {
                    if (carried == 0) {
                        ansArray[ansArray.size - dist] = 0
                    } else {
                        ansArray[ansArray.size - dist] = 1
                        carried = 0
                    }
                }
                degitA == '0' && degitB == '1' -> {
                    if (carried == 0) {
                        ansArray[ansArray.size - dist] = 1
                    } else {
                        ansArray[ansArray.size - dist] = 0
                        carried = 1
                    }
                }
                degitA == '1' && degitB == '0' -> {
                    if (carried == 0) {
                        ansArray[ansArray.size - dist] = 1
                    } else {
                        ansArray[ansArray.size - dist] = 0
                        carried = 1
                    }
                }
                degitA == '1' && degitB == '1' -> {
                    if (carried == 0) {
                        ansArray[ansArray.size - dist] = 0
                        carried = 1
                    } else {
                        ansArray[ansArray.size - dist] = 1
                        carried = 1
                    }
                }
            }
        }
        return if (ansArray[0] == 0) ansArray.slice(1..ansArray.lastIndex).joinToString("") else ansArray.joinToString("")
    }
}
~~~

作問の意図がわからず、1つ目をまず最初に思いついたが、制限がなければFAでいいと思った
2つ目は2進数同士の和は1つの半加算器と桁数分の全加算器の出力結果であるから、2つの入力degitA,degitBと繰り上がりを表すcarriedについて場合分けを羅列しただけである。
答えを入れるansArrayについて最上位桁が0となる場合はinvalidとなっているのでsliceする必要があるのが、この解法の注意点である。

# Profile

- 1つ目
    - Runtime: 156 ms
    - Memory Usage: 35.6 MB

- 2つ目
    - Runtime: 204 ms
    - Memory: Usage: 37.6 MB

# Submission
- https://leetcode.com/submissions/detail/594955052/
- https://leetcode.com/submissions/detail/594949772/
