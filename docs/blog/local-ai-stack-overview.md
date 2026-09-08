---
created: 2026-07-06 14:00(Asia/Tokyo)
updated: 2026-07-06 14:00(Asia/Tokyo)
type: concept
status: active
tags:
  - ローカルLLM
  - Ollama
  - Hermes-Agents
  - Obsidian
  - AI活用
  - loop-engineering
  - knowledge-management
title: "ローカルLLM / Ollama / Hermes Agent / Obsidian AI活用アーキテクチャ統合"
description: "Brain3 Vaultで運用されているローカルLLM・エージェント・ナレッジ管理の各要素を統合的に整理した概念ページ。Ollama（モデル管理）、Hermes Agent（自律ループ実行）、Obsidian Vault（知識ベース）、Loop Engineering（設計思想）を一本化して解説する。"
---

# ローカルLLM / Ollama / Hermes Agent / Obsidian AI活用アーキテクチャ統合

## 1. このページの目的

このVault（Brain3）でのAI活用基盤を構成する4大要素：

| 要素 | 役割 | 対応ファイル |
|---|---|---|
| **Ollama** + [[llama.cpp]] | ローカルLLM推論エンジン | `[[GPUバックエンド検証記事]]` |
| **Hermes Agent** | スキル定義・自律ループ実行・Cron定期運用 | `AGENTS.md`、 `_agent/memory/*` |
| **Obsidian Vault**（Brain3） | 知識ベース・ナレッジグラフ | `/home/ubun/Documents/AI-Operations/Brain3/` |
| **Loop Engineering** 設計思想 | AI自動化作業のパラダイムシフト | `[[ループエンジニアリング概念ノート]]`

これらを「データ層 → 推論層 → エージェント層 → 知識層」の階層として一体化する。

## 2. システム全体アーキテクチャ

```
┌───────────────────────────────────────────────┐
│  Layer 4: 知識層（Knowledge / Interface）      │
│  Obsidian Vault (Brain3)                       │
│  raw/ → wiki/ → AGENTS.md                      │
├───────────────────────────────────────────────┤
│  Layer 3: エージェント層（Agent / Loop Engine） │
│  Hermes Agent + Skills + Cron                  │
│  AKS / skills/ + _agent/skills/               │
├───────────────────────────────────────────────┤
│  Layer 2: 推論エンジン層（Inference Engine）   │
│  Ollama (GGUF) → llama.cpp → CUDA/Vulkan       │
│  モデル: Gemma 4 12B / Qwen / Llama等          │
├───────────────────────────────────────────────┤
│  Layer 1: ハードウェア層（Hardware）            │
│  GMKtec EVO-X2 (96GB RAM, VRAM48GB)           │
│  + RTX 5090 32GB                               │
└───────────────────────────────────────────────┘
```

## 3. Ollama / llama.cpp（推論エンジン）

### 3.1 基本設計

- **Ollama** がローカルLLMのランタイムとして動作
- モデルは GGUF フォーマット（HuggingFace Hub から管理 → hf CLI でダウンロード）
- 量子化フォーマット: Q4_K_M, Q8_0 等
- バックエンド: CUDA（NVIDIA専用最適化済） / Vulkan（汎用移植層）

### 3.2 GPUバックエンド比較（zephel01 検証結果要約）

| パラメータ | CUDA | Vulkan |
|---|---|---|
| **出力再現性** | temp 0 + seed固定 → バイト単位完全一致 | 同じ条件でも出力は18/18件で不一致 |
| **速度（生成）** | ベースライン | ベースラインから -31% |
| **プロンプト処理** | ベースライン | ベースラインから最大 2倍遅い |
| **無限ループ発生率** | 2/144件（Gemma 4 QAT 12B） | 1/144件 |
| **ハルシネーション内容** | 「量子化で低下した精度を推論時に補正」 | 「llama-quantizeのオプション」と別の嘘 |

### 3.3 重要な教訓

1. temp 0 の決定論出力もバックエンド依存 → "再現性"はビルド番号込みで評価すべき
2. 無限ループ対策として `--repeat-penalty` を必須設定に含める
3. ハルシネーションは環境依存するので、同じ質問でも複数バックエンドで確認すべき
4. Vulkan は移植性重視 / CUDA は最適化重視のトレードオフ

## 4. Hermes Agent（エージェント層）

### 4.1 基本構造

```
Hermes Agent = Gateway + Skills + Cron + Memory + Tools
```

| コンポーネント | 役割 | 配置場所 |
|---|---|---|
| **Gateway** | HTTPサーバー / WebSocket / cronスケジューラ | システムサービス |
| **Skills** | 反復手順のコード化（SKILL.md） | `~/.hermes/skills/` & `_agent/skills/` |
| **Cron** | 定期タスク管理 | `~/.hermes/cron/` |
| **Memory** | 長期記憶 / ユーザープロファイル | `~/.hermes/memories/MEMORY.md` & `_agent/memory/` |

### 4.2 Brain3 Vaultとの連携

- AGENTS.md がVaultの「CLAUDE.md的」役割（ただしAGENTS.md優先）
- `_agent/` ディレクトリでエージェント作業を分離
- `raw/` は読み取り専用、`wiki/` はAI更新可能
- バックアップは `_agent/backups/YYYYMMDD_HHMMSS_` + 条件付き削除

