---
title: "決定版セットアップガイド — Hermes Agent構築"
date: "2026-07-25T06:00:00+09:00"
status: active
tags:
  - " hermes-agent"
  - " local-ai"
  - " tools"
  - " ubuntu"
  - "setup"
---


# 決定版セットアップガイド — Hermes Agent 24時間自律AI社員構築

## 概要
Ubuntuでの安全な実行、コスト最適化（外部APIゼロ）、外部インターフェース準備を含む、Hermes Agentを「24時間自律稼働するシステム（AI社員）」として全機能活用するための決定版セットアップガイド。最新仕様v0.18.2（2026-07-25時点）に基づく。

## 前提条件
- Ubuntu（またはDebian系）、Python ≥3.11, Docker Engine ≥24.0, Ollama (ローカルLLM)
- 空きメモリ: 最低4GB（Ollama+AI Stack込みで推奨8GB以上）
- ネットワーク: 基本LAN内完結、外部公開は `bind_address: 127.0.0.1` で制限

---

## 1. セキュリティ・Blank Slate (初期化)

```bash
# Ubuntu専用ユーザーまたはDockerコンテナ環境推奨（root/ssh直接ログイン不可）
hermes setup
# ※ ブランクスレート選択。ファイル操作とターミナルのみ許可し、他はオフに設定。
```

---

## 2. 構成ファイル更新

`~/.hermes/config.yaml` または `~/.hermes/profiles/main/config.yaml` に以下を追記：

### memory セクション（自動記憶・自己改善ループ①）
```yaml
memory:
  memory_enabled: true        # 長期記憶をオン
  user_profile_enabled: true  # ユーザープロファイルをオン
  auto_memory: true           # 記憶の自動学習を有効化（重要！）
  write_approval: false       # 自動記録時の確認はオフ
```

### curator セクション（スキル自動整理・自己改善ループ②）
既存値の上書き。`consolidate: true` に変更：
```yaml
curator:
  enabled: true
  interval_hours: 168       # 7日ごと
  consolidate: true         # スキルの自動統合を有効化（重要！）
  archive_after_days: 90    # 90日未使用スキルはarchive
```

### auxiliary セクション（ローカル判定・コスト最適化③）
新セクションの追加（`goal_judge:` が存在しない場合は追記）。既存の `curator`, `monitor` と同レベルで追加：
```yaml
auxiliary:
  goal_judge:               # タスク目標判定を無料ローカルLLMへ
    provider: ollama
    model: qwen:8b-instruct  # Ollamaでの推論。他のモデルは変更可能。
    timeout: 30             # 判定に許容する秒数
```

### stt・voice セクション（音声ローカル処理）
以下を追記または上書き（2つのプロバイダを `faster-whisper` に統一）：
```yaml
voice:
  mode: local                # ローカルクラウドの音声認識と合成にフォールバックし、Cloud Whisper, OpenAI APIキー不要
stt:
  provider: faster-whisper   # オンデバイス。GPU (CUDA) 環境なら `faster-whisper-cuda` を推奨
voice_mode.stt_provider:     # ここに指定（hermes config set voice_mode.stt_provider <プロバイダ名>）→ 'faster-whisper' に設定する

# おまけ: テキスト音声 (TTS) にもedge_ttsを使う場合 (インターネットアクセス不要版は `piper` も可)
tts.provider: edge-tts       # Microsoft Edge TTS（ローカル完結型）を推奨。クラウド送信不要。
```

---

## 3. AI社員プロファイルの構築（複数運用）

1つのエージェントではなく、**役割ごとにプロファイルを分離**するのが安定運用の秘訣。最低2プロファイル運用を推奨：

```bash
# スカウトAI（Web調査・競合分析担当）
hermes profile create scout
hermes -p scout chat    # 起動

# アナリストAI（レポート作成・データ分析担当）
hermes profile create analyst
hermes -p analyst chat  # 起動
```

### スキル保護（ピン留め）
自動アーカイブされないように重要スキルを保護：
```bash
hermes curator pin [スキル名1]
hermes curator pin [スキル名2]
```

---

## 4. プロンプト・コマンド集

### A. サブエージェント連携（並列処理・並列推論）
最新版の `delegate_task` は**サブエージェントがデフォルト有効**。並列作業も会話を止めずに可能：

#### 自然言語での依頼例:
> 「[競合他社3社] について、それぞれのサブエージェントを立ち上げて並列で調査してください。親エージェントはそれらの結果を統合してレポートを作成してください」

### B. スラッシュコマンド（セッション実行用）
| コマンド | 役割 | 例 |
|---|---|---|
| `/background` | バックグラウンドでタスク実行 | `/background [新機能の要件定義ドラフトを作成して]` |
| `/steer`     | 進行中の軌道修正 | `/steer [価格帯比較も追加して]` |
| `/queue`     | タスクリストに予約 | `/queue [結果をSlackに送って]` |
| `/model`     | モデル切替（コスト最適化） | `/model qwen:8b-instruct` |
| `/goal`      | 自律ループ開始 | `/goal [対象100件リストアップ → LP構築 → メール起案まで実行して]` |

### C. カンバンによる複雑なタスク管理（複数エージェント連携）:
```bash
# 親タスク作成 & スカウトへ割り当て
/kanban create "[100件のクリニック調査]" --assignee [scout] --goal --goal-max-turns 15

# 子タスクの依存関係設定（親が終わってから動く）
/kanban create "[LP構築]" --assignee [coder] --goal --goal-max-turns 20 --depends-on "[100件のクリニック調査]"
```

