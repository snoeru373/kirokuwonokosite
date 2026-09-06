---
title: "Brain3 × AI活用 統合設計書 v1"
created: 2026-07-17 18:45（Asia/Tokyo）
updated: 2026-07-17 18:45（Asia/Tokyo）
type: article
status: active
tags:
  - Brain3 Vault
  - AI活用設計
  - プロンプトエクスプローリング
  - 3層記憶スタック
  - ループエンジニアリング
---

# Brain3 × AI活用統合設計書 v1 — LOOP進化版（7記事統合）

## A. 哲学的基盤 — Why: 何のためにAIと関わるのか

### AI活用の根底姿勢（又吉直樹式）

> **「天才」の定義：何でも知っている人ではなく、普通なら途中で面倒になって閉じる**
> **問いを閉じずに持ち続けられる人のことだ。**

又吉直樹は夜ごとに約3時間、AIと対話し、**答えを急がせない。作品を丸ごと書かせようともしない。AIから「問題」を出してもらい、自分で考え、反論し、別の角度から問い直し、言葉が意味を失う地点まで会話続ける。**

また吉式思考の5つの動き：
1. **細部を拾う** — テーブルに付いた輪ジミなど抽象的な問題は具体的な細部から立ち上げる
2. **すぐに意味を決めない** — 「これは現代人の孤独」などとまとめず、偶然かもしれないと留める
3. **自分の外へ出る** — 植物・アリ・宗教・経済・文学を視点を持ち込み最初の解釈を崩す
4. **矛盾を残す** — 優しいけれど残酷。正しいけれど嫌い。どちらかを誤りとして消さない
5. **全体を組み替える** — 問いの順番を変える

> 「答えが簡単に整い過ぎたとき、もう1回壊すしつこさ」——これが又吉式思考の核心。

### Prompt Engineering → Prompt Exploring

| タイプ | 目的 | プロンプトは |
|---|---|---|
| **Prompt Engineering** | 到着地点を決めて精度を上げる | 命令文。条件が明確であることほどよい |
| **Prompt Exploring** | 出発地点だけを決めて到着地点を固定しない | ルール。途中で条件を書き換えることにも価値がある |

又吉：「私の使い方では、プロンプトは命令文ではなく、対話のプロトコル（= ルール）になる」。これがLoop #4の核心概念——**Prompt Exploring**。

### 遅い思考 — Fast & Slow Thinking in AI Era

> AIは速い：数秒で要約し数十秒で企画を出し、長い文書を読み、複数の情報を統合する

又吉は、その速さを使って「早く答えを得ようとしていない」——**より多くの問いを作る**ためにAIを使う。これが「遅く考える」の核心。

### 思考の最初と最後を人間に残す

| AIに任せられる | 自分が決める |
|---|---|
| **作業の大部分** | **What, How fast, Why** |
| シグナル生成 | 何が疑問か |
| 調査（ディープサーチ） | なぜそれを調べなければならないか |
| 文書要約 | 何を自分の言葉として引き受けるか |

---

## B. コンセプト設計層 — How: AI + Obsidianを統合して何を作るのか

### 3つの基本要素

| 概念 | Loop # | Brain3での実装 |
|---|---|---|
| ループとは何か（ループエンジニアリング） | loop-01 | Claude Codeの定義：エージェントが停止条件を満たすまで作業サイクルを繰り返す仕組み |
| 3層記憶スタック | loop-02 | Obsidian(記憶の地層)→ AI操作員(読み込み・推論エンジン) |
| Brain3 Vault構造 | — (既存wiki/AGENTS.md参照) | _agent/inbox/raw → wiki/articles → reviews |

#### ループの基本構造（loop-01から抜粋）

又吉式ループでは「主語が人間から動かない」：
```
AIに問題を出させる → 自分が考える → AIが別の角度を示す → 自分が考え直す
         ↑                           ↓
    バックリンクで繋がったノードがAIにとりの読める記憶の地層になる
```

### ループの実行パターン（LOOP #1-#7 の実績から）

| タイプ | 対応記事 | 説明 |
|---|---|---|
| **探索型ループ** | loop-04 (又吉式思考術) | AIに問いを出させて自分で考える、Prompt Exploring |
| **構造型ループ** | loop-01, loop-03, loop-05 | 既存のWiki記事を要約・cross-link化し新しい wiki を構築 |
| **自動化型ループ** | loop-06 (米国株Bot) | ルール→AI審査→実行 の2段ゲート |

---

## C. AI活用パターン — What: Brain3 Vaultでどんなことができるか

