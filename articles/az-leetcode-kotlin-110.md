---
title: "LeetCode 110. Balanced Binary Tree" # 記事のタイトル
emoji: "😒" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/balanced-binary-tree/

~~~txt
Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

a binary tree in which the left and right subtrees of every node differ in height by no more than 1.
~~~

2分木が与えられるので、`height-balanced`であるかどうかを判定する。
`a height-balanced binary tree`とは、以下の性質を持つ木と定義されていて、
`a binary tree in whtch the left and right subtrees of every node differ in height by no more than 1.`
とあるので、すべてのノードの左右の部分木に高さが1以内の2分木であることが重要。
いわゆるAVL木であるかを調べる問題である。

# Code

~~~kotlini
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
import kotlin.math.abs

class Solution {
    fun isBalanced(root: TreeNode?): Boolean {

        fun dfs(node: TreeNode?): Int {
            if (node == null) return 0

            val ld = dfs(node?.left)
            val rd = dfs(node?.right)

            if (ld == -1 || rd == -1 || abs(ld -rd) > 1) return -1

            return maxOf(ld, rd) +1
        }

        return dfs(root) != -1
    }
}
~~~

基本的なロジックは、
呼び出し元に帰るときに、深さ分の1とその地点からより深いnodeを持つ側(ld or rd)の深さを足して戻すことを行う。
これを、DFSで一番木の深いところまで到達したら、下から深いnodeを持つ側の深さが集計されていくイメージ。
どこかで、その深さの差が2以上あれば、AVL木でなくなってしまうため、-1で返して上に伝播させていく。
最終的にどこかの部分木を処理する`fun dfs`において-1が帰ってしまうと、dfs(root)の呼び出しは-1で帰ることになるので、
AVL木であるかどうかはdfs(root)が-1以外かどうかで判定ができる

# Profile

- Runtime: 291 ms
- Memory Usage: 40.1 MB

# Submission

- https://leetcode.com/submissions/detail/606957634/
