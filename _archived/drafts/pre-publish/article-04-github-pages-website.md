---
title: "GitHub Pagesで無料アフィリエイトサイトをつくる手順（MkDocs + Material）"
date: "2026-07-12T13:00:00+09:00"
updated: 2026-07-12
categories: [GitHub Pages, MkDocs, ブログ運営]
tags: [github-pages, mkdocs, material-theme, 無料ブログ, site-construction]
status: draft
affiliate: true
review_required: true
description: "50代でも実践可能な、GitHub Pages × MkDocs × Materialテーマを使った無料アフィリエイトサイトの構築手順を段階的に解説します。サーバー不要・ドメイン不要で始められます。"
---

# GitHub Pagesで無料アフィリエイトサイトをつくる手順（MkDocs + Material）

※この記事には広告・アフィリエイトリンクを含む場合があります。

## はじめに：サーバー代0円でブログサイトを立ち上げたい

WordPressを使おうと思ったら、まずレンタルサーバーを契約し、ドメインを買って、SSL証明書を設定して……と、初期費用だけで数万円かかることがあります。「もっと簡単に安く始められないか」と思っていた私にとって、GitHub Pages + MkDocsの組み合わせは朗報でした。

**完全無料、Markdownだけで書ける、Gitでバージョン管理できる。** こういう理由で、私はアフィリエイト用の公式サイトをGitHub Pagesに構築しました。この記事で手順を一つずつ説明します。

---

## TL;DR（結論）

> GitHub Pages（無料ホスティング）× MkDocs（サイトビルドツール）× Materialテーマを使えば、サーバー代0円・ドメインなしでプロfefessionalなブログサイトを無料で作れる。
> 
> 手順は「レポジトリ作成 → Python環境構築 → MkDocsインストール → 記事執筆 → コミット＆公開」の4ステップ。

---

## なぜGitHub Pagesを選んだのか

アフィリエイトサイトを作る場合、以下の選択肢があります。

| 方法 | コスト/年 | 技術力 | メリット | デメリット |
|------|----------|--------|---------|-----------|
| WordPress（レンタル） | ¥15,000〜 | △ | プラグイン豊富 | コストがかかる |
| note.com | ¥0 | ○ | とにかく簡単 | グラフィックの制約 |
| **GitHub Pages + MkDocs** | **¥0** | △ | 無料＋Git管理 | コマンドライン必須 |

私の選択は「コストをかけたくないのでGitHub Pages」でした。ただし、Linux（Ubuntu）環境が既にあるのでコマンドラインにも抵抗はありません。もしWindowsのみで Linuxが全く触れたことがない場合は、記事-02の手順から始めることをお勧めします。

---

## 実際の構築手順

### ステップ1：GitHubレポジトリの作成