### 1. スキル運用（Hermes Agent + Obsidian vault連携）

| セクション | loop-01 (Concept) | Loop #実装例 |
|---|---|---|
| ループ構造 | AIが自身でチェックして修正・再検証する自己ループ | LOOP#1-7 で Wiki 化の実践 |
| 3層メモリ | Obsidian(記憶の地層), AI(操作員) → プロンプト（推論エンジン） | Brain3 Vault(AGENTS.md, loop_memory.md, wiki/) + Hermes Agent |

### 2. NotebookLM32活用法 × Claude Codeとの比較

| テーマ | NotebookLM | Claude Code/Hermes | 両方の併用 |
|---|---|---|---|
| **得意** | 素材ベースの要約・音声化・スライド生成 | ゼロ→ありのコード生成、自動化 | NotebookLM→素材集め →Claude→処理結果の統合 |
| **弱点** | 既存資料がないと使われない | 学習データにない情報は「存在しない」 | NotebookLMを「一次情報保管庫」として使う |

> **「32番目：掛け合わせが重要」。NotebookLMは素材ベースの要約・音声化・スライド生成。Claude Codeはゼロからコードや自動化スクリプトを生成。**
> **両方を使うことで、「素材→要約→分析→統合」のパイプラインになる**。

### 3. Claude Code活用パターン（loop-06の米国株Botで学んだもの）

| パターン | Brain3 Vaultでの使い道 |
|---|---|
| Paper→実弾 (Virtual → Real) | raw/inbox でテスト → Wiki 化で確定 → _agent/reports/ に保存 |
| AI審査ゲート | ルーチンにルールシグナル→Claudeが審査 | SlackやTelegramの通知先も同じ |

### 4. AIによる思考支援パターン（又吉式思考術から学んだもの）

| アプローチ | Brain3 Vaultでの使い道 |
|---|---|
| **壁打ち5連コンボ** | loop-propt-templateで「この仮説の弱点は？反対意見は？」を自問 |
| **概念耐久試験** | AIに言葉を壊れるまで問い返す。loop_memory.md に記録 |
| **五者反対尋問評議会** | AIに5つの異なる視点（実証/歴史/当事者-child/100年後）で反論させる |

---

## D. ツール比較・選び方 — Which: どう使い分けるか

### セクター別のツール選定マトリックス

| セクター | 推薦ツール | Brain3での応用 |
|---|---|---|
| **AI/LLM関連検索** | Arxiv + Blogwatcher + QMD（将来的に） | AI研究、論文の探索 |
| **自動化・Pythonコード** | Claude Code / Codex / Hermes Agent cron | ループの自動化、rawフォルダ監視 |
| **UIデザイン・プロトタイピング** | Claude Design vs OpenDesign vs CoDesign | ブログ記事のプロトタイプ用 |
| **PDF・OCR** | ocr-and-documents (builtin) | 行政資料の読み取り |
| **動画/字幕要約** | youtube-content + Humanizer | YouTubeをNote記事素材化 |
| **検索エンジン** | SearXNG or WebSearch（将来SearXNGコンテナ追加予定) | ローカル検索基盤 |

### NotebookLM vs Claude Code の使い分けガイド

| 課題 | NotebookLM | Claude Code/Hermes | 推薦 |
|---|---|---|---|
| **大量PDFの要約** | ⭐⭐⭐ (引用元つきで裏取り) | ◯ (OCRスキルで抽出) | NotebookLM |
| **自動化スクリを書く** | ✗ | ⭐⭐⭐ (実装可能) | Claude Code/Hermes |
| **資料作成(スライド/音声)** | ⭐⭐⭐ (自動生成) | ○（python-pptxなど） | NotebookLM |
| **AIへの問い出し・思考の深化** | △（壁打ち機能あり） | ⭐⭐⭐（又吉式思考術とPrompt Exploring） | Claude Code/Hermes |

---

## E. Brain3 Vaultでの実装例 — Where/How: 具体的にどう運用するか

### Brain3 Vault × AI活用 の設計パターン

```
[raw/inbox/](未整理 → wiki/articles/)
          │                      │
          ├─ ① NotebookLM → 要約 | 音声化（一次情報保管庫）
          ├─ ② Claude Code/Hermes → 分析・Wiki化 → [[Wiki記事]] 
          └─ ③ Loopプロンプト → [[Wiki記事]]の整合性チェック → [[loop-memory.md]] に記録
```

### loop #8の実行パターン（新着raw素材がある場合）

