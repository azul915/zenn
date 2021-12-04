---
title: "LeetCode 160. Intersection of Two Linked Lists" # 記事のタイトル
emoji: "☺️" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/intersection-of-two-linked-lists/

~~~txt
Given the heads of two singly linked-lists headA and headB, return the node at which the two lists intersect. If the two linked lists have no intersection at all, return null.

For example, the following two linked lists begin to intersect at node c1:

The test cases are generated such that there are no cycles anywhere in the entire linked structure.

Note that the linked lists must retain their original structure after the function returns.
~~~

2つの単方向リストの先頭がheadA, headBとして与えられるので、それぞれのリストが交差するノードを返せという問題
例や図が問題の方に書いてあるので、詳細についてはLeetCodeにてご確認を

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
    fun getIntersectionNode(headA:ListNode?, headB:ListNode?):ListNode? {
        if (headA == null || headB == null) return null

        var visited = mutableSetOf<ListNode?>()
        var pointerA = headA
        var pointerB = headB

        while (pointerA != null) {
            visited.add(pointerA)
            pointerA = pointerA?.next
        }

        while (pointerB != null) {
            if (visited.contains(pointerB)) return pointerB
            pointerB = pointerB?.next
        }
        return null
    }
}
~~~

~~~kotlin
class Solution {
    fun getIntersectionNode(headA:ListNode?, headB:ListNode?):ListNode? {

        if (headA == null || headB == null) return null

        var pointerA = headA
        var pointerB = headB

        while (pointerA != pointerB) {

            if (pointerA == null) {
                pointerA = headB
            } else {
                pointerA = pointerA?.next
            }

            if (pointerB == null) {
                pointerB = headA
            } else {
                pointerB = pointerB?.next
            }
        }

        return pointerA
    }
}
~~~

1つ目はheadAを先頭ノードとするリスト(以下、リストA)、末尾まで探索しながら探索済みのノード(ListNode?)を格納するSet(MutableSet<ListNode?>)に入れていき、headBを先頭ノードとするリスト(以下、リストB)に同じノードが含まれているかを確認している。計算量も $O(n)$ なので特に問題ないと思う。

2つ目は自力で思いついたものではないが、リストAとリストB上を等速で動く点がそれぞれあるような状況として捉えて、
お互いのリストを行ったり来たりしていれば、等速で動いているのだから、2点が同じノードに位置するときにそれが交差するノードのはずだということを利用した解法である。自分で思いつける気がしない。

# Profile
- 1つ目
    - Runtime: 200 ms
    - Memory Usage: 39.3 MB
- 2つ目
    - Runtime: 168 ms
    - Memory Usage: 38 MB

# Submission
- https://leetcode.com/submissions/detail/596208594/
- https://leetcode.com/submissions/detail/595982195/