---
created: 2026-07-06 15:10(Asia/Tokyo)
updated: 2026-07-06 15:10(Asia/Tokyo)
title: "Hermes Agent — Nous Research製自律型AIエージェント"
source: https://github.com/NousResearch/hermes-agent
author: "[[Nous Research]]"
published: 2026-02-04
type: article
status: active
tags:
  - Hermes-Agent
  - AI-agent
  - オープンソース
  - persistent-memory
  - self-improving
---

## Hermes Agent概要

- **Nous Researchが開発した自律型AIエージェント**。2026年2月にオープンソース公開
- **"The agent that grows with you"** — 唯一の内蔵学習ループ付きエージェント
- 経験からスキルを自動生成し、使用中に改善する
- IDEやノートアプリケーションに縛られない

## 主要特徴

| 機能 | 説明 |
|---|:---|
| **Persistent Memory** 永続メモリ | セッション間で記憶を保持。ユーザープロファイル・コンテキスト長期維持 |
| **Skill System** スキルシステム | 反復作業手順をSKILL.mdとして保存・共有・再使用 |
| **Cron Integration** Cron統合 | 定期タスクの自動化 | 
| **Tool Ecosystem** ツールエコシステム | browser、coding、terminal、web、search等多数ツール対応 |
| **Gateway Architecture** ゲートウェイアーキテクチャ | HTTPサーバー/Ws/WebSocket通信 | 
| **Multi-channel Support** チャネルサポート | iMessage(Photon経由)、Raftエージェントネットワーク等 |

## Brain3 VaultでのHermes Agentの役割

本Vaultにおいて、Hermes Agentは:

1. **自動化エンジン**: cron定期タスク(morning-brief/evening-processing)で自律ループ実行
2. **知識整理役**: raw→wikiの半自動変換フローの制御
3. **矛盾検出**: AGENTS.md基準でのvault構造整合性チェック
4. **学習ループ**: 運用経験からskillを自動生成・改善

## Architecture Integration Map

```
Brain3 Vault (Obsidian) ←AGENTS.md→ Hermes Agent → LLLm(Ollama)
      ↑                                      ↓
   Graph View                          cron+Ski
   RAG Chat(Local-LlM-Helpe)           Persistent Memory
   Interactive UI                    tool calling(terminal/web/browser)
```

## 2026年7月時点の最新状態 (GitHub Releasesより抜粋)

- iMessageチャネル新導入(Photon経由)
- Raftエージェントネットワーク対応
- desktopアプリ大幅機能拡張
- subagent実行環境改善