1. raw/inbox/の新着ファイルを確認
2. ループ#で Wiki化（要約 + カテゴリ分類 + frontmatter）
3. article-loop-0NN として wiki/articles に保存
4. _loop_memory.md に loop #N の結果を追記 → 残り課題をメモ
5. cross-linkを既存Wiki追加

---

## F. Loopプロセスの統合 — The Meta-Layer: LOOPとは何か revisit

### LOOPの概念（第1,2,3, loopsで定着）

| Layer | CONTENT | LOOP #で具体化 |
|---|---|---|
| **Philosophy** (why) | Prompt Exploring, 遅く考える, AIに問いを与えろ | loop-04 / loop-6 |
| **Conceptual** (what) | ループの基本構造・3層記憶スタック | loop-01 / loop-02 |
| **Tactical** (how) | NotebookLM + Claude Codeの比較活用パターン | Loop #5 / #7 |
| **Operational** (where) | Brain3 Vaultでの自動化・Wiki化フロー | LOOP#1-7 (実績) |

### LOOPの進化（第2回） — 最初の LOOP として構築、7回のLOOPを経て「統合設計書 v1」として再構成

```
ループ #1 → ループの定義       → loop-01 (Getting started with loops)
    └──→ ループ #7 → Wiki化パターン     → Loop-6 / loop-7
            ↓        Loop-3 (Claude Code 45タスク自動化)
                → LOOP #8 (この記事)      → 統合設計書 v1（全7記事の概念合成）
                    → LOOP #9          次回：loop #9でさらに洗練→「Brain3 Vault運用マニュアル」へ進化するはず
```

---

## G. Brain3 Vaultでの適用可能性（実装例一覧）

| loopで学んだことを | Brain3 Vaultでの具体例 |
|---|---|
| Prompt Exploring (loop-04) | loopプロンプト集(templates/)で「なぜわからないのか → どうすればわかるか」を自問 |
| 3層記憶スタック（ loop-02） | Obsidian(Vault×Hermes Agentの記憶→推論エンジン）→ AI操作員→ プロンプト |
| NotebookLM vs Claude Code (loop-05) | NotebookLMは「一次情報保管庫」、Claude Code/Hermesは分析・自動化ツール |
| 米国株BOTの実装例（ loop-06） | ルール→AI審査→実行 のパターンをBrain3 Vaultに適用可 |
| Loopプロンプトテンプレート（templates/） | ループ運用の「ループ#8→Wiki化 → _loop-memory に追記」フロー |

---

## H. Brain3 Vaultでの設計原則

### ルール：「AIが考える部分を人間が残す」という境界設計

```
┌─┐ ┌───────────┐  ┌────────────┐  ┌──────────┐
│人│→│What/Why    │  │ AIに任せる      │ →│結果の確認│→│人間の判断│
└─┘ └───────────┘  └────────────┘  └──────────┘

| ループの構造 | loop_01（基本定義） → loop-02（3層記憶） → loop-3（自動化パターン） |
   │
   ↓「統合設計書 v1として再構成」→ LOOP #8（この記事） 
```

### Brain3 Vault × AI活用 の5設計原則

| 原則 | 説明 | ループでの根拠 |
|---|---|---|
| **① Loopに「問いを出す」** | loopメモのLOOP#には「なぜわからないか」「どれが見えてないか」といった質問をする。答えではない | loop-04 (又吉式思考術) |
| **② Wikiを蓄積する** | raw素材 → Wiki化 → cross-link → 知識の地層 | loop-02 (3層記憶スタック) + LOOP #1-#8 |
| **③ ループに「壁打ち」をつける** | 「この仮説は正しいか？反対意見は？」をloop-04の「壁打ち5連コンボ」で自問し、LOOPメモに保存 | loop-05 (NotebookLM32活用法) |
| **④ 設計書はアップデートする** | LOOP #9以降も追加・再構成→「統合設計書 v2, v3…へ進化」 | LOOP #8の試み（記事A-Hに進化する） |
| **⑤ Brain3 Vaultが「記憶の地層」** | Obsidian Vault → AI操作員 → プロンプト（推論エンジン） — 記憶→処理→出力 のループ構造に適用。この循環が回るほど知識は蓄積していく | loop-02 (3層記憶スタック)|

---

## Brain3 × AI活用統合設計書 v1 LOOP#8

### LOOP #9への引き継ぎ情報

| 次回やるべきこと | 提案 |
|---|---|
| **raw_folder_watchers_design_v2に常駐設定を追記する** | Cronジョブで「every 6hにraw/inboxを監視し、新着があればloop #NとしてWiki化」を追加設計 |
