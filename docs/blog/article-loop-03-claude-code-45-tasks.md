---
title: "Claude Codeで日常の45タスクを自動化 — 東大院生の実践記録（要約）"
date: "2026-07-17T06:00:00+09:00"
status: active
tags:
  - "AIパートナー"
  - "cronジョブ"
  - "クロージャコード活用"
  - "システム設計"
  - "自動化"
---


# Claude Codeで日常の45タスクを自動化 — 東大院生の実践記録（要約）

## 出典

- Zenn: [@shunya_sudo](https://zenn.dev/shunya_sudo/articles/claude-code-45-automation-tasks)
- クリップ日時: 2026-07-14

---

## 要約（全文）

東大大学院修士2年の研究者が、Claude Codeを約半年間で**日常業務の45タスクをcronジョブで自動化**した全記録。AIに「全部任せる」のではなく、「考える部分だけAIに任せて最終判断は人間」の設計原则を実現しました。実質コストはClaude Code Maxの$100/月のみ（合計約月1.5万円）。

---

## 全体アーキテクチャ

| カテゴリ | 数値 |
|---|---|
| cronジョブ（定期実行タスク） | 45本 |
| カスタムエージェント定義 | 36個 |
| Pythonスクリプト | 132本 |
| Slack通知チャンネル | 12個 |

全体のフロー: **cron → Python(データ取得/加工) → Claude CLI(テキスト要約・分類) → Slack（出力先）**

ポイント：Claude CLIは「考えるパーツ」として使い、データ取得・加工はPython。判断だけをClaudeに任せる設計が基本。

---

## カテゴリ別の自動化内容

### 1. メール処理（最も効果が大きかった）
- Before: 3アカウントを1日3回手動確認 + 返信に30分〜1時間
- After: Gmail APIで10分おきに取得 + Claude AIが4段階分類（reply/action/see/skip）
- **効果**: 毎日20〜30分の節約。「通知を見て下書きを確認して送信ボタンを押すだけ」

### 2. 日程調整
- Before: 「来週どこかで」と → カレンダー開いて手動確認 → テキストで調整
- After: ICS URL（OAuth不要・読み取り専用）から定期取得 → メール返信に空き日程を自動挿入
- **ポイント**: APIではなくICS URL。BANリスクゼロ、壊れない

### 3. 論文の新着監視
- Before: 週1回手動巡回 → 見逃しが多い
- After: 毎日15時自動実行（RSS + PubMed API）→ Claudeが関連度を5段階評価 → Slack通知 / ダウンロード自動追加
- **効果**: 見逃しがほぼゼロ。「3ヶ月前に出てたのか…」がなくなった

### 4. 日次レポート・週次レポート
- Before: 「今週何やったっけ」と思い出す → メモ書く
- After: 毎日20時自動生成（gitコミット / cron成功状況 / タスクキュー / カレンダー予定を要約）

### 5. AI・テクノロジー情報収集
- Before: X/TwitterやHNを手動巡回 → 時間が溶ける
- After: 1日3回AIニュースを自動収集・要約 → Slack通知
- **効果**: 「情報収集のため」に見る必要がなくなった → SNSは発用に特化

### 6. 面談・ミーティング記録
- Before: ミーティング後に整理（プレッシャーあり）
- After: 録音ファイルを置くなり、録音を放り込むだけ → Whisper(文字起こし) → Claude(要約+アクション抽出) → Slack投稿

### 7. MLコード開発でのAI活用
ペアプログラミングに近い使い方：
- コードの雛形生成「scikit-learnでRandom Forestのパイラインを作ってくれて」→ 動くコード
- デバッグ: エラーメッセージを貼るだけで原因特定
- リファクタリング」「型安全にして」→ 型注釈付きに変換
- テスト生成: ユニットテスト自動生成

### 8. システムの自己監視
「自動化したら放置」ではなく壊れたらすぐ気づける仕組み：
- 毎日9時 ハースチャック（cronジョブ正常実行確認）
- 重要ファイルの最終更新時刻チェック
- エラーログ件数チェック → Slack #ai-system に報告

---

## 自動化の4設計原則（半年で学んだこと）

### 原則1: 「判断」と「作業」を分離する
全部AIにやらせると失敗する。AIには下書き・候補・通知を出させ、最終判断は人間。

### 原則2. 壊れにくい方法を選ぶ
- Google Calendar API → ❌ OAuth認証・トークン更新が必要
- ICS URL → ✅ 読み取り専用、認証不要、壊れない
- Webスクレイピング → ❌ サイト変更で壊れる
- RSS/API → ✅ 安定

### 原則3: Slackをダッシュボードにする
通知先をSlackに統合（#ai-email, #ai-research, #ai-news, #ai-daily, #ai-system）。朝Slackを開くだけで状況把握。

### 原則4: 5分で作れるものから始める
1日目: 30行スクリプト → 1週間後: +50行 → 1ヶ月後: +100行

---

## コスト内訳（月間）
| クラウード | 金額 |
|---|---|
| Claude Code Max | $100/月（約¥1.5万） |
| Slack | ¥0 |
| Gmail API | ¥0（無料枠内） |
| サーバー | ¥0（自分のMacで動作） |
| **合計** | **¥15,000/月** |

時給換算で十分元が取れている。

---

## 私（Shinroueru環境への適用可能性）

Hermes Agent + Cron + Obsidian Vault の組み合わせで似たアーキテクチャが再現可能。

| 東大大学院生の設計 | Brain3 Vaultでの代替案 |
|---|---|
| cronジョブ45本 | hermes cron / Watchers スキル |
| Gmail API (10分ごと) | Himlaya CLI + Herme Agent |
| Claude CLI（要約・分類） | Hermes Chat / Humanizerスキル |
| Slackダッシュボード | Telegram/Discord Gateway通知 |
| 論文監視RSS | blogwatcher スキル |
| システムヘルスチェック | Watchers + cron daily health check |

HermesではSearXNGの追加も設計済み（_agent/instructions/searxng_design.md） → 自前ベースで外部依存ゼロにできる可能性がある。

---

## 関連Wiki

- [[getting-started with loops]] — ループの基本
- [[3層記憶スタック]] — Vault×AI連携の設計原則
- [[SearXNG設計書]] — ローカル検索基盤
- [[rawフォルダ監視設計書]] — Inbox→Wiki自動化の将来設計
