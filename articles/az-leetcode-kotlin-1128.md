---
title: "LeetCode 1128. Number of Equivalent Domino Pairs" # 記事のタイトル
emoji: "😎" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/number-of-equivalent-domino-pairs/

~~~
Given a list of dominoes, dominoes[i] = [a, b] is equivalent to dominoes[j] = [c, d] if and only if either (a == c and b == d), or (a == d and b == c) - that is, one domino can be rotated to be equal to another domino.

Return the number of pairs (i, j) for which 0 <= i < j < dominoes.length, and dominoes[i] is equivalent to dominoes[j].
~~~

2つの任意のdomino同士が、equivalent(同等)な条件を整理する
dominoes[i] = [a, b], dominoes[j] = [c, d]のとき、equivalentとなるのは
- a == c かつ b == d
- a == d かつ b == c
のときなので、もう一方のdominoが同じ数字の並び方かその逆となればよい。
例えば、domino[1,2]があったときにequivalentなdominoは[1,2]あるいは[2,1]となる

# Code

~~~kotlin
class Solution {
    fun numEquivDominoPairs(dominoes: Array<IntArray>): Int {
        var ans = 0
        var count = mutableMapOf<Int,Int>()

        for(domino in dominoes) {
            val key = 10*minOf(domino[0], domino[1])+maxOf(domino[0], domino[1])
            ans += count.getOrDefault(key, 0)
            count[key] = count.getOrDefault(key, 0) +1
        }
        return ans
    }
}
~~~

equivalentなdominoを数えるために、domino[0], domino[1]で小さい方を10の位、大きい方を1の位の2桁の10~99の自然数をkeyとしてcountというMapにカウントを格納していく。
新たに、equivalentなdominoを`count.getOrDefault(key, 0)`が評価されたときにansに足すのは、
それぞれのkeyですでにカウントされている分だけequivalentなdominoのペアができるためである。

# Profile

- Runtime: 260 ms
- Memory Usage: 52.2 MB

# Submission
- https://leetcode.com/submissions/detail/605534936/
