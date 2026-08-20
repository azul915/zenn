---
title: "コーディングエージェントのハーネスは A 駆動で育てる — issue-pr-mapping とゲートの役割"
emoji: "🛡️"
type: "tech"
topics: ["cursor", "ai", "workflow", "jira"]
published: false
---

## tl;dr

- エージェント運用で再現性を上げるには、**事前に全部書く**より **A（命令に対する出力が意図とズレた瞬間）を拾って硬化する**方が現実的
- **work-report** は Story 単位の日報、**issue-pr-mapping** はプロジェクト横断の問題台帳（索引）
- **Must 層**（`verify_*.sh` / pre-push）は「同じ A を二度踏まない」ための固定境界
- A の出口先は 3 層に分ける: **機械検出 → sh**、**短い常時ルール → SKILL**、**手順・判断 → skill / plugin**
- 前編の [Jira 前提のエージェント実行フレームワーク](https://zenn.dev/azul915/articles/agent-planning-execution-framework) が「地図と OS」、本稿は「その OS をどう育てるか」

## はじめに — A とは何か

ここでは **A** を次のように定義する。

> **A**: 人間が意図した完了条件・コマンドの結果・doc の状態と、エージェント（またはスクリプト）の出力がズレた瞬間

例:

- Story ブランチ名は `TOKUHO-1350` だが、中身は main と同一で棚卸し doc が入っていない
- `push_story_branch.sh` を信じて push したら、PR diff に `doc/` が紛れ込んだ
- macOS の bash 3.2 で `mapfile` が落ち、doc 同期が途中で死んだ
- Jira は「完了」だが、完了条件の Slack 実機確認は未着手

こういう A は、プロンプトを長くするだけでは消えない。**一度起きた A を、次は機械か索引で検知できる形に落とす** — これが自分たちのハーネス育成の中心だった。

## 前編との関係 — 5 層のうちどこが育つか

前編で整理した 5 層のうち、A 駆動で厚くなるのは主に次の 3 つ。

| 層 | A が起きたときの典型 | 硬化の例 |
|---|---|---|
| **Must** | push 後に PR を見て「doc が混ざってた」 | `verify_story_pr_diff.sh` |
| **Why** | 「なぜそうしたか」が work-report にない | `verify_story_doc_coverage.sh`（WARN） |
| **索引** | defer した問題を別 Story で二重に踏む | `issue-pr-mapping` + `audit_doc_cross_refs --backlog-gap` |

**What**（plan / Jira）と **How**（spec / コード）は、A から逆算して**追記・修正**されるが、ゲート化の主戦場ではない。

## issue-pr-mapping は何のためか

`issue-pr-mapping.md` は、**問題 ↔ PR ↔ Story** の横断索引。

### work-report との違い

| | work-report | issue-pr-mapping |
|---|---|---|
| 粒度 | 1 Story（TOKUHO-xxxx） | プロジェクト全体 |
| 主な内容 | 実施内容・**なぜそうしたか**・完了条件チェック | 症状・原因・積み先 Story・PR 状態 |
| 読者 | その Story を後から追う人 | 「あの監視ギャップ、どこで直した？」と横断で探す人 |

work-report が **日報** なら、issue-pr-mapping は **案件管理表**。

### 典型セクション（SB4 版が最も充実）

1. **PR 早見表** — PR / Story / Jira / 状態
2. **未対応 → 対応予定 Story** — 既知ギャップ（# 番号付き）
3. **PR 別の問題記録** — 各 PR で何を解消したか
4. **doc 更新時の突き合わせチェックリスト** — work-report・plan・Jira との整合

Lambda 横断監視（`doc/lambda-monitoring/issue-pr-mapping.md`）は SB4 から移植した**薄い初版**。未対応表に「Lambda メトリクスアラームなし → TOKUHO-1351」など、棚卸しで見つかったギャップが載っている。

### Story PR に含めない理由

mapping は **`feature/sb4-docs` / `feature/lambda-monitoring-docs`** で管理し、**Story ブランチの PR には入れない**。

- Story PR = コード変更に集中（レビュー対象を絞る）
- mapping = 全 Story 共通の索引（マージのたびに更新される横断 doc）

PR 本文は短く、defer・判断の詳細は work-report と mapping に書く、という分担。

## A 駆動のループ — 漏れないようにする設計

全部を事前設計できない前提で、**漏れを減らす**ために固定していることは次の 4 点。

### 1. A を記録する（入力を残す）

A が起きたら最低これを残す。

- 何を期待したか
- 実際に何が返ったか
- なぜズレたか（1 行の洞察）

`retrospective-codify` スキルは、まさに **「最初の失敗 ↔ 最終解」** のペアを skill / lint / ルールに固定するためのもの。記録しないと同じ A は次の Story で再発する。

### 2. 固定境界で「まだ codify されていない A」を炙る

| 境界 | 例 | 拾う A |
|---|---|---|
| `push_story_branch.sh` 内 | `verify_story_pr_diff` | doc が Story PR に混入 |
| doc push 前 | `audit_doc_cross_refs --backlog-gap` | mapping 未対応表と work-report の defer がズレ |
| doc push 前 | `audit_doc_cross_refs --plan-gap` | plan の状態行・タスクと実態がズレ |

**「A が起きた → 次はこの境界で WARN/error」** をパイプラインに足すと、同種の A は再発しにくい。

前編の「同じ失敗 2 回 → script 化」と同じ思想。1 回目はメモ、2 回目で Must 化。

### 3. 出力先を 3 層に分ける（全部 skill にしない）

| A の種類 | 置き場所 | 例 |
|---|---|---|
| 機械検出できる | sh / python ゲート | doc が PR diff に含まれる |
| 短い常時ルール | SKILL の When NOT / トラブルシュート | prd は `tokuho_infra-prd` worktree |
| 手順・判断が必要 | plugin スクリプト or 専用 skill | `sync_issue_pr_mapping_status.sh` |

skill を増やしすぎると、エージェントのルーティングが濁る。**検出できるものは検出に寄せる**。

### 4. 意図的にゲートにしない A も明示する

全部を sh にするとハーネスが重くなる。次は **人間確認に残す** ことがある。

- apply 後の Slack 実機確認
- 本番での意図的エラー注入

こういうものは work-report の「未完了・手動」、mapping の備考に書く。A 駆動の正しい処理は **「ゲート化できない A を隠さない」** こと。

## 具体例 — Lambda 監視 LMON-00 / 01 で起きた A

### A-1: Story ブランチと doc ブランチの役割が見えにくい

- **期待**: `feature/TOKUHO-1350` に棚卸し成果物がある
- **実際**: ブランチは main と同一。doc は `feature/lambda-monitoring-docs` にのみ存在
- **硬化**: issue-pr-mapping 冒頭に「doc は doc ブランチで管理。Story PR に含めない」と明記。1350 ブランチは削除して整理

→ 索引と運用ルールの問題。**mapping + verify_story_pr_diff** の領域。

### A-2: bash 3.2 で doc 同期が落ちる

- **期待**: `sync_story_docs.sh` が doc を doc ブランチへ反映
- **実際**: macOS 既定 bash で `mapfile` / `local -A` が使えず失敗
- **硬化**: bash 3.2 互換にスクリプトを修正（plugin 内）

→ 純粋な **Must（スクリプト）** 層。

### A-3: prd Terraform の置き場所

- **期待**: `tokuho_infra` の Story ブランチだけで prd も直せる
- **実際**: prd `060_monitoring` は `tokuho_infra-prd`（`sugi/feature/prd`）のみ
- **硬化**: work-report に「prd ブランチは別」と明記。`feature/TOKUHO-1351-prd` を切る

→ **When NOT / トラブルシュート** と work-report。**ゲート化は難しい**（リポジトリ分割は構造問題）。

### A-4: 完了条件と実装完了のズレ

- **期待**: TOKUHO-1351 完了 = Slack 通知まで
- **実際**: Terraform は書いたが apply・意図的エラーは未実施
- **硬化**: work-report の完了条件チェックを ⏳ にし、mapping #2 を「PR マージ後に更新」状態に

→ **意図的にゲート外**の A。Jira を閉じる前に人間が見る欄。

## sync と audit — mapping を生きた索引に保つ

| スクリプト | 方向 | 役割 |
|---|---|---|
| `sync_issue_pr_mapping_status.sh` | GitHub / Jira → mapping | PR 早見表の状態を自動更新 |
| `audit_doc_cross_refs.sh --backlog-gap` | mapping ↔ work-report | 未対応表の抜け・OPEN PR の # 漏れ |
| `audit_doc_cross_refs.sh --plan-gap` | plan ↔ work-report | Story 状態行・タスクの抜け |

人手で mapping を書き続けると、PR セクションだけ更新して未対応表を飛ばす、という A が起きる（SB4 で #40〜#42 型があった）。**機械チェックは、その A を拾うためのもの**。

## retrospective-codify との接続

セッション終了時（または Story 完了時）に、次を問う。

1. 今日の A で、**ゲートにまだなっていない**ものは？
2. **mapping / work-report に書いたが索引が弱い**ものは？
3. **2 回目の同種 A** なら、どの `verify_*` にする？

`retrospective-codify` はユーザーが明示したときに動くメタスキルだが、運用上は **「A ログ → 週次で Must 候補を洗う」** とセットにすると漏れが減る。

## まとめ — A 駆動を漏れなくする式

```text
A が起きる
  → work-report / mapping に記録（Why + 索引）
  → 同種が 2 回目なら push 境界に verify を足す
  → 教訓が手順化できるなら plugin / skill へ
  → ゲート化できない A は「手動」と明記して隠さない
```

前編の「計画で地図を描き、実行エンジンが歩き、失敗のたびにゲートを増やす」は、その **失敗のたび** を体系化したもの。

issue-pr-mapping は、その循環の **横断メモリ**。Story が増えても「あの問題、どこで触った？」が辿れる。work-report だけだと Story の壁の向こうが見えなくなる。

## おわり

コーディングエージェントのハーネスは、最初から完璧な設計図では作れない。**A 駆動で育てる**前提に立つと、issue-pr-mapping と push 前ゲートの役割がはっきりする。

- **mapping** = 問題の横断索引（defer・解消・PR 状態）
- **verify** = 同じ A の再発防止（Must 層）
- **work-report** = その Story での判断記録（Why 層）

次に同じ型のプロジェクトを始めるときは、Tier 1（`.env` + doc 正本）から入り、**最初の A が出た週末に mapping の雛形と 1 本目の verify** を足す、くらいのペースが現実的だと思う。

---

## 参考 — 自分たちの doc / スキル配置（2026-08 時点）

| プロジェクト | mapping | doc ブランチ | 実行スキル |
|---|---|---|---|
| SB4 移行 | `doc/move-to-sb4/issue-pr-mapping.md` | `feature/sb4-docs` | `tokuho-sb4-story`（Tier 3 plugin） |
| Lambda 監視 | `doc/lambda-monitoring/issue-pr-mapping.md` | `feature/lambda-monitoring-docs` | `tokuho-infra-story` → `story-execution` meta |

関連スキル: `story-execution`, `retrospective-codify`, `empirical-prompt-tuning`（スキル品質の A 駆動評価）
