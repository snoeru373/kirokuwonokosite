---
title: "Brain3 Vault 運用ガイド v2 — 実運用データに基づいた改善版"
created: 2025-07-18 16:00（Asia/Tokyo）
updated: 2025-07-18 16:00（Asia/Tokyo）
type: article
status: active
tags:
  - Brain3 Vault
  - 運用ガイド
  - LOOP進化版
  - Cron運用
  - AI外部脳
---

# Brain3 Vault 運用ガイド v2 — 実運用データに基づいた改善版 : LOOP #X

## このファイルについて

このv2ガイドは、LOOP#9のBrain3 Vault運用マニュアル(v1)を実運用で検証した結果と、Cronジョブの実稼働データ（morning-brief / evening-processing logs）を基に更新しました。

**主な改善点:**
- LOOP#8→#9を経て得た「理論から実践へのギャップ」の解消
- 実在する10個のCronジョブの運用状況の反映
- raw素材の実際の配置場所とWiki化提案をデータ付きで明記
- _loop_memory.mdのLOOP状態（完了→次 LOOP#X）のエビデンスつき

---

## 1. Brain3 Vault -- 2026年7月時点の実運用状況

### 1.1 Cronジョブ全10個の実稼働データ

v1マニュアルではmorning-brief(日6時)/evening-processing(20時)の2タスク設計でしたが、実運用では **以下の10個のCronジョブが同時に動作**しています。

| # | ジョブ名 | スケジュール | 役割 | v1との違い |
|---|---------|------|------|-----------|
| 1 | morning-brief | `0 6 * * *`（6時） | 朝の重点タスク3つ + The One Thing | v1設計通り ✅ |
| 2 | evening-processing | `0 20 * * *`（夜） | その日の振り返り + the_one_thing達成評価 | v1設計通り ✅ |
| 3 | inbox-watcher | every 30分 | raw/inbox/新着チェック→Wiki化提案（ LOOP#N自動処理） | **LOOP#9追加** |
| 4 | weekly-synthesis | `0 19 * * 0`（日19時） | 週次統合ノート | v1設計通り ✅ |
| 5 | daily-report | `0 7 * * *`（7時） | デイリーステータス報告 | **既存ジョブ追加** |
| 6 | nightly-serendipity-link | `0 2 * * *`（夜2時） | 夜間クロスリンク自動生成 | **既存ジョブ追加** |
| 7 | daily-inbox-check | `0 12 * * * | 正午inbox確認 | **既存ジョブ追加** |
| 8 | weekly-planning | `0 7 * * 1`（月7時） | 週次タスク計画 | **既存ジョブ追加** |
| 9 | inventory-check | `0 9 * * 5`（金9時） | Vault資産インベントリ確認 | **既存job追加** |
| 10 | weekly-content-pipeline | `0 7 * 3 * | 週中コンテンツパイプライン処理 | **既存job追加** |

> **: loop-memory.mdでLOOP #Xの次に何をすべきか？という問いは「実運用のエビデンスを見てから判断する」ことになりました。理論上のnext_taskではなく、real-timeの状態を見て判断します。**

### 1.2 Vault Health Snapshot（7/15 AM時点） -- v1からの改善点

```
raw/inbox/           : ⚠️ 3件（未処理）→ daily-inbox-check cron(正午)が自動処理予定
wiki/md総数          : ~65件 → loopingで継続的に蓄積中
cronジョブ            : ✅ 全10個active (日8連覇=morning-brief gapなし)
backups              : ~97MB（hermes-skillsバックアップが主体）→ クリーンアップは月曜以降回す
tasks/daily          : open-loop状態（raw/直下21件の分類配置が最優先課題）
```

---

## 2. LOOP#1~LOOP#Xで蓄積した知見の統合版 -- Brain3 Vault運用パターン

### 2.1 Brain3 Vaultの核心ループ構造（実運用版 v2）

v1で設計した理論図に、 **LOOP#8→LOOP#9を経ての実運用エビデンスを加えた更新版。**

