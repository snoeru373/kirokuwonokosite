---
title: Hermes AgentでObsidian外部脳化：Brain3 Vaultの運用方法
tags: [Hermes Agent, Obsidian, Brain3, 知識管理]
date: 2026-08-02
status: published
description: Hermes Agentを利用してObsidian Vault（Brain3）を外部脳として運用する方法を解説。自動化管理、Cronスクリプト、記事作成のワークフローを一気に説明します。
---

# Hermes AgentでObsidian外部脳化：Brain3 Vaultの運用方法 {{< internal-link article-03-hermes-agent-ob >}}

## 1. なぜ外部脳なのか？
「AIに聞きっぱなしで終わらせない」「あとで何がやったか分からない」という問題を解決するのが、**知識を外部化する仕組み** です。Obsidianはノード型のメモツールで、双方向リンクにより関連情報を自動で見つけることができます。

Hermes AgentはこのObsidian Vault（Brain3）と連携し、以下のような作業を自動化できます：
- 每日のモーニングブリーフ作成
- 外部情報の収集・要約
- アフィリエイト記事の下書き生成

## 2. Brain3 Vaultの構造
以下の4層で構成しています：
- **_agent/**：エージェントのスキル・ルール・ルーン管理ファイル
- **wiki/**：学習済み知識（AI活用、ツール比較）
- **_agent/tasks**：進行中のタスク管理
- **daily-notes/**：日記・思考ログ

## 3. Cronジョブによる自動化
Hermes Agentには「cron-based-daily-system」スキルがあり、以下のジョブを自動実行します：
- morning-brief（毎日6時）：最新のWiki要約と本日の優先タスクを出力
- evening-processing（20時）：本日の作業記録をまとめ、明日の課題を特定
- inbox-watcher（30分ごと）：届いた情報を整理・分類

## 4..obsidian-vault-bootstrapスキルについて
Obsidian Vaultがまだない場合、bootstrapスクリプトで即座に構造を設定できます。詳しくは[Obsidian Vaultセットアップスキル](https://hermes-agent.nousresearch.com/docs)参照。


---
## 公開前チェックリスト：
- [ ] Cronジョブの設定値（頻度・時刻）が実際に動いているか確認
- [ ] vaultパスの実態と書類内の記述が一致しているか
