---
title: "LeetCode 551. Student Attendance Record I" # 記事のタイトル
emoji: "😋" # アイキャッチとして使われる絵文字（1文字だけ）
type: "tech" # tech: 技術記事 / idea: アイデア記事
topics: ["leetcode", "kotlin"] # タグ。["markdown", "rust", "aws"]のように指定する
published: true # 公開設定（falseにすると下書き）
---

# Question

- https://leetcode.com/problems/student-attendance-record-i/

~~~txt
You are given a string s representing an attendance record for a student where each character signifies whether the student was absent, late, or present on that day. The record only contains the following three characters:

'A': Absent.
'L': Late.
'P': Present.
The student is eligible for an attendance award if they meet both of the following criteria:

The student was absent ('A') for strictly fewer than 2 days total.
The student was never late ('L') for 3 or more consecutive days.
Return true if the student is eligible for an attendance award, or false otherwise.
~~~

「欠席」、「遅刻」、「出席」を意味する、'A', 'L', 'P'が連結されて文字列sとして渡されるので皆勤賞の条件を満たしているかをBooleanで返してねという問題

皆勤賞の条件は
- 欠席が2日未満であること
- 3連続以上の遅刻がないこと


# Code

~~~kotlin
class Solution {
    fun checkRecord(s: String): Boolean {
        return s.filter { it == 'A' }.length < 2 && "LLL" !in s
    }
}
~~~

これは、アルゴリズムもデータ構造も特にない。
皆勤賞の条件をどう捉えれば、'A', 'L', 'P'で構成される文字列のまま満たすことを検証できるかである。

# Profile

- Runtime: 152 ms
- Memory Usage: 35.4 MB

# Submission
- https://leetcode.com/submissions/detail/599431782/