## 5. Obsidian Vault（Brain3）— ナレッジ管理

### 5.1 Level 3 アーキテクチャ（AGENTS.md基準）

```
raw/          → 元素材 / AI読み取り専用
wiki/         → AIが構造化した知識ベース
_agent/       → エージェント作業領域（logs/reports/tasks/skills）
SOUL.md       → 人格・価値観（参照のみ）
AGENTS.md     → Vault主指示ファイル
```

### 5.2 ループエンジニアリングとの関係

Brain3 Vaultの日常運用は **ループエンジニアリング** の実践である：

| ---                | ---                                                    |
| Automations（トリガー） | Cron 朝6:00 / 夜20:00 定期タスク |
| Worktrees（並列隔离） | `_agent/skills/` スキル毎の分離 |
| Skills（スキプト化） | `wiki-maintainer.md`, `content-director.md`等 |
| Maker / Checker | エージェントが生成 → AGENTS.md基準で検証 |
| Durable State | Vault内のmdファイル自体がメモリ |
| HITL（人間の関与） | 承認ゲート > raw/変更・外部送信以外 |

| ループ構成要素 | Brain3実装例 |
| Automations（トリガー） | Cron 朝6:00 / 夜20:00 定期タスク |
| Worktrees（並列隔离） | `_agent/skills/` スキル毎の分離 |
| Skills（スキプト化） | `wiki-maintainer.md`, `content-director.md`等 |
| Maker / Checker | エージェントが生成 → AGENTS.md基準で検証 |
| Durable State | Vault内のmdファイル自体がメモリ |
| HITL（人間の関与） | 承認ゲート > raw/変更・外部送信以外 |

## 6. Loop Engineering — パラダイムシフト

### 6.1 4世代の推移

```
Prompt Engineering (2022-2024)
    ↓
Context Engineering (2025, Tobi Lütke → Anthropic公式)
    ↓
Harness Engineering (2026年初頭)
    ↓
Loop Engineering (2026年6月~) ← 現在
```

### 6.2 ループ構成6要素（Addy Osmani基準）

1. **Automations** — タイマー/イベントでループ起動
2. **Worktrees** — 並列エージェントの衝突回避
3. **Skills** — SKILL.mdで暗黙知を構造化
4. **Plugins/Connectors** — MCP等実行権限
5. **Maker / Checker** — 生成と検証の分離
6. **Durable State** — メモリはディスクに

### 6.3 Brain3での適用状況

|要素|実装済み|未実装|
|---|---|---|
|Skills|wiki-maintainer, content-director等5件|proposals/|
|DurableState|Vault mdファイル群|-|
|M/H分離|AGENTS.md基準|—|
|Automations|Cron（morning-brief/evening-processing）|daily-note-skill化|
|Worktrees|cron/agent毎の分離|—|

## 7. AI活用ワークフロー統合図

```
1. 素材投入 (raw/inbox/)        ← 人間のみ書き込み
2. Loop取り込み (Hermes Agent)    ← cron定期 or リクエストリップ
3. wiki整理 + 概念ページ化        ← skill-maintainerが自動
4. 矛盾チェック (AGENTS.md基準)   ← self-check → report
5. クエリ / インサイト出力        ← user/agentの両方から
6. ループ再帰（Step2へ）          ← cronで自動 or HITLで促進
```

## 8. ハルシネーション対策（推論層→エージェント層共通）

| レベル | 対策 | 担当者 |
|---|---|---|
| GPU/モデル層 | temp 0 + repeat-penalty + max-tokens上限 | Ollama/Llama.cpp設定 |
| エージェント層 | AGENTS.md第12条承認ゲート + ルール強制 | Hermes Agent |
| Vault層 | rawの読み取り専用 + バックアップ必須ルール | Vault設計 |

## 9. 関連コンテンツ

| ノード種別 | ファイル |
|---|---|
### 3. GPUバックエンド検証 | `[[GPUバックエンド検証記事]]` |
| AI外部脳構築 | `[[●AI外部脳構築手順.md]], [[AI外部脳構築.mdd]]` |
| Loop Engineering 4文献 | `[[ループエンジニアリング概念ノート×4]]` |
| Loop Engineering詳細① | `Loop Engineering（ループエンジニアリング）とは — AIエージェントに「指示する」のをやめ、エージェントを回す「ループ」を設計する.md` |
| Loop Engineering徹底解説 | `Loop Engineering 徹底解説 — Prompt → Context → Harness...\.md` |
| ループ×組織論 | `ループエンジニアリングは新発明ではない──ソフトウェア工学が組織論を輸入し始めた日.md` |
| AI活用② (note.com) | `AI有効活用：ループエンジニアリングとは何か...md` |
| Vault環境概要 | `_agent/memory/environment_summary.md` |
| ワークフロー定義 | `_agent/memory/workflow_summary.md` |

## 10. 今後検討すべき課題

- [ ] Ollama固有の量子化・温度設定ガイドライン作成
- [ ] Hermes Agent Cronジョブの標準テンプレート整理
- [ ] Vault × Obsidian Graph Viewとの連携設計
- [ ] Loop Engineering「未実装要素」の優先順位付け（Skills/Worktrees/DurableState）

## 11. 更新履歴

| 日付 | 内容 |
|---|---|
| 2026-07-06 | 初版作成。sources/wiki/concepts/_agent/memory/から16ファイル統合 |