まず [github.com](https://github.com) にアカウントを作成（無料）したら、新しいレポジトリを作ります。

```bash
# GitHub CLIを使えば一行でレポジトリ作成（gh コマンドが必要）
gh repo create ai-practical-note --public --description="50代からのローカルAI実践ノート"
cd ai-practical-note
```

`--public` を忘れると公開できないので注意してください。これで https://username.github.io/ai-practical-note としてサイトが立つ準備が整います。

### ステップ2：Python環境の構築

MkDocsはPython製です。Ubuntuなら通常python3が入っています。

```bash
# Python バージョン確認
python3 --version

# virtualenvを作成して隔離（重要！）
cd ai-practical-note
python3 -m venv .venv
source .venv/bin/activate
```

### ステップ3：MkDocsとMaterialテーマのインストール

```bash
pip install mkdocs mkdocs-material
mkdocs new .
```

`mkdocs new .` で `docs/index.md` と `mkdocs.yaml` が自動生成されます。この状態ですでにサイトが動きます。

### ステップ4：プレビュー確認

```bash
mkdocs serve
```

ブラウザで http://127.0.0.1:8000 を開くと、Materialテーマのダッシュボードが表示されます。「やった！」という瞬間です。Ctrl+C で止めます。

---

## MkDocs.yamlの設定（カスタマイズ）

デフォルトの `mkdocs.yaml` はそのままでも十分ですが、サイト名やナビゲーションを設定しましょう。以下が私の設定例です。

```yaml
site_name: 50代からのローカルAI実践ノート
site_description: Ubuntu × ローカルAI × Obsidianの実践記録とアフィリエイト活用
theme:
  name: material
  palette:
    scheme: slate
    primary: deep purple
    accent: indigo

nav:
  - ホーム: index.md
  - ブログ記事:
      - UbuntuでのローカルAI体験: blog/article-01.md
      - OllamaとOpen WebUIでできること: blog/article-02.md
      - Hermes AgentでObsidianを外部脳に: blog/article-03.md
  - アフィリエイト比較: comparison.md
  - 管理人プロフィール: profile.md
```

---

## 記事をGitHub Pagesに反映させる手順

MkDocsの面白いところは、「Markdownファイルを書く → プレビュー確認 →コミット → GitHubで公開」というフローです。

```bash
# 新しい記事を作成
cp docs/index.md "docs/blog/article-04-github-pages-site.md"

# mkdocs.yamlを更新（navに追加）

# ローカルプレビュー
mkdocs serve -a 127.0.0.1:8003

# ブラウザで確認し、問題なければコミット＆push
git add .
git commit -m "記事-04を追加：GitHub Pages構築手順"
git push origin main
```

これで https://username.github.io/ai-practical-note/ に自動的に反映されます。

---

## ハマりポイント（実際の失敗体験）

### ハマリ①：mkdocs.yamlのnav配列でインデントミス

最初の設定時、ナビゲーションに10記事以上を追加しようとして、yamlのインデントを間違えたら `mkdocs build` が「bad indentation of a mapping entry」でコケました。これは何時間格闘したか……**yamlでは半角スペース4つごとに一段階**と厳格です。ツールとしてはyamllintを使うのがおすすめ。

### ハマリ②：mkdocs new の存在を知らなかったこと

最初は mkdocs.yamlを手書きで作りました。`mkdocs new .` を使えば `index.md` と YAMLファイルが自動生成されるのに気づかず、2時間無駄にしました。まずこれを使おう。

### ハマリ✗3：Custom Domain（独自ドメイン）は有料と知った時

GitHub Pages自体は無料ですが、カスタムドメインは別途Domain登録会社で取得する必要があります（年 ¥1,000〜）。最初は `username.github.io/your-repo-name` で十分だったので問題なし。有料ドメインを取得するのは収益が見えてからです。

---

## おすすめする人 / おすすめしない人

| | 詳細 |
|-|------|
| ✅ **おすすめ** | Markdownが書ける方、Gitの基本概念を知っている方、サーバー管理の手間をかけたくない方 |
| ❌ **おすすめしない** | グラフィック・ビジュアル中心のサイトを作りたい方、コマンドラインに不安がある方（→note.comから始めよう） |

---

## まとめ：無料でサイトを作るならMkPages + MkDocs一択

最初の手順こそコマンドラインが必要ですが、一度セットアップしてしまえば、**Markdownを書く → git push → 自動公開**のサイクルが回ります。このサイクルの中で、記事数を増やすだけがブログ運営です。

アフィリエイト運用において重要なのは「サイトがあること」ではありません。「継続的に更新し続けること」。そのための環境を無料・低負担で提供してくれるGitHub + MkDocsは素晴らしい組み合わせだと思います。

---

## FAQ

### Q1. GitHub Pagesは本当に永久に無料ですか？

はい、個人の用途では永久に無料です。ビジネス用途でも制限付きで使えますが、トラフィックが大きくなると制限がかかる可能性があります。

### Q2. WordPressより劣っているところがありますか？

はい。WordPressには数百〜数千のプラグインがありますが、MkDocsのプラグインは限定的です。「機能拡張したい → プラグインを探す」ができなくなります。だからシンプルにブログ記事を配信する用途に適しています。

### Q3. ドメインなしでURLが長いけど大丈夫ですか？

大丈夫です。最初は `username.github.io/your-repo` で十分。収益が見えてきたら独自ドメインを取ればOK。

---

## 参考URL（確認済み）

| リソース | URL |
|---------|-----|
| MkDocs公式サイト | https://www.mkdocs.org/ |
| MkDocs-Materialテーマ | https://squidfunk.github.io/mkdocs-material/ |
| GitHub Pagesドキュメント | https://pages.github.com/ |
| Git CLIドキュメント | https://git-scm.com/doc |

> **広告**: この記事には広告・アフィリエイトリンクを含む場合があります。価格、仕様、利用条件は変更される場合があります。必ず公式サイトで最新情報をご確認ください。
