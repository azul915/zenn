---
title: "Jira 前提のエージェント実行フレームワーク — 計画と実行の分離、そして育て方"
emoji: "🤖"
type: "tech"
topics: ["cursor", "ai", "jira", "workflow"]
published: false
---

## tl;dr

- コーディングエージェントに再現性を持たせるには、**What / How / Why / Must / When NOT** の 5 層を分ける
- 計画は **spec + plan** の 2 本、スキルは **`project-planning`（書く）と `story-execution`（読んで実行）** に分離する
- `story-execution` はプロジェクトごとの雛形ではなく **meta エンジン 1 本**。プロジェクト差分は主に `projects/<id>.env`
- 厚いスキル（SB4 級）は **失敗のたびに Must を硬化**した結果。Tier 1 から始め、同じ失敗が 2 回出たら script 化する
- 本稿は Spring Boot 4 移行（`tokuho-sb4-story`）の実践を抽象化したメモ

## はじめに

Cursor などのコーディングエージェントに「計画どおりゴールまで連れて行ってほしい」と依頼するとき、プロンプトだけでは再現性が足りない。自分たちの Spring Boot 4 移行（SB4）では、`tokuho-sb4-story` スキルと `doc/move-to-sb4/` 配下のドキュメント群が、計画・実行・記録・検証を分離した**オペレーティングシステム**として機能している。

本稿はその実践を抽象化し、今後のプロジェクトでも再利用できる枠組みとして整理した。

## 1. SB4 で何が効いていたか — 5 層

| 層 | 役割 | SB4 での具体例 |
|---|---|---|
| **What** | ゴールとタスクの一次ソース | `jira-ticket-plan.md` の SB4-XX、完了条件 |
| **How** | 設計と実装 | `sb4-dev-parallel.md`、コード変更 |
| **Why** | 判断の経緯 | `work-report.md` の「なぜそうしたか」 |
| **Must** | LLM が忘れやすいことの機械化 | `push_story_branch.sh`、pre-push hook |
| **When NOT** | スコープ外の委譲 | `tokuho-infra-story` など別スキルへ |

計画があるなら、記録とゲートまでセットが SB4 型の強み。

## 2. 計画ドキュメント 2 本の棲み分け

### spec と plan

| | spec（`sb4-dev-parallel.md`） | plan（`jira-ticket-plan.md`） |
|---|---|---|
| 問い | 何を・なぜ・どうやるか | 誰が・いつ・どのチケットで |
| 軸 | 技術フェーズ | Jira Story（SB4-XX） |
| 相互参照 | Phase 6 で plan へ | 冒頭で spec へ |

**plan → spec は中身を読む。spec → plan はチケットを探す。**

技術 Phase と Story は 1:1 ではない。plan は並行作業単位、spec は技術の横断マップ。

### memo → 正本の流れ（SB4 の実例）

```text
探索（チャット）→ spec memo → plan memo → Jira 起票 → doc/<project>/ へ昇格 → 実行 doc 追加
```

| 日時 | 出来事 |
|---|---|
| 7/17 0:30 | `memo-spring-boot-4-sb4-dev-parallel.md` 初版 |
| 7/21 14:10 | `memo-spring-boot-4-jira-ticket-plan.md` 初版 |
| 7/21 午後 | Jira 起票、`doc/move-to-sb4/` へ正本化 |

`~/sugi/memo-*.md` はスナップショット。以降の正本は `tokuho_server/doc/move-to-sb4/` のみ更新。

### 実行フェーズで増える doc

```text
spec / plan（計画）
issue-pr-mapping（問題索引）
reports/TOKUHO-xxxx/work-report（判断記録）
```

## 3. Jira との 3 段 ID

| ID | 例 | 記載先 |
|---|---|---|
| プロジェクトコード | SB4, LMON | plan 見出し |
| Story 連番 | SB4-07 | plan セクション |
| Jira キー | TOKUHO-1315 | 対応表、ブランチ |
| Task 連番 | 7-3 | plan のみ |

## 4. スキル設計 — 計画と実行は別

