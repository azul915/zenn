---
title: "LeetCode 717. 1-bit and 2-bit Characters" # 記事のタイトル
emoji: "🧐" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/1-bit-and-2-bit-characters/

~~~txt
We have two special characters:

- The first character can be represented by one bit 0.
- The second character can be represented by two bits (10 or 11).

Given a binary array bits that ends with 0, return true if the last character must be a one-bit character.
~~~

0(1桁)と10もしくは11(2桁)の文字表現があり、
0と1で構成されていて、末尾が0の整数配列が与えられるとき、最後の文字表現を0(1桁)とせざるを得ない場合にtrueを返す問題

# Code

~~~kotlin
class Solution {
    fun isOneBitCharacter(bits: IntArray): Boolean {

        var idx = 0
        while (idx < bits.lastIndex) {
            idx = when (bits[idx]) {
                0 -> idx +1
                else -> idx +2
            }
        }
        return idx == bits.lastIndex
    }
}
~~~

0から始まると必ず0(1桁)、1から始まると必ず10もしくは11(2桁)の文字表現なので、
先頭から1桁or2桁のいずれかを割り当てて確定させていくことができる。
これを最終インデックス-1が、つまり先頭から1桁or2桁を確定させてきた和となっていれば、
最終インデックスが0(1桁)であることを確定させられる。(Given a binary array bits that ends with 0 なので、これを利用する)

# Profile

- Runtime: 168 ms
- Memory Usage: 36.4 MB

# Submission

- https://leetcode.com/submissions/detail/601788160/
