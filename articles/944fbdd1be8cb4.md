---
title: "LeetCode 1971. Find if Path Exists in Graph" # 記事のタイトル
emoji: "🙂" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/find-if-path-exists-in-graph

~~~txt
There is a bi-directional graph with n vertices, where each vertex is labeled from 0 to n - 1 (inclusive). The edges in the graph are represented as a 2D integer array edges, where each edges[i] = [ui, vi] denotes a bi-directional edge between vertex ui and vertex vi. Every vertex pair is connected by at most one edge, and no vertex has an edge to itself.

You want to determine if there is a valid path that exists from vertex start to vertex end.

Given edges and the integers n, start, and end, return true if there is a valid path from start to end, or false otherwise.
~~~

二次元配列edgesの形式でグラフに関する情報が与えられるので、始点startから終点endまで、経路が存在するかどうかを調べる問題。以下は条件を整理したもの。

- n個の頂点を持つ双方向グラフがあり，各頂点には0からn - 1（含む）までのラベルが付けられている
- グラフの辺は2次元整数配列edgeで表され，各edge[i] = [ui, vi]は頂点uiと頂点viの間の双方向の辺を示す
- すべての頂点のペアは最大で1つのエッジで接続されており、どの頂点も自分自身へのエッジを持っていない

# Code
~~~kotlin
class Solution {
    fun validPath(n: Int, edges: Array<IntArray>, start: Int, end: Int): Boolean {
        var dests = mutableMapOf<Int, MutableList<Int>>()
        var visited = mutableSetOf<Int>()
        
        for (edge in edges) {
            val from = edge[0]
            val to = edge[1]
            
            dests[from]?.let {
                it.add(to)
            } ?: run {
                dests[from] = mutableListOf(to)
            }
            dests[to]?.let {
                it.add(from)
            } ?: run {
                dests[to] = mutableListOf(from)
            }
        }
        
        fun dfs(node: Int): Boolean {
            visited.add(node)
            if (end == node) return true
            for (child in dests[node]!!) {
                if (child !in visited && dfs(child)) {
                    return true
                }
            }
            return false
        }
        
        return dfs(start)
    }
}
~~~

与えられるedgesを、各nodeをkeyとして、edgeで接続されているnodeをlistとするvalueの形に変換
木構造のグラフの問題に帰着できるので、endが見つかるまで再帰的に探索を続けるDFSで解いた。

# Profile

- Runtime: 1900 ms
- Memory Usage: 336.6 MB

# Submission
- https://leetcode.com/submissions/detail/593802685/
