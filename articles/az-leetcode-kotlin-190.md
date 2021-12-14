---
title: "LeetCode 190. Reverse Bits" # 記事のタイトル
emoji: "🤨" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/reverse-bits/

~~~txt
Reverse bits of a given 32 bits unsigned integer.

Note:

- Note that in some languages, such as Java, there is no unsigned integer type. In this case, both input and output will be given as a signed integer type. They should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
- In Java, the compiler represents the signed integers using 2's complement notation. Therefore, in Example 2 above, the input represents the signed integer -3 and the output represents the signed integer -1073741825.
~~~

与えられた32ビット符号なし整数nを反転する問題

# Code

~~~kotlin
class Solution {
    // you need treat n as an unsigned value
    fun reverseBits(n:Int):Int {
        var reversed = 0
        var cn = n
        repeat(32) {
            val bottomBit = cn and 1
            reversed = reversed shl 1
            reversed = reversed or bottomBit
            cn = cn shr 1
        }
        return reversed
    }
}
~~~


![](/images/az-leetcode-kotlin-190/reverse-bits.drawio.png)

# Profile
- Runtime: 112 ms
- Memory Usage: 32.2 MB

# Submission
- https://leetcode.com/submissions/detail/601259597/
