---
title: "LeetCode 374. Guess Number Higher or Lower" # 記事のタイトル
emoji: "🥰" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/guess-number-higher-or-lower/

~~~txt
We are playing the Guess Game. The game is as follows:

I pick a number from 1 to n. You have to guess which number I picked.

Every time you guess wrong, I will tell you whether the number I picked is higher or lower than your guess.

You call a pre-defined API int guess(int num), which returns 3 possible results:

-1: The number I picked is lower than your guess (i.e. pick < num).
1: The number I picked is higher than your guess (i.e. pick > num).
0: The number I picked is equal to your guess (i.e. pick == num).
~~~

`int guess(int num)`というAPI(関数)を使いながら、pickの値を特定する問題
二分探索の題材として所謂数当てゲームが挙げられることはさすがに知っているので、二分探索で解き計算量 $O(logN)$を目指す

# Code

~~~kotlin
/** 
 * The API guess is defined in the parent class.
 * @param  num   your guess
 * @return       -1 if num is lower than the guess number
 *                1 if num is higher than the guess number
 *               otherwise return 0
 * fun guess(num:Int):Int {}
 */

class Solution: GuessGame() {
    override fun guessNumber(num: Int): Int {

        var low: Long = 1
        var high: Long = n.toLong()

        while (low <= high) {
            val middle: Long = (high + low) /2
            val res = guess(middle.toInt())
            if (res == 0) {
                return middle.toInt()
            } else if (res < 0) {
                high = (middle.toInt() -1).toLong()
            } else {
                low = (middle.toInt() +1).toLong()
            }
        }
        return -1
    }
}
~~~

~~~kotlin
class Solution:GuessGame() {
    override fun guessNumber(n: Int): Int {

        var low = 1
        var high = n

        while (low <= high) {
            val middle = low + ((high - low) /2)
            val res = guess(middle)
            if (res == 0) {
                return middle
            } else if (res < 0) {
                high = middle -1
            } else {
                low = middle +1
            }
        }
        return -1
    }
}
~~~

制約として、1 \leq n \leq 2^{31} -1 があるので、
middleを求めるために`high + low`をすると32ビットの範囲からオーバーフローしてしまうため、都度Longに変換して計算する必要がある

これは、二分探索のWikipediaで紹介( [Wikipedia/二分探索#実装上の間違い](https://ja.wikipedia.org/wiki/%E4%BA%8C%E5%88%86%E6%8E%A2%E7%B4%A2#%E5%AE%9F%E8%A3%85%E4%B8%8A%E3%81%AE%E9%96%93%E9%81%95%E3%81%84))されていて(以下抜粋)、
二分探索におけるmiddleの求め方として、覚えておいておいたほうが良さそう

~~~
よくある間違いの一つは、上記のC言語のコードで imin + (imax - imin) / 2 を (imax + imin) / 2 としてしまう事である。(imax + imin) / 2 では、imax + imin が int の値の上限 (INT_MAX) を超えて不正な値になってしまう可能性がある。（imax + imin が INT_MAX を超える可能性が全くないと保証できる場合は問題ない。）
~~~

ということで、middleの求め方を`low + ((high + low) / 2)`としたのが2つ目である。
変換部分が削れたからか若干速度も速くなる。

# Profile

- 1つ目
    - Runtime: 128 ms
    - Memory Usage: 32.6 MB
- 2つ目
    - Runtime: 124 ms
    - Memory Usage: 32.9 MB

# Submission
- https://leetcode.com/submissions/detail/597156500/
- https://leetcode.com/submissions/detail/596778973/