| | `project-planning` | `story-execution` |
|---|---|---|
| 層 | What（+ spec としての How 設計） | How 実装 + Why + Must + When NOT |
| いつ | プロジェクト開始時 | Story 着手〜完了のたび |
| 関係 | **書く側** | **読む側** |

計画スキルは 1 本で全プロジェクト共用しやすい。実行は **meta 1 本 + `.env`** に寄せる。

### story-execution は雛形ではない

```text
story-execution/          # meta（1 本だけ育てる）
├── SKILL.md
├── projects/
│   ├── registry.yaml     # プロジェクト選択
│   ├── _template.env     # 雛形（ここだけコピー）
│   └── sb4.env
└── scripts/              # 汎用
```

新プロジェクトで本体を複製するのではなく、**`_template.env` をコピー**（Tier 1）。

## 5. 3 段階モデル

**meta は常に存在。Tier はプロジェクト側に何を足すか。**

| Tier | 足すもの | 例 |
|---|---|---|
| 0 | なし | 単発 Story → planning のみ |
| 1 | `doc/` + `.env` | lambda-monitoring |
| 2 | verify script 1〜2 本 | gradlew / terraform |
| 3 | plugin / wrapper | SB4（30+ scripts） |

SB4 の厚さは初回設計ではなく、**#1645（spotless 落ち）など失敗のたびに Must が硬化した結果**。

## 6. ルーティング — どの doc を読むか

### ① プロジェクト選択（registry）

```yaml
sb4:
  env: sb4.env
  jira_parent: TOKUHO-1309
```

Jira キー → registry → `.env` を load。

### ② ドキュメントパス（.env）

```bash
PLAN_REL=doc/move-to-sb4/jira-ticket-plan.md
SPEC_REL=doc/move-to-sb4/sb4-dev-parallel.md
```

### ③ Story → spec 章（plan が持つ）

```markdown
## SB4-07 (TOKUHO-1315): ...
**spec 参照:** Phase A
```

registry はプロジェクト入口、plan は Story 単位の参照。二重管理しない。

## 7. フィードバックループ（標準装備にしたいもの）

| 段階 | 内容 |
|---|---|
| 0 | 手動 1 本 → 迷いをメモ |
| 1 | ワークフローのみ |
| 2 | **同じ失敗 2 回 → script 化** |
| 3 | waxa eval |
| 4 | retrospective-codify |
| 5 | 遡及適用（スキル改善時に既存 doc を放置しない） |

層ごとの出口:

- **Must** が主戦場（verify / hook）
- **Why** → work-report
- 教訓 → `reference.md` 変更履歴に 1 行（SB4 の #1645 パターン）

## 8. 実行フロー（6 フェーズ）

```text
1. Story 特定（plan + Jira Blocks）
2. 環境準備（ブランチ、.env）
3. レポート初期化（完了条件を plan から転記）
4. タスク実行 + 記録（Why）
5. 索引・計画更新（mapping、plan 状態行）
6. 完了確認（Must ゲート → push/PR）
```

## 9. 新規取り組みの判断フロー

```text
単一 Story / 短期？     → Tier 0（planning のみ）
Epic / 複数 Story？     → Tier 1（.env）
専用ゲート必要？        → Tier 2（verify）
doc 分離・複雑 Jira？   → Tier 3（plugin）
```

## まとめ

1. **5 層**で責務を分ける
2. **計画 2 本**（spec + plan）、**スキル 2 本**（planning + execution）
3. **story-execution は meta**。プロジェクト差分は `.env`
4. **ルーティング**は registry + `.env` + plan
5. **育て方はフィードバックが標準** — 同じ失敗 2 回で Must 化

計画で地図を描き、実行エンジンが地図を見て歩き、失敗のたびにゲートを増やす — この循環を仕組みにするのが、エージェントにゴールまで連れて行かせるための実践的な答え。

## 参考

- `tokuho_server/doc/move-to-sb4/` — SB4 正本 doc
- `~/.cursor/skills/tokuho-sb4-story/` — SB4 実行スキル（Tier 3 寄り）
- `~/.cursor/skills/tokuho-infra-story/` — infra 実行スキル（Tier 1 寄り）
