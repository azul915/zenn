---
title: "LeetCode 141. Linked List Cycle" # 記事のタイトル
emoji: "😠" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/linked-list-cycle/

~~~txt
Given head, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter.

Return true if there is a cycle in the linked list. Otherwise, return false.
~~~

毎度おなじみのListNodeの連結リストであるheadが与えられるので、Cycleがあるかどうかを判断する問題。連結リストにCycleがある場合はtrueを、そうでない場合はfalseを返す。
連結リストにCycleがある状態とは、後続のノードへのポインタを追い続けることで再び到達することができるノードがリスト内に存在する場合である。
内部的に、posは末尾の次のポインタが接続されているノードのインデックスである。
また、posは引数として渡されるものではない。(単なるCycleをもつ連結リストをheadとposで表現するということ)

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
    fun hasCycle(head: ListNode?): Boolean {
        var current = head
        val visited = mutableListOf<ListNode>()
        while (current != null) {
            visited.add(current)
            if (current?.next in visited) return true
            current = current?.next
        }
        return false
    }
}
~~~

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
    fun hasCycle(head: ListNode?): Boolean {
        var normal = head
        var faster = normal?.next

        while (normal != null) {
            if (normal == faster) return true
            normal = normal?.next
            faster = faster?.next?.next
        }
        return false
    }
}
~~~


訪問済みのListNodeをリストvisitedに詰めながら、都度次のListNodeがvisitedに入っていないかを調べる
あれば、Cycleが存在することになるので、trueを返して終了。
Cycleが存在しないとき、必ずnextがnullとなる地点まで到達するので、whileを抜けることになり、おのずとfalseを返すことになる。
最初は、検索をO(n)で行う事ができるvisitedをListNodeをkeyとするハッシュテーブル(Map)で管理する方式も試したが、実際はListの方がプロファイルが優秀だったため、Listでの実装をFAとした。

別解として、連結リストをたどるポインタを2つ用意して、異なる速さで走らせてみる。
もし連結リストがCycleならば必ず速いほうが遅い方に1周回って追いつくはずという性質を利用した解き方もできる。
パフォーマンスの改善は見られなかった。

# Profile

- 1つ目
	- Runtime: 320 ms
	- Memory Usage: 36.9 MB
- 2つ目
	- Runtime: 298 ms
	- Memory Usage: 41.6 MB

# Submission
- https://leetcode.com/submissions/detail/614295751/
- https://leetcode.com/submissions/detail/614312023/
