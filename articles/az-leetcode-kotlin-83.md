---
title: "LeetCode 83. Remove Duplicates from Sorted List" # 記事のタイトル
emoji: "😃" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/remove-duplicates-from-sorted-list/

~~~txt
Given the head of a sorted linked list, delete all duplicates such that each element appears only once. Return the linked list sorted as well.
~~~

線形リストheadが与えられるので、要素が1つのみになるように重複を削除して、再び線形リストを返しなさい。という問題

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
    fun deleteDuplicates(head: ListNode?): ListNode? {
        var current = head
        while (current?.next != null) {
            if (current.`val` == current?.next?.`val`) {
                current.next = current?.next?.next
            } else {
                current = current.next
            }
        }
        return head
    }
}
~~~

ListNodeのコンストラクタ引数の (var \`val\`: Int) に戸惑ったが、
`val`はKotlinでは予約語なので、バッククォートで回避しているらしい。(わざわざ可読性を落とすようなことを好き好んでしないから初めて知った。)

Exampleを読んで分かるように、ListNodeというclassは、ListNodeの字句の如くListにおけるNodeの値としてintの\`val\`と、ListNodeのnextを持つことで、これが連なり線形リストをなしていること読み取れる。

与えられた先頭のListNodeであるheadから順番に見ていく中で、
とあるListNode(current)の\`val\`とその次のListNodeの\`val\`が同値であるときに値が重複している状況である。よってこれを削除するために、とあるListNodeの次のListNode(current.next)として次の次のListNode(current.next.next)を指すようにする。

current.nextの値がnullとなったとき、currentに続くListNodeがないことになるので、この条件を満たすまでwhileでcurrentに対してその次のListNodeであるcurrent.nextを入れることでcurrentを移しながら続けることになる。


## 1->1->2->3->3のときの例
![](/images/8f1eeb7b07e7f1/remove-duplicates-from-sorted-list.drawio.png)

# Profile

- Runtime: 180 ms
- Memory Usage: 35.8 MB

# Submission
- https://leetcode.com/submissions/detail/589568400/
