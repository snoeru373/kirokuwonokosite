---
title: "Hermes AgentでObsidianを外部脳化する手順｜Ubuntu上のAI自動化活用"
description: "Ubuntu上でのObsidian活用を、Hermes Agentの自動化と組み合わせる手順を解説します。外部脳化でAIとの会話を記録し、記事や知識資産に変える方法。"
publishedAt: 2026-07-18T15:00:00+09:00
status: draft
tags: [Hermes Agent, Obsidian, ローカルAI, Ubuntu, 自動化]
affiliate: true
review_required: true
---

# Hermes AgentでObsidianを外部脳化する手順｜Ubuntu上のAI自動化活用

※この記事には広告・アフィリエイトリンクを含む場合があります。

## はじめに：AIとの会話を「残す」ということ

AIを活用していると、便利な反面「その場限り」の問題に直面します。

* AIの回答を見て「なるほど」と思う
* 実際に試してみる
* でも数日後にその内容を探しても見つからない
* どのコマンドを実行したのか忘れてしまった
* なぜその設定にしたのか思い出せない

AIは素晴らしい道具ですが、**「使った記録」** を一緒に管理しないと、知識として蓄積できません。

そこで活用するのがObsidianとHermes Agentです。

ObsidianはMarkdownベースのノートアプリで、ローカルファイルとして情報を保存できます。

Hermes AgentはUbuntu上で動くエージェント（タスク自動化ツール）で、これらのファイルを自動処理・改善できます。

この2つを組み合わせることで、**AIとの会話、作業記録、調査メモを「外部脳」として残す** 仕組みを作れます。

---

## TL;DR（結論）

> ObsidianにMarkdownでメモを残し、Hermes Agentで自動処理・改善するだけで、AIの知識を資産化できる。
> 
> 手順は「Obsidianインストール → フォルダ構成決定 → プロンプト準備 → 自動化」の4ステップ。
> 
> コマンド操作も基本不要（または簡単なターミナル操作のみ）。

---

## なぜObsidian＋Hermes Agentなのか

| 方法 | メリット | デメリット |
|------|----------|-----------|
| Obsidian単体 | ローカル管理・検索強い | 手動整理が必要 |
| AIチャット画面 | 会話できる | 記録が散在する |
| **Obsidian＋Hermes Agent** | **自動処理＋構造化** | 初期設定の手間 |

私の場合、Ubuntuにインストールして使っています（Mac版もある）。Markdownファイルとして保存されるので、Gitでバージョン管理も可能です。

---

## 実際の構築手順

### ステップ1：Obsidianの準備

まず[obsidian.md](https://obsidian.md) から最新版をダウンロード・インストールします。

Linux (Ubuntu) の場合は Flatpak でも配布されています。

```bash
# flatpak経由でインストールする場合
flatpak install flathub md.obsidian.Obsidian
```

### ステップ2：Vault（フォルダ）の作成

Obsidianを開き、「Create new vault」を選択。

名前例: `affiliate-free-ops-vault`

保存先はUbuntuなら以下を推奨：

```text
/home/ubun/Documents/AI-Operations/Brain3/
```

このフォルダがObsidian上のすべてのファイル（.md）の保管場所になります。

### ステップ3：フォルダ構成の設定

Obsidian内には以下の構造を作ります：

```text
affiliate-free-ops-vault/
├── 00_Inbox/     # とりあえず何でも入れる
├── 01_Research/  # 調査メモ（ASP、ツール比較）
├── 02_Articles/  # 記事下書き
├── 03_SiteDocs/  # GitHub Pages用ファイル
├── 04_SNS/       # note、X用原稿
├── 05_Tracking/  # Search Console、ASPレポート
└── 06_Agent/     # Hermes Agentルール・ログ
```

### ステップ4：Hermes Agentとの接続準備

Ubuntuターミナルで作業ルートを確認：

```bash
cd /home/ubun/Documents/data/affiliate-free-ops/
tree -L 1
```

Obsidianとこのルートを連携させます。例えば、`02_Articles/drafts/」に書いたものはObsidian上から見放題になります。

---

## MkDocs.yamlの設定（カスタマイズ）

ObsidianでMarkdownファイルを作成し、MkDocsでサイト化する設定です：

```yaml
site_name: 50代からのローカルAI実践ノート
nav:
  - ホーム: index.md
  - ブログ記事一覧:
    - Ubuntu環境構築: drafts/article-01-50-kara-ubuntu.md
    - Ollama＆Open WebUI: drafts/article-02-ollama-open-webui.md
    - Obsidian外部脳化: drafts/article-03-hermes-agent-ob.md
```

---

## メモの基本テンプレート（自動生成）

Obsidianに保存する際、このフォーマットに従うと後でHermes Agentが楽です：

````markdown
---
title: "[タイトル]"
date: "2026-07-18T15:00:00+09:00"
tags: [タグ, タグ]
status: draft
type: memo
source: hermes-agent
---

## 目的（What）
このメモを書いた理由。

## やったこと（How）
やった手順のステップバイステップ。

## つまずき（Why not working)
うまくいかなかったポイント。

## 結果（Result）
最終的にどうなったか。

## 記事化候補
この内容から記事にできるかもしれないポイント。
````

---

## ハマりポイント

### ハマリ①：フォルダ構成を迷う

Obsidian内には多数のファイルが溜まります。最初のフォルダ構成で失敗すると、後で整理しきれなくなります。

私の場合は「`tree -L 2」で構造を可視化した後、整理しました。

### ハマリ②：ファイル名にスペースや日本語を入れたら表示が崩れる

最初は日本語タイトル（例: `AIの調べもの.md）を入れました。しかしMkDocs側でリンク切れが起きました。

**解決策：** ファイル名は英数字＋ハイフンに統一（例: `ai-no-shirabe.md`).

---

## おすすめする人 / おすすめしない人

||
|------|
| |Obsidianを使っている人、自動化に関心のある方|
| ❌ **おすすめしない** | 手動整理を避けたい方、コマンドラインに不安がある方|

---

## まとめ：AI知識の蓄積は「外部脳＋自動処理」

Obsidianに記録し、Hermes Agentがそれを処理する。このサイクルが回ると:

```text
1. AIチャット → 2. Obsidian保存 → 3. Hermes Agent分析 → 4. MkDocsサイト化
```

最初のステップ（Obsidianインストール＋フォルダ作成）は比較的簡単です。

その後もファイル単位で管理できるので、拡張性に優れています。

アフィリエイト運用において重要なのは「継続的な改善」です。記録がないと改善できません。この方法を早く始めればそれだけ資産化されます。

---

## FAQ

### Q1. Obsidianは無料ですか？

デスクトップ版・Android版は無料です（iOS版有料）。

### Q2. MkDocsとObsidianの連携方法教えてください。

両方ともMarkdownファイルを扱うので、同じフォルダを共有するだけです。

参考URL：
| リソース | URL |
---------|-----|
| Obsidian公式サイト | https://obsidian.md/ |
| MkDocs公式ドキュメント | https://www.mkdocs.org/ |
> **注**: この記事には広告・アフィリエイトリンクを含む場合があります。仕様や利用条件は変更される場合がありますので、必ず公式サイトで最新情報をご確認ください。