---

## 5. ローカル統合サービス群（Docker Compose）

Hermes Agent の `~/.hermes/local-services/docker-compose.yml` に設定済み。以下を起動：
```bash
cd ~/.hermes/local-services && docker compose up -d # full stack: n8n, Stirling-PDF, AppFlowy, Immich, Firecrawl
# or selective:
docker compose up -d n8n stirling-pdf firecrawl    # 代表的なものだけ起動

# アクセスURL一覧:
#   n8n         → http://localhost:5678
#   Stirling-PD → http://localhost:8100
#   AppFlowy    → http://localhost:8200
#   Immich      → http://localhost:2283
#   Firecrawl   → http://localhost:3002/v1
```

### 外部公開する場合（注意必須）
`config.yaml` または `.env` でバインドを `0.0.0.0` から `127.0.0.1` に固定してください：
```yaml
API_SERVER_ENABLED: true
API_SERVER_KEY: [your_secret_key]
bind_address: 127.0.0.1   # ← 必ず外部非公開に！
```

**Photon Spectrum (iMessage)**はMac固有機能なので、Linux(Ubuntu)では動作しません。同等機能は Telegram Bot または Discord bot で代替可能です。Telegram連携設定は `hermes gateway setup` を実行 → `telegram` セクションにBOT_TOKENを入力してください。

---

## 6. Obsidian Vault連携（ナレッジベース・長期記憶）

Brain3 Vault (Obsidian形式) にて、ローカル知識を蓄積：
```bash
# 設定ファイルへの追記:
config.yaml
   wikipath: /home/your_username/Documents/AI-Operations/Brain3/wiki
   obsidianvault-path: /home/your_username/Documents/AI-Operations/Brain3

# ワードの例: 「クライアントとの面談テキスト」→ [議事録]フォルダに格納。既存プロジェクトと矛盾があればフラグを立てる： > [議事録] folder に index.md を生成して、関連wikiへ自動リンクしてください。
```

---


## 7. ループエンジニアリング（自己改善ループ）

Hermes Agent を進化的 AI 社員にする仕組み。以下の 4 つのループが基本：

| ループ名 | 詳細 | 用途 |
|----------|------|------|
| **INPUT** | ローカルデータ取得 → Brain3 Vault に格納 | yt-dlp, Immich, Stirling-PDF, Firecrawl 統合 |
| **PROCESSING** | n8n ワークフローで自動分類・OCR、Immich へアーカイブ | （旧：raw/inbox → Wiki フロー自動化） |
| **OUTPUT** | AI エージェントメタデータ（タグ・要約）生成 | Brain3 Concepts/Articles 自動インデックス |
| **FEEDBACK** | `raw/inbox` → wiki 移行実績を `policies/vault-maintenance-rules.md` 等にフィードバック | スキル改善・メンテナンス循環 |


## 8. チェックリスト（セットアップ完了確認用）

- [x] Blank Slate初期化完了 (`hermes setup`)
- [x] memory, curator, auxiliary.goal_judge 設定反映 (`hermes config set ...`)
- [ ] AI社員プロファイル x2以上作成 (`scout, analyst etc.`)
- [ ] プロンプト集・スラッシュコマンド理解済み (本ドキュメント参照)
- [x] Obsidian/Brain3 Vault連携設定 (brain3/local-ai-integrations.md)
- [ ] Docker AI Stack が起動中 (`docker compose up`)
- [ ] カンバン複数プロファイル運用開始 (`/kanban create --assignee --goal`)


## 9. コスト最適化まとめ（外部 API 費用を $0 に）

| 機能 | ローカル代替方案 | メモ |
|------|-----------------|------|
| 検索・要約判定 | `auxiliary.goal_judge` + Ollama(**qwen:8b-instruct**) | $0/月（既に適用済み ✅） |
| STT（音声認識） | `stt.provider: faster-whisper` (CUDA) | GPU不要の CPU版も可（✅ 済） |
| TTS | `tts.provider: edge-tts` | Microsoft Edge のローカル API（無料、✅ 済） |
| PDF処理 | Stirling-PDF ローカル Docker インスタンス（✅ 準備済み） | |
| スクレイピング/クロール | Firecrawl Local | 外部 Web API 不要（✅ Docker Compose 定義済み） |
| ワークフロー自動化 | n8n Local Docker インスンス | 自動ループのエンジンに最適 |


## 10. セキュリティ & リスク管理

Hermes Agent を外からの接続や操作に対して安全に運用するためには、設定と定期的な監視が重要です：

1. **ファイル制限**: 重要ファイルは `~/.hermes/` に限定し、ローカルのみで利用する（外部共有しない）
2. **Docker ネットワーク**: プライベートIP範囲（例: `172.16.0.0/12`）にバインド
3. **API サーバー**: 基本 `bind_address: 127.0.0.1` で外部公開オフ
4. **承認モード**: データ送信・削除系ツールには approval.mode = manual が推奨


## 11. 関連リンク

- [Brain3 Vault — ツールインテグレーション](local-ai-integrations.md)
- ~~#セットアップログLOOP-~~ (旧： ../raw/ は削除済み → Wiki統合済み)
- [Loop Engineering (概念)](@session:main/)
- [AI 社員プロファイル管理](@hermes CLI → `hermes profile --help`)

---

