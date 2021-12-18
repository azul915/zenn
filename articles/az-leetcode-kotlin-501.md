---
title: "LeetCode 501. Find Mode in Binary Search Tree" # 記事のタイトル
emoji: "🤓" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/find-mode-in-binary-search-tree/

~~~txt
Given the root of a binary search tree (BST) with duplicates, return all the mode(s) (i.e., the most frequently occurred element) in it.

If the tree has more than one mode, return them in any order.

Assume a BST is defined as follows:

- The left subtree of a node contains only nodes with keys less than or equal to the node's key.
- The right subtree of a node contains only nodes with keys greater than or equal to the node's key.
- Both the left and right subtrees must also be binary search trees.
~~~

重複要素を含む二分探索木のルートが与えられるので、その中のモードをすべて配列に列挙しなさいという問題
モードとは、最も頻繁に出現する要素のことらしい

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
    fun findMode(root: TreeNode?): IntArray {
        var visited = mutableMapOf<Int, Int>()

        fun dfs(node: TreeNode?) {
            node?.`val`?.let { 
                if (visited.containsKey(it)) {
                    val v = visited[it] ?: return
                    visited[it] = v +1
                } else {
                    visited[it] = 1
                }
            } ?: run { return }

            node?.left?.let { dfs(it) }
            node?.right?.let { dfs(it) }
        }

        dfs(root)

        val ml = mutableListOf<Int>()
        val mfn = visited.values?.max()
        for ((k, v) in visited) {
            if (v == mfn) ml.add(k)
        }
        
        return ml.toIntArray()

    }
}
~~~

DFSでとりあえず全部辿りながら、visitedというMapにノードに到達したときの値(`val`)をキーとして、valueの値にカウントする
`visited.values?.max()`で最大値を調べることはできるものの、同じカウント数である可能性があるため、これと一致するvisitedのキーを新たに抽出する必要がある
大きさを予測できないので、とりあえずlistに詰めてからIntArrayに変換

# Profile
- Runtime: 510 ms
- Memory Usage: 43.9 MB

# Submission
https://leetcode.com/submissions/detail/602750828/
