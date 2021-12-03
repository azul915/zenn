---
title: "LeetCode 1. Two-Sum" # 記事のタイトル
emoji: "😄" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/two-sum/

~~~txt
Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.
~~~

intの配列numsとintの整数値targetが与えられるので、和がtargetと同値になる配列numsにおけるindexを返しなさいという問題

# Code

~~~kotlin
class Solution {
    fun twoSum(nums: IntArray, target: Int): IntArray {
        var m = mutableMapOf<Int, Int>()
        for (i in nums.indices) {
            val complement = target - nums[i]
            if (m.containsKey(complement)) {
                return intArrayOf(m[complement]!!, i)
            }
            m[nums[i]] = i
        }
        return intArrayOf()
    }
}
~~~

予めmapを用意して、配列numsをイテレートしながら値とindexをmapに詰める中で、
targetとの差を取ったとき、mapにその値があれば、値をkeyとしてindexを取り出す方式。
最初は2重forで、内側のスタート値を外側+1にして、重複の探索を避けられるようにしていたが、
mapに詰めて戻れるようにすることでO(1)でindex値を見つけることができる。(m.containsKey(complement)箇所)

時間計算量、空間計算量ともにO(n)となるので、とりあえず自分のBAとしてはこれで良いのかなと思う。

i = 0のときはm.containsKey(complement)がtrueになることはありえないので、`m[nums[i]] = i`と格納してから、i = 1からイテレーションを始めることもできる。
だが、配列numsが長いほど誤差なので、素直にすべて回してしまった。

ちなみにreturn intArrayOf()は`You may assume that each input would have exactly one solution`, `You can return the answer in any order.`とあることから、ここを通ることはない。戻り値の都合で書いているdeadcodeである。

# Profile

- Runtime: 192 ms
- Memory Usage: 38 MB

# Submission
- https://leetcode.com/submissions/detail/589142602/
