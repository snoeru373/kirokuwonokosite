---
title: "GitHub Pagesで無料アフィリエイトサイトを作る方法｜Ubuntu + MkDocs + 無料で始める実践手順"
date: "2026-06-28T13:00:00+09:00"
categories: [github-pages, マークダウン, アフィリエイト]
tags: [github-pages, mkdocs, ubuntu, localai, affiliate]
status: draft
affiliate: true
review_required: true
description: "50代がUbuntu上でMkDocs + GitHub Pagesを使って無料アフィリエイトサイトを構築した完全手順。WordPress不要・費用ゼロでの始め方を画像なしで解説します。"
---

# GitHub Pagesで無料アフィリエイトサイトを作る方法｜Ubuntu + MkDocsで無料で始める実践手順

※この記事には広告・アフィリエイトリンクを含む場合があります。

## はじめに：なぜGitHub Pagesなのか

ブログサービスというとWordPressやnoteを思い浮かべますが、それぞれデメリットもあります。

| サービス | メリット | デメリット |
|---------|---------|-----------|
| WordPress | テーマ豊富・プラグイン充実 | サーバ代・ドメイン代がかかる |
| note | 簡単投稿可能 | カスタマイズ性・SEOが弱い |
| **GitHub Pages** | **完全無料・独自ドメインも可** | コマンド操作が必要・デザインに限界 |

 Ubuntu + MkDocsを使えば、Markdown1本でサイト全体を管理できます。

---

## 必要なもの＆環境要件

最低限必要なものは次の4つだけです。

| 項目 | パス／URL |
|------|-----------|
| GitHubアカウント | https://github.com/ |
| Git（Ubuntu標準） | `git --version` で確認 |
| MkDocs + mkdocs-material | pip installで導入可 |
| Markdownエディタ | Obsidian or VS Code等 |

PCスペックは、ビルド時のみ1GBメモリがあれば十分です。

---

## ステップ① GitHubリポジトリを作成する

```bash
# 新規リポジトリ作成（ブラウザまたはgh CLI）
gh repo create my-affiliate-site --public
git remote add origin https://github.com/<ユーザ名>/my-affiliate-site.git
```

---

## ステップ② MkDocsプロジェクトを初期化する

```bash
cd ~ && mkdir mkdocs-site && cd mkdocs-site
python3 -m venv .venv && source .venv/bin/activate
pip install mkdocs mkdocs-material
mkdocs new docs/index.md
mkdocs serve -a 127.0.0.1:8084
```

---

## ステップ③ GitHub Pagesへ反映する手順

`mkdocs.yaml` の `site_url` を設定し、ビルド後 `ghp-import` または `git push` します。

```bash
pip install ghp-import
ghp-import -n -b gh-pages site/
git push origin gh-pages
```

これで公開URL（例：`https://<ユーザ名>.github.io/my-affiliate-site/`）で閲覧できます。

---

## つまずいたポイント①〜③

| 課題 | 回避策 |
|------|-------|
| MkDocs buildで警告が大量 | navに存在しないファイルへの参照を排除 |
| 画像が GitHub Pages で表示されない | `![](../images/foo.png)` の相対パス表記を確認 |
| Markdownの日本語タイトルが化ける | ファイル名と見出しは小文字英数字＋ハイフンで統一 |

---

## おすすめする人 / おすすめしない人

| ✅ おすすめ | ❌ 向かない |
|-----------|-----------|
| ローカルAI環境にUbuntuを使っている方 | Windows PowerToys で完結させたい方 |
| コマンド操作が少しできる方 | 「クリックだけですべて終わり」を望む方 |

---

## まとめ：最初の5ページができれば十分

無料アフィリエイトサイトを始める際、最初に作成すべきは以下の5ページです。

1. トップページ index.md
2. プロフィール profile.md
3. 記事例（3編程度）
4. 広告表記 disclosure.md
5. 参考ツール一覧 list.md

これらがあれば無料でスタートできます。収益が安定してからWordPressや独自ドメインを検討しましょう。

---

## FAQ

### Q1. ドメインを買わなくても使えるので？
はい。`.github.io` ドメインなら無料です。

### Q2. WordPressの移行は可能ですか？
可能です。HTMLエクスポート後に MkDocs に変換できます。

### Q3. 更新頻度はどのくらいがいいですか？
週1〜月2回程度を続けた方がサイト評価が安定します。

---

## 参考URL（確認済み）

| リンク | 内容 |
|-------|------|
| https://docs.github.com/ja/pages | GitHub Pages公式ガイド |
| https://www.mkdocs.org/ | MkDocs プロジェクト |
| https://squidfunk.github.io/mkdocs-material/ | Material テーマ |

---

> **広告**: この記事には広告・アフィリエイトリンクを含む場合があります。価格や仕様は変更されることがあります。必ず公式サイトで最新情報をご確認ください。
