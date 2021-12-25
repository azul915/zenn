---
title: "LeetCode 225. Implement Stack using Queues" # 記事のタイトル
emoji: "😄" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/implement-stack-using-queues/

~~~
Implement a last-in-first-out (LIFO) stack using only two queues. The implemented stack should support all the functions of a normal stack (push, top, pop, and empty).

Implement the MyStack class:

- void push(int x) Pushes element x to the top of the stack.
- int pop() Removes the element on the top of the stack and returns it.
- int top() Returns the element on the top of the stack.
- boolean empty() Returns true if the stack is empty, false otherwise.
~~~

LIFO(last-in-first-out)のいわゆるstackを2つだけのqueue(キュー)を用いて実装しなさいという問題。

stackのclassには、以下の関数が実装されている必要があるみたい。
- void push(int x): stackの末尾にint値xを追加する。
- int pop(): stackの末尾からint値を削除して取り出す。
- int top(): stackの末尾のint値を返す。
- boolean empty(): stackが空ならtrueを、そうでなければfalseを返す。

Notes:には、queueにおける一般的な操作のみで実装すること、具体的には、「push to back(末尾への追加。つまりenqueue)」、「peek/pop from front(先頭要素の参照やdequeue)」、「size(大きさを調べること)」、「is empty(空かどうか調べること)」のみが正当な実装とみなされることが丁寧に記されている。
 また、言語によってはqueue相当のデータ構造がサポートされていない場合もあるので、listやdeque(double-ended queue)を用いてqueueの一般的な操作を表現しなさいともある。
取得は前から、追加は後ろからといういうことを守りさえすれば例えばListを用いても良さそうである。

# Code

~~~kotlin
class MyStack {

    private var q1 = java.util.ArrayDeque<Int>()

    fun push(x: Int) {
        q1.add(x)
    }

    fun pop(): Int {
        var q2 = java.util.ArrayDeque<Int>()
        while (q1.size > 1) {
            val head = q1.removeFirst()
            q2.add(head)
        }
        val lastOne = q1.removeFirst()
        q1 = q2
        return lastOne
    }

    fun top(): Int {
        var q2 = java.util.ArrayDeque<Int>()
        while (q1.size > 1) {
            val head = q1.removeFirst()
            q2.add(head)
        }
        val lastOne = q1.removeFirst()
        q2.add(lastOne)
        q1 = q2
        return lastOne
    }

    fun empty(): Boolean = q1.size == 0
}
~~~

~~~kotlin
class MyStack {

    private var q1 = java.util.ArrayDeque<Int>()

    private var cap = -1


    fun push(x: Int) {
        cap = x
        q1.add(x)
    }

    fun pop(): Int {
        var q2 = java.util.ArrayDeque<Int>()
        while (q1.size > 1) {
           val head = q1.removeFirst()
           if (q1.size == 1) cap = head
           q2.add(head)
        }
        val lastOne = q1.removeLast()
        q1 = q2
        return lastOne
    }

    fun top(): Int = cap

    fun empty(): Boolean = q1.size == 0

}
~~~


java.util.ArrayDeque<E>を用いて実装を行った。
https://docs.oracle.com/javase/jp/8/docs/api/java/util/Deque.html を確認すると、「両端の要素の挿入および削除をサポートしている線形コレクションです」とあるように、
最後方の要素をremoveLast()で取り出すことができ、所謂stack的な使い方もできてしまうので、これを使わないように気をつけたい。

よって、java.util.ArrayDeque<E>をqueueとして使うためには以下のみを使う方針で実装する。

|queueにおける操作|ArrayDeque<E>におけるメソッド(Java)|
|:----|:----|
|enqueue|void add(E e)|
|dequeue|E removeFirst()|
|peek|E peek() |
|size|int size()|
|is empty|boolean size() == 0|
	
この問題の肝であるのはpop()だ。
ポイントは以下、
- 格納しているqueueから最後方を取り出すために、queueの大きさ-1だけ他のqueueに退避させること
- 取り出した後の状態に戻すこと
	
一つ目のCodeでも通るが、
top()について、pop()同様に最後方の要素を確認するために、queueの大きさ-1分だけ走査してしまう。結果として実行時間やメモリ使用量にはほぼ改善はなかったがtop()が頻繁に実行される場合を考慮して修正したのが二つ目のCodeである。

二つ目のCodeでは、最後方の要素の情報を持たせるためのintのフィールドcapを用意している。
add()の時にcapに対して書き込みを行うこととpop()の時にqueueから取り出した直後に残り要素数が1であるとき、取り出した値が最後方となることから、取り出す値で書き込みを行う。
これらを行うことで、最後方の要素の値を参照するだけのtop()はわざわざqueueの大きさ-1分だけ走査する必要がなくなり、フィールドcapの値を返すだけで良くなる。

# Profile

- 上
  - Runtime: 160 ms
  - Memory Usage: 35.6 MB
- 下
  - Runtime: 160 ms
  - Memory Usage: 35.5 MB


# Submission

- 上
    - https://leetcode.com/submissions/detail/589967863/

- 下
    - https://leetcode.com/submissions/detail/589945566/
