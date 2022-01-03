---
title: "LeetCode 234. Parindrome Linked List" # 記事のタイトル
emoji: "😤" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/palindrome-linked-list/

~~~txt
Given the head of a singly linked list, return true if it is a palindrome.
~~

線形リストの値が回文になっているかどうかを確認する問題

# Code

~~~kotlin
/**
 * Example:
 * var li = ListNode(5)
 * var v = li.`val`
 * Definition for singly-linked list.
 * class ListNode(var `val`: Int) {
 *     var next: ListNode? = null
 * }
 */
class Solution {
    fun isPalindrome(head: ListNode?): Boolean {

        val stack = mutableListOf<Int>()
        var current = head
        while (current != null) {
            stack.add(current.`val`)
            current = current?.next
        }
        val half = stack.size /2
        for (idx in 0 until half) {
            if (stack[idx] != stack[stack.lastIndex-idx]) return false
        }
        return true
    }
}
~~~

一度普通のlistに詰めて両端から中心に向かって値を確認していく、
対数時間での実行は難しそう。

# Profile
- Runtime: 569 ms
- Memory Usage: 96.5 MB

# Submission
- https://leetcode.com/submissions/detail/611943403/
