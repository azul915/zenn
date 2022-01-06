---
title: "LeetCode 819. Most Common Word" # 記事のタイトル
emoji: "🙆" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/most-common-word/

~~~txt
Given a string paragraph and a string array of the banned words banned, return the most frequent word that is not banned. It is guaranteed there is at least one word that is not banned, and that the answer is unique.

The words in paragraph are case-insensitive and the answer should be returned in lowercase.
~~~

文字列paragraphと文字列配列bannedが与えられるので、paragraphに含まれる単語で最もよく出現する単語を返す。ただし、bannedに含まれるものであってはいけない。
そして禁止されていない単語が少なくとも1つあり、答えが一つしかないことは保証されている。
また、paragraph中の単語は大文字と小文字を区別せず、答えは小文字で返される必要がある。

# Code

~~~kotlin
class Solution {
    fun mostCommonWord(paragraph: String, banned: Array<String>): String {
        val pl = paragraph.split(" ", ",", ".", "!", "?", ";", "'").asSequence()
            .filterNot { it == "" }
            .map { it.toLowerCase() }
            .filterNot { it in banned }
        val mfe = pl.groupingBy { it }.eachCount()
            .maxBy { it.value }?.key
        return mfe?.let { it } ?: ""
    }
}
~~~

アルゴリズムというよりも、データ処理
splitで対象の文字について分割デリミタとしてみなさせ、空白文字となってしまうものについて削る
大文字と小文字について区別せず、答えを小文字で返すのでlowerCaseに変換しておき、文字列配列bannedに含まれるものについて削る
あとは、コレクション中に含まれる最多要素を抽出するために、一度グループ化とカウントを行い、maxByで一意の単語を特定する(that the answer is unique.)

# Profile

- Runtime: 381 ms
- Memory Usage: 49.1 MB

# Submission

- https://leetcode.com/submissions/detail/610796654/
