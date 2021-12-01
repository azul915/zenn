---
title: "LeetCode 155. Min Stack" # 記事のタイトル
emoji: "🥳" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

~~~txt
Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the MinStack class:

MinStack() initializes the stack object.
- void push(int val) pushes the element val onto the stack.
- void pop() removes the element on the top of the stack.
- int top() gets the top element of the stack.
- int getMin() retrieves the minimum element in the stack.

Constraints:
- -2^31 <= val <= 2^31 - 1
- Methods pop, top and getMin operations will always be called on non-empty stacks.
- At most 3 * 104 calls will be made to push, pop, top, and getMin.
~~~

push, pop, top, getMin(最小要素の取得)を一定時間できるようにしたstackの実装を求められている
どんな関数が何回呼ばれても、各々の呼び出し時の時間計算量が $O(1)$ となっていないとAcceptedとなっても、
作問の意図からは外れた実装となってしまう

# Code

~~~kotlin
class MinStack() {

    var stack = mutableListOf<Pair<Int, Int>>()

    fun push(`val`: Int) {

        val min = if (stack.isEmpty()) `val` else stack.last().second

        if (`val` < min) {
            stack.add(Pair(`val`, `val`))
        } else {
            stack.add(Pair(`val`, min))
        }
    }

    fun pop() {
        stack.removeAt(stack.lastIndex)
    }

    fun top(): Int {
        return stack.last().first
    }

    fun getMin(): Int {
        return stack.last().second
    }

}
~~~

スタックにおける最小値とスタックの要素をフィールドに持つclassを作り、その型のMutableListをstackをして扱うなどやり方は色々あると思うが、
KotlinにはPairやTripleといった複数の変数をまとめて扱うためのデータ構造があるためこれを使う
Pair<Int, Int>とし、firstにはstackにpushされるそのものの値を、secondにはpush直後のstackにおける最小値を格納する仕様とした
これによって、最小値を都度計算しなくても、pushするときにtop()な位置にある要素の最小値を見て、それより少ないときには更新しておけば、
getMinも $O(1)$ で返すことができるし、popしたときにstack内の最小値を再計算する必要がなく$O(1)$ で返すことができる

# Profile
    - Runtime: 224 ms
    - Memory Usage: 39.5 MB

# Submission
- https://leetcode.com/submissions/detail/595424070/

P.S LeetCodeのコンパイラ1.3から上げて欲しいなぁ
