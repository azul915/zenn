---
title: "LeetCode 441. Arranging Coins" # 記事のタイトル
emoji: "🤬" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/arranging-coins

~~~
You have n coins and you want to build a staircase with these coins. The staircase consists of k rows where the ith row has exactly i coins. The last row of the staircase may be incomplete.

Given the integer n, return the number of complete rows of the staircase you will build.
~~~


# Code

- 1つ目
~~~kotlin
class Solution {
    fun arrangeCoins(n: Int): Int {
        if (n < 2) return 1
        var nn = n
        var steps = 0
        var require = 1
        while (require <= nn) {
            nn -= require
            steps++
            require++
        }
        return steps
    }
}
~~~

- 2つ目
~~~kotlin
class Solution {
    fun arrangeCoins(n: Int): Int {
        var left: Long = 0
        var right: Long = n.toLong()
        while (left <= right) {
            val mid: Long = left + (right - left)/2
            val cur: Long = mid*(mid + 1)/2
            if (cur == n.toLong()) return mid.toInt()

            if (n < cur) {
                right = mid - 1
            } else {
                left = mid + 1
            }
        }
        return right.toInt()
    }
}
~~~

1つ目は最初に思いついた普通の解法。
2つ目はk段の階段のすべてにコインが含まれているときのコインの合計の個数が`1+2+...+k = k*(k+1)/2`であることを利用した二分探索。mid*midでIntでは扱い切れないこともあるので、Longで計算することになる。

# Profile

- 1つ目
    - Runtime: 213 ms
    - Memory Usage: 34.6 MB

- 2つ目
    - Runtime: 292 ms
    - Memory Usage: 34.2 MB

# Submission

- https://leetcode.com/submissions/detail/700027759/
- https://leetcode.com/submissions/detail/700105826/