```
【情報収集層】raw/ ← human書き込み専用（VPS常駐型BOTもこのパターン適用可）
    │
    └── loop_N Wiki化フロー → cron: every30min (inbox-watcher) が自動検知
          │                        ↓
     _loop_memory.md にLOOP#N追記（NNは_loop_memoryで最新LOOP#+1を参照）
               ↓
【知識蓄積層】wiki/ ← cron出力(朝6時/20時) + LOOP_N自動生成
    ├── articles/         → Loop #1→8, 9の結果保存済み（article-loop-0X.md）
    ├── concepts/         → loop-engineering(5編), AFO計画(7編), local-AI Stack (8件)
    ├── policies/         → v2版運用方針統一済（advertising/publishing/blogging/hermes-agent）
    └── sources/          → 原典・出典ノード（Ubuntuベンチ etc.）

【運用管理層】_agent/ ← 全Cronジョブ（10個）、ログ、レポート、タスク
    ├── tasks/daily/     → today-task / evening-task (7/15時点で3件open-loop)
    ├── logs/daily/      → 9ファイル蓄積
    ├── reports/daily/   → status-report等8件蓄積
    └── outputs/briefings/evening/weekly → Cron出力成果物

【自律ループの心臓部】morning-brief + evening-processing
    朝6時（重点3タスク+The One Thing）→ その日中に実行 → night20時(振り返り)
    この「朝決定→夜評価」サイクルがBrain3 Vaultの中核パルス。
```

### 2.2 LOOP哲学の実運用エビデンス

LOOP #1~#9で蓄積した核心概念 + real-worldの実稼働データ：

| 概念(LOOP源) | Brain3での実運用状況 | エビデンス |
|---|---|---|
| **指示ではなく仕組み** | cron全10ジョブ自動実行中 | morning-brief gap = 8連覇（7/8→7/15）の実績（= AIが自律的に朝の重要タスクを確定していること） |
| **3層記憶スタック** | Vault(raw) → Cron(エンジン) → outputs(出力) | _loop_memory.md: LOOP #0→#9 まで完了済み記録。全7件 inbox → Wiki化済みデータ付き |
| **Maker-Checker(審査)** | AGENTS.md基準 + loop-memoryによるLOOP自動追跡 | 矛盾チェック：brain3-rulesとmaster-indexは100%整合（前回の修正で） |
| **Loopが回るほど知見が増える** | loops #8+9→#Xで実運用エビデンスつき | daily logs ×7件, reports×8件 -- loopメモリ上: LOOP#1~8+9=全完了 |

---

## 3. Brain3 Vault運用の日常パターン -- 実例付き

### 3.1 Morning Brief (朝6時) - v2の実例

v1マニュアルには「前日のdaily/をチェック→3つに厳選」とだけ書きましたが、以下は **実際の7/15 morning-briefデータ** を基にした改善版です：

```
[ユーザー用]今日重点3タスク（期限最速 or インプパクト最大順）:
1. raw/直下21件の最終分類案 — ユーザー判断待ち（10日目放置中→金曜まで最終期日）
2. inbox残留3件 — 処理可否を今すぐ指示
3. the ONE THING進捗確認 — raw/分類が完了したか

The One Thing: 【raw/直下21件の分類配置】 -- 「これwiki/sourcesへ」という一言で半分は完了

Open Loops（7/15時点）-- cronジョブ全10個が稼働中:
🔴 High → raw/直下21件(10日目放置) | 🟡 Medium → inbox3件処理中
🟢 Low -> wiki/空folder問題、バックアップ97MB肥大

Before You Start Your Day (今日やるべきこと):
1. 「これsourcesへ」と一言でraw/分類の半分を完了させる
2. inbox残留3件の「消して」「archive」いずれかの指示を出す
```

### 3.2 Evening Processing (夜8時) - v2の実例

v1では単に「今日やったことを記録」でしたが、v2では以下の評価を追加：

```
[ユーザー用]今日完了タスク（確認済み）:
- [done] Cronジョブ全10個の稼働確認 ✅
- [done] morning-brief gapなし（8連覇達成）✅

The One Thing 達成状況: ⚠️ 未達（raw/直下21件の最終分類をユーザー判断待ちのため）

今日の問題(あれば): raw/直下21件→10日目放置 -- この「放置ループ」こそがBrain3 Vault運用上の最大のギャップ（= theory-manualと実運用の差）
```

