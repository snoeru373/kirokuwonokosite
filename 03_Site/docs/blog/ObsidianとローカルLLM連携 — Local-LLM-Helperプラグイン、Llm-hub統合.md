---
created: 2026-07-06 15:05(Asia/Tokyo)
updated: 2026-07-06 15:05(Asia/Tokyo)
title: "ObsidianとローカルLLM連携 — Local-LLM-Helperプラグイン、Llm-hub統合"
source: https://community.obsidian.md/plugins/local-llm-helper
author: "[[manimohans]]"
published: 2026-05-20
type: article
status: active
tags:
  - Obsidian
  - ローカルLLM
  - プラグイン
  - Local-LLM-Helpe
  - RAG
  - semantic-search
---

## Local-LLM-Helper概要

- **プライバシー重視のObsidian統合LLMプラグイン**
- チャット、セマンティック検索、テキスト変換、ワークフロー自動化をローカルまたはクラウドモデルで実行
- Obsidianを出ずに vault内で完結するAI体験
- Author manimohans, Version 2.4.7, ダウンロード数10k+, Licensed MIT

## 主要機能

| カテゴリ | 内容 |
|---|:--- |
| RAGチャット | ベース検索によるsemantic Q&A、クリック可能なソース参照 |
| ドッキングサイバーチャット | persistent sidebarで一般LLM・RAGチャット、markdownレンダリング、会話記憶 |
| テキストコマンド/プロンプト |要約、書き換え、アクション抽出、カスタム/保存済みPompts |
| 手動ワークフロー自動化 | 週次レビュー、ミーティングからアクション抽出、プロジェクト要約 | writeは明示的な承認カード必須 |
| Related-Notes Sidebar | アクティブまたは選択したテキストに基づいて動的に更新 |

## プロバイダ設定例

```md
## Ollamaの場合
Server http://localhost:11434
Model llama3.2 (browseも可能)
Embedding Model mxbai-embed-large

## OpenAIの場合
Server https:/api.openat.com
API Key [your-key]
Model gpt-4 or gpt-3.5-turbo

## LM Studioの場合
Server http://localhost:1234
モデル Browse loaded modelsまたはデフォルト空白
```

## Quick Start

1. **プロバイダ設定:** `Settings -> Local LLM Helpe` → プロバイダ選択・認証情報を登録
2. **Vaultインデックス:** コマンドパレット -> `Notes: Index notes for RAG` (増分アップデート対応。PDF/OCR添付も対応)
3. **チャット or 変換:** サイドバーで `Chat: Notes(RAG)` を使用またはテキスト選択->コマンドPallete
4. **ワークフロー実行:** コマンドパレット -> `Workflow: Run workflow...` → レシピ選択→承認カード確認→適用

## Recent Changelog Highlights

| Version | Key Features |
|---|:---|
| 2.4.7 | Obsidian markdownレンダリングのオプション + サイドバートグル |
| 2.4.5 | ドッキングチャットサイドバー、複数行コンポーザ、インラインエラーバナー |
| 2.4.2 | 手動ワークフローランナー、persistent Related Notes sidebar |
| 2.4.0 | 保存済みprompts(CRUD+hotkeys)、ペルソナ編集、リーソン抽出トグル |

## Obsidian LLL統合の他プラグイン

- **Obsidian LLm-hub:** AIチャット、ワークフロー自動化、セマンティック検索(Gemini、OpenAI、OpenRouter、Groko、Ollama、CLIバックエンド対応)
- **Copilot plugin:** OpenAI、GoogleAnthropicモデルをデフォルトでサポート
- **Smart Lookup:** 関連文脈プラグイン

## Brain3 Vaultへの示唆

- Local LLM Helpe + Ollama = **Brain3 VaultのRAGチャット基盤**として最適
- Hermes Agentと併せて:
 - Hermesがバックエンドでの知識整理・Cron自動化を担当
 - ObsidianLocalLLMHelpeがフロントエンドでのインタラクティブQ&A/ワークフローを担当
