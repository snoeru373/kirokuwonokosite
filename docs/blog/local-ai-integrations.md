---
title: "ローカル統合ツールインテグレーション — Hermes Agent 連携"
date: "2026-07-25T06:00:00+09:00"
status: active
tags:
  - " Docker"
  - " Hermes-Agent"
  - " インテグレーション"
  - " ツール連携"
  - "ローカルAI"
---


# ローカル統合ツールインテグレーション — Hermes Agent 連携

## 概要

2026年7月25日にHermes Agent に導入したオープンソースツールの連携基盤。以下の7つを**2つのアプローチ**で統合:

| # | ツール | クラスタ | カテゴリ | 運用形態 |
|---|--------|---------|----------|----------|
| 1 | yt-dlp | CLI | メディア | bashラッパー + manifest |
| 2 | Whisper.cpp | CLI | オーディオ | bashラッパー + manifest |
| 3 | LocalSend | CLI | ネットワーク | bashラッパー + manifest（承認必須） |
| 4 | n8n | Docker+MCP | 自動化 | MCPサーバー接続 |
| 5 | Stirling-PDF | Docker | ドキュメント | HTTP API |
| 6 | AppFlowy | Docker | ドキュメント/DB | HTTP API |
| 7 | Immich | Docker | メディア管理 | HTTP API + MCP |
| 8 | Firecrawl | Docker+MCP | Web | MCPサーバー接続 |

## アクセス一覧

| サービス | URL | 認証 | 用途 |
|----------|-----|------|------|
| n8n | http://localhost:5678 | Basic（初回アカウント作成） | ワークフロー自動化 |
| Stirling-PDF | http://localhost:8100 | なし | PDF処理（結合・OCR・分割） |
| AppFlowy | http://localhost:8200 | なし | ドキュメント/DB/プロジェクト管理 |
| Immich Web | http://localhost:2283 | APIキー生成必須 | 画像・動画ファイル管理 |
| Firecrawl API | http://localhost:3002/v1 | ローカルは不要 | Webコンテンツ抽出 |

## ディレクトリ構造

```
~/.hermes/
├── custom-tools/          # ← CLIラッパー群
│   ├── yt-dlp/
│   │   ├── scripts/
│   │   │   └── tool.sh   # YouTube動画DL / audio抽出
│   │   └── tool.yaml     # manifest (manifest_format: tool_schema_v2)
│   ├── whisper/
│   │   ├── scripts/
│   │   │   └── tool.sh   # 文字起こし (TXT/SRT/VTT出力)
│   │   └── tool.yaml
│   └── localsend/
│       ├── scripts/
│       │   └── tool.sh   # LAN宛ファイル送信（承認必須）
│       └── tool.yaml
├── local-services/        # ← Docker + API manifest群
│   ├── docker-compose.yml           # 8サービス一括起動定義
│   ├── README.md                    # インストール手順書（←このファイルと連動）
│   └── tool-manifests/              # Manifest YAML一覧
│       ├── n8n.yaml
│       ├── stirling-pdf.yaml
│       ├── appflowy.yaml
│       ├── immich.yaml
│       └── firecrawl.yaml
└── config.yaml                # MCP設定 + custom_tools 一覧（追記済み）

Brain3 Vault/
├── wiki/tools/local-ai-stack-integration.md   # ← このファイル
├── master-index.md                              # カテゴリ「tools/」を追記済み
└── raw/setup-local-ai-stack-2026-07-25.md     # 設置記録

```

## 安全制御（承認・リスク）

すべてのツールは`tool.yaml`内で `danger_level` で分類:

| danger_level | ツール | 制御内容 |
|---|---|---|
| `write_outside_default` | yt-dlp download | ファイルサイズ上限2GB, output_dir固定 |
| `destructive_transfer` | LocalSend send | 外部デバイス送信 → **manual承認必須** |
| `write_to_media` | whisper transcribe | Brain3出力配下に書き出し、CPU長時間実行あり |
| `none` | n8n, Stirling-PDF, AppFlowy, Immich, Firecrawl | ローカル完結なので risk-low |

global設定は `approvals.mode: manual`（デフォルト）のままで保持。

## ループ連携ポイント

このインテグレーションは「出力」のみで終わらない。以下のように**進化的ループ**を張る🔁：

- **Input**: ローカルで収集したメディア / PDF / 画像 → Brain3 Wiki化
- **Process**: n8nワークフローで自動分類・OCR → Immichにアーカイブ
- **Output**: AIエージェント用メタデータ（タグ、要約、インデックス）生成
- **Feedback**: `raw/` から wiki/ への移行実績を `policies/vault-maintenance-rules.md` にフィードバック

詳細は [[Loop Engineering — Tool Self-Evolution]] を参照。

## 関連ページ

- [[Master Index]]（Vault索引）
- [[Loop Engineering 徹底解説]]
- [[local-ai-stack-overview]]
- Obsidianプラグイン連携：[[ObsidianとローカルLLM連携]]