### 3.3 weekly Synthesis (日曜19時) - v2改善点

v1では「daily/weekly/を整理」とだけありましたが、v2の実運用エビデンスでは：

| 週次チェック項目 | Brain3 Vault実運用時の注意点 |
|---|---|
| dailyタスク完了率 | 7/8~7/15: daily-taskの「The One Thing」未完了が週5日（raw分類の放置による） |
| cronジョブエラー数 | 今日のエラー数=0（全10個正常稼働）✅ -- ただしskill名統一前のjobも混在の可能性 |
| backlogの数 | 7件以上溜まっている場合は月次クリーンアップを提案する |

---

## 4. Brain3 Vault運用の「放置ループ」問題 -- LOOP#Xへの教訓

### 4.1 最大の発見 -- theory-manualと実運用の差 = **raw/素材の大量放置**

v1マニュアル(v9)作成時に一番の問題だったのは **theory vs practiceギャップ**。理論としては `brain3-rules` と `AGENTS.md` の矛盾チェックは完璧（8件すべて修正済み）なのに、実際の運用では：

> **The One Thing** が「raw/直下21件の分類配置」 -- **10日間放置中**.

これはLOOP#9が完成した理由でもある。LOOP #9で実運用データに基づいたv2ガイドを作成できた最大の理由は、このギャップを認識することでした。

### 4.2 LOOP#Xの提案 -- Brain3 Vault実運用改善案（theory→practiceの橋渡し）

| グループ | 施策 | 期待効果 |
|---|---|---|
| **A. raw/素材整理** | userに「wiki/sourcesへ」と一言指示を出す | raw/分類が半分完了。残件10に減る |
| **B. inboxのCron監視を改善** | daily-inbox-check cronも連携してWiki化提案（loop-memory追記済み） → 毎週月曜は「raw/ + inbox」全整理日と定義 | raw素材放置ループ脱却 |
| **C. バックアップ肥大対策** | hermes-skillsバックアップ97MBのクリーンアップ作業を月次で行う（100件超）→ 5%減 | backupの肥大防止 = Vault全体のスピード改善 |

---

## 5. Brain3 Vault運用 -- LOOP #Xへの引き継ぎ情報

### 5.1 実運用評価（LOOP #9完了後の状態）

| ルール | state(evidence) | comment(v2: improvement from v1) |
|---|---|---|
| morning-brief gap | ✅ gapゼロ (8连覇=7/8 → 7/15) | theory→reality。gapなし = AIが自律的に朝のタスクを確定している証拠 |
| evening-processing | ✅ 正常稼働 | v2: the-one thing达成状況を記録する仕組みに改善 |
| inbox-watcher | ⚠️ 3件待ち（cronがwatch中） | LOOP#9追加job. cron every-30mで新着検知→Wiki化提案 |
| raw分類 | ⚠️ 21件放置 (10日目) | theory vs practiceの最大gap。user判断待ち |
| vault-sync | ✅ master-indexと実態100%一致 | LOOP#9作成後に全6件の矛盾修正済み |

### 5.2 Brain3 Vault運用ガイド v2の教訓 -- LOOPループが回らない最大の理由

**結論: Brain3 Vaultの理論上「完璧な構造（AGENTS.md × brain3-rules × loop #1~9）」は機能している。しかし実運用上の放置要因 == user-sideの判断待ち(= raw/素材)。**

LOOP #Xで改善すべきは **AI側の自動化の技術的ギャップではなく、Human側とのインターフェイス** です。

---

## Brain3 Vault 運用ガイド v2 LOOP #X : next loop proposal

| Loop No. | タスク名 |LOOP#9の結果に基づく改善提案内容|
|---|---|---|
| (LOOP #X) | **Brain3 Vault運用実態レポート** | 10個のCronジョブの実稼働データ（morning-brief/evening logs ×7件）を集計し、週次レポートのテンプレートに落とし込む。brain3-rulesとmaster-indexの整合は完璧だったが、「raw素材放置」が実運用最大のボトルネックだったことを記録する |
