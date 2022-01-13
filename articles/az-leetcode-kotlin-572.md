---
title: "LeetCode 572. Subtree of Another Tree" # 記事のタイトル
emoji: "😠" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/subtree-of-another-tree/

~~~
Given the roots of two binary trees root and subRoot, return true if there is a subtree of root with the same structure and node values of subRoot and false otherwise.

A subtree of a binary tree tree is a tree that consists of a node in tree and all of this node's descendants. The tree tree could also be considered as a subtree of itself.
~~~


2つの二分木rootとsubRootの根が与えられたとき、subRootと同じ構造とノードの値を持つ。
rootのサブツリーがあればtrueを、そうでなければfalseを返す問題。


# Code

~~~kotlin
/**
 * Example:
 * var ti = TreeNode(5)
 * var v = ti.`val`
 * Definition for a binary tree node.
 * class TreeNode(var `val`: Int) {
 *     var left: TreeNode? = null
 *     var right: TreeNode? = null
 * }
 */
class Solution {
    fun isSubtree(root: TreeNode?, subRoot: TreeNode?): Boolean {

        fun isSameTree(s: TreeNode?, t: TreeNode?): Boolean {
            return when {
                s == null || t == null -> s == null && t == null
                s.`val` == t.`val` -> isSameTree(s?.left, t?.left) && isSameTree(s?.right, t?.right)
                else -> false
            }
        }

        return when {
            root == null -> false
            isSameTree(root, subRoot) -> true
            else -> isSubtree(root?.left, subRoot) || isSubtree(root?.right, subRoot)
        }
    }
}
~~~

2つの再帰関数によって求める。
1つ目は、isSameTreeというもの。これは、引数で渡す2種のTreeNodeのそれぞれの根について、根の値が同じであれば
それぞれ下の2分木も同じように再帰で調べに行くものである。
またどちらかのTreeNodeがnullになったときは、葉まで探索したことになるので、両TreeNodeがnullかどうかをチェックする必要がある。(isSameTreeで探索しきってそれぞれ葉までたどり着いたとき両TreeNodeがnullとなるはず)

2つ目は、isSubtreeというもの。これは、関数の中で1つ目を呼び出してみて、そうでなかったときに子のTreeNodeを調べにいくためのものである。
rootがnullのときは問答無用でfalseを返す。

# Profile

- Runtime: 216 ms
- Memory Usage: 37.9 MB

# Submission
- https://leetcode.com/submissions/detail/613627524/

