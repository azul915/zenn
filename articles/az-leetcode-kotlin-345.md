---
title: "LeetCode 345. Reverse Vowels of a String" # 記事のタイトル
emoji: "🤪" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/reverse-vowels-of-a-string/

~~~txt
Given a string s, reverse only all the vowels in the string and return it.

The vowels are 'a', 'e', 'i', 'o', and 'u', and they can appear in both cases.
~~~

文字列sが与えられるので、母音について反転させる問題

**and they can appear in both cases.** に気をつけたい
一読して理解できなかったが、これはUpperCaseとLowerCaseの両方ともあるよという意味なので、
母音を`a,e,i,o,u,A,E,I,O,U`として扱う必要がある


# Code

~~~kotlin
class Solution {
    fun reverseVowels(s: String): String {

        var left = 0
        var right = s.lastIndex
        val vowels = setOf<Char>('a','e','i','o','u','A','E','I','O','U')
        val sca = s.toCharArray()
        while (left < right) {
            if (sca[left] in vowels && sca[right] in vowels) {
                sca.swap(left, right)
                left++
                right--
            }
            if (sca[left] !in vowels) left++
            if (sca[right] !in vowels) right--
        }
        return sca.joinToString("")
    }
    
    fun CharArray.swap(index1: Int, index2: Int) {
        val tmp = this[index1]
        this[index1] = this[index2]
        this[index2] = tmp
    }
}
~~~

文字列に含まれる母音について反転させるので、母音同士をswapさせることになる

左と右からインデックスを中央方向に移動させながら調べるために変数を用意して、
両方とも母音だったときはswapして、それぞれ中央方向へ進み
母音のときは固定しておく必要があるので、母音でないときに中央方向に進めるようにした。
計算量は$O(n)$

# Profile

- Runtime: 252 ms
- Memory Usage: 38.5 MB

# Submission
- https://leetcode.com/submissions/detail/600145830/
