---
title: "LeetCode 1018. Binary Prefix Divisible By 5" # 記事のタイトル
emoji: "😗" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/binary-prefix-divisible-by-5/

~~~txt
You are given a binary array nums (0-indexed).

We define xi as the number whose binary representation is the subarray nums[0..i] (from most-significant-bit to least-significant-bit).

For example, if nums = [1,0,1], then x0 = 1, x1 = 2, and x2 = 5.
Return an array of booleans answer where answer[i] is true if xi is divisible by 5.
~~~


# Code

~~~kotlin
class Solution {
    fun prefixesDivBy5(nums: IntArray): List<Boolean> {

        val list = mutableListOf<Boolean>()
        var num = 0
        for (n in nums) {
            // num = (num * 2 + n) % 5 でも同じ意味なのでいいと思うが、2のべき乗の乗除算は、、シフト演算のほうが速い...?
            num = ((num shl 1) + n) % 5
            list.add(num % 5 == 0)
        }
        return list
    }
}
~~~


例えば、nums = [1, 0, 1, 0, 1, 1, 0] だった場合、
1, 10, 101, 1010, 10101, 101011, 1010110として2進数を5で割り切れるかを判定していけばよい。
桁が増えるたびに、左に1ビットシフト+0または1する。

ロジックは問題なさそうだが、このまま素直に実装したときに2つ問題点が発生した。

1. 10進数で扱いたい変数をintで扱うとオーバーフローする
2. TLE(TimeLimitExceeded)になってしまった

まず、1について、
1 <= nums.length <= $10^{5}$ が制約としてあり、扱える10進数は$10^{30106}$ (※1) でKotlinで解く場合、IntはもちろんLongでも格納することができないため、BigDecimalでの計算を試みた 

次に、2について
BigDecimalで数を扱うことはできるようになったが、おそらく単純に巨大な数の演算を行おうとした結果、200msほどでは結果を求めることができなかった。

巨大な数を5で除算することはロジック上使えないことがわかったので、
以下のような表の調査を行った所、剰余について左シフトして+0または+1したものを5で剰余が0となることのロジックに使っていけることがわかるのでこれで、巨大に数字で判断をする必要がなくなる

| binary | decimal, decimal % 5 == 0 | (decimal % 5) % 5 | (decimal % 5) % 5 == 0 |
| --- | --- | --- | --- |
| 1 | 0*2+1 = 1, false | 1 | false |
| 10 | 1*2+0 = 2, false  | 2 | false |
| 101 | 2*2+1 = 5, true | 0 | true |
| 1010 | 5*2+0 = 10, true | 0 | true |
| 10101 | 10*2+1 = 21, false | 1 | false |
| 101011 | 21*2+1 = 43, false | 3 | false |
| 1010110 | 43*2+0 = 86, false | 1 | false |


※1: https://paiza.io/projects/QZK_zp_x3L1Buyy7oz0fEw

# Profile

- Runtime: 240 ms
- Memory Usage: 39.1 MB

# Submission

- https://leetcode.com/submissions/detail/597865703/
- https://leetcode.com/submissions/detail/598918702/