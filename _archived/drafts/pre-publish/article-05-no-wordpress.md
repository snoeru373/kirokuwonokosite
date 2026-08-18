---
title: "WordPressを使わず無料で副業サイトを作る方法 — 全部無料ツールで完結する実例"
date: "2026-07-12T15:00:00+09:00"
updated: "2026-07-12"
categories: [副業, ブログ運営, 無料ブログ]
tags: [wordpress-madan, 無料副業サイト, mkdocs-free, no-cost-site]
status: draft
affiliate: true
review_required: true
description: "レンタルサーバーもWordPressもしない。全部無料ツールでアフィリエイトサイトの構築と維持を完結する方法を実体験ベースに解説します。初期投資¥0で始められます。"
---

# WordPressを使わず無料で副業サイトを作る方法 — 全部無料ツールで完結する実例

※この記事には広告・アフィリエイトリンクを含む場合があります。

## はじめに：WordPressの敷居の高さを知った日から

私が最初にブログを立ち上げようとした時、まずWordPressを選びました。でも「サーバー契約 → ドメイン取得 → SSL設定 → インストール」「プラグイン選定」。50歳の私にはハードルが高すぎるのです。しかも月¥1,000〜 ¥3,000のサーバー代。

**「もっと安く気軽に始められる方法はないのか？」** その問いから、全部無料で完結する方法を探し続けました。そして辿着したのは、MkDocs + GitHub Pages + Obsidianという3点セットです。

この記事では、WordPressに頼らず無料でアフィリエイトサイトを作る方法を詳しく説明します。「なぜこれで十分なのか」も解説します。

---

## TL;DR（結論）

> WordPressより安い、メンテナンス無料のアフィリエイトサイトを構築する方法は
> MkDocs × GitHub Pages + Obsidianの3点です。サーバー代¥0・ドメインなしで始められ、Gitで管理できるので安心。WordPressのプラグインに依存しないシンプルさが長寿サイトを作るコツです。

## なぜWordPressをあえて選ばなかったのか（私の体験）

### WordPressを使う場合のコスト（最低ライン）

| 項目 | コスト/月 |
|------|----------|
| レンタルサーバー | ¥500〜¥1,500 |
| ドメイン | ¥80〜¥130（年払いの場合） |
| SSL証明書 | ¥0（Let's Encrypt無料） |
| サードパーティープラグイン | ¥0〜¥ Thousands/月 |

**最初の年に最低 ¥6,000〜¥18,000 もかかります。** 私はこの金額で「収益が少しでも出るのであれば」と考えましたが、実際にWordPressをインストールした段階で、次の問題に直面しました。

### WordPressと私のミスマッチ

1. **更新が複雑** - プラグインのアップデートで画面が消える（実際にはデータベース互換性エラー）、テーマの変更にまたコストがかかる
2. **バックアップ取らないと危険** - 公式バックアップは有料。自力でFTPで取るのはめんどくさい（正直に言います）
3. **重い** - 大量のPHPプロセスが裏側で動くので、低スペック環境ではストレス

---

## 無料ツールで完結する4つの構成要素

### 1. MkDocs（サイトビルダー）— ¥0

[https://www.mkdocs.org/](https://www.mkdocs.org/) で公開されているPython製静的サイトジェネレーター。Markdownファイル → HTMLに変換してくれます。

**メリット:**
- Python + pipで一発インストール
- Materialテーマを使えばデザイン不要
- ドキュメント構築もできるからブログにも使える

### 2. MkDocs-Material（テーマ）— ¥0

[https://squidfunk.github.io/mkdocs-material/](https://squidfunk.github.io/mkdocs-material/) で公開されているMaterial Designベースのテーマ。これがサイト全体のデザインを担います。

**メリット:**
- ダークモード対応あり
- 検索機能も最初から搭載（プラグイン不要）
- GitHubとGoogle Analyticsとの連携も簡単に

### 3. GitHub Pages（ホスティング）— ¥0

[https://pages.github.com/](https://pages.github.com/) で公開されているGitHubの静的ホスティングサービス。git pushで自動的にページが更新されます。

**メリット:**
- 永久無料、HTTPS対応済み
- CI/CDパイプラインも無料で使える（GitHub Actions）

### 4. Obsidian（執筆・整理）— ¥0

[https://obsidian.md/](https://obsidian.md/) はMarkdownファイルの可視化と検索に最適化されたメモアプリ。

**メリット:**
- Markdownで執筆しながら確認できるのが大きい
- プラグインでダッシュボード作成も可能

---

## 無料アフィリエイトサイト運営の流れ（全体像）

```
1. Obsidianで下書きを書く（raw/blog下にmarkdownを配置）
   ↓
2. MkDocsでビルド（mkdocs build → docs/blog/に出力）
   ↓
3. GitHub Pagesにpushして自動公開
   ↓
4. 毎週の日曜日にサイト全体の確認と改善案のレビュー
```

**重要**: コスト ¥0ということは、**すべてが自分の責任**である。バックアップは自分でとる（ObsidianのSync or Git clone）。サーバー停止リスクもないが、GitHubのTerms of Serviceに反するコンテンツを置くのは禁止です。

---

## 実際の初期構築手順（50代向け詳細版）

### ステップ①：PCの準備
Ubuntu 24.04環境が必要です。私のマシンは：
- GMKtec EVO-X2 AI (96GB RAM / 48GB VRAM)
- Ubuntu 24.04 LTS

Windows or Macでもできますが、Python + pip + venvがインストールされている必要があります（OS標準パッケージマネージャを使ったらまず入るはずです）。

### ステップ②：レポジトリの作成
GitHubにログイン → New Repository → `my-free-site` と名前を付けてPublicでNew。

```bash
cd ~/Documents
git clone https://github.com/YOUR_USERNAME/my-free-site.git
cd my-free-site
pip install mkdocs mkdocs-material
mkdocs new .
```

これで初回のindex.mdとmkdocs.yamlが自動生成されます。

### ステップ③：プレビューしてみる
```bash
source .venv/bin/activate && mkdocs serve
```

http://127.0.0.1:8000 をブラウザで開けば、Materialテーマのダッシュが確認できます。

### ステップ④：記事を公開する流れ（以後はこのサイクル）
```bash
# 記事を書く（MkDocs/docs/blog/に配置）
nano docs/blog/article-05.md

# mkdocs.yamlを更新してnavに追加

mkdocs serve          ← ブラウザで確認してから
git add . && git commit -m "新記事追加" && git push origin main
```

---

## ハマりポイント（実際の失敗体験）

### ハマリ①：テーマのインストール漏れでサイトが真っ白に

`pip install mkdocs-material` を忘れた状態でもmkdocs buildは成功します。しかし、ページは空白です。「バグなのか？」と1時間格闘しました。mkdocs.yamlに `theme: name: material` と書くだけではダメ。pipでインストールする必要があります。

### ハマリ②：nav配列のインデントミス → mkdocs build失敗

MkDocsのmkdocs.yamlにナビゲーション項目を10個以上追加した時、半角スペース4つが5つになってしまい「bad indentation of a mapping entry」でコケました。これは何時間格闘したか……私はyamllintを使い始めてから解決しました。**yamlは厳格です**。

### ハマリ③：GitHub Pagesにpushしても反映されない

最初にgit pushしてhttp://username.github.io/my-free-site/openしたら、まだindex.mdが表示されていなくて、"Oops, something's wrong"が出ました。原因はmain branchではなくmaster branchだったこと。**gh CLIでレポジトリを作るとmainになるので**。これは後からmkdocs.ymlに `site_url` を追加したことで解消されました。---

## おすすめする人 / おすすめしない人

| | 詳細 |
|-|------|
| ✅ **おすすめ** | WordPressより軽く無料で運用したい方、Markdownが少し使える方、長期運営を目指している方 |
| ❌ **おすすめしない** | 完全ドラッグ＆ドロップでデザインしたい方、PHPやMySQLを覚えたい方、「WordPressが標準」と思っている方 |

---

## まとめ：WordPressに頼らない方が長寿サイトになる

無料ツール構成の一番の利点は、「**壊れない**」ことです。WordPressと違ってデータベースもないし、サーバーも停止しない（GitHub Pagesなので）。MkDocs + Materialテーマは安定して動作し続けます。

50代からのアフィリエイト運用において重要なのは「継続力」。高コストの WordPressに縛られると続けるのが大変ですが、無料ツール構成なら毎月の負担がありません。**最初の2周間は面倒でも、その先10年同じサイトを運営するという視点を持てば、選択はシンプルです。**

---

## FAQ

### Q1. WordPressより機能が少ないのは困りませんか？

はい、プラグインが限定的なのは事実。ただし「ブログ記事を配信する」という目的なら十分です。WordPressのテーマカスタマイズに費やす時間とコストを考えれば、MkDocsで十分でしょう。

### Q2. 検索エンジンにSEO対策は必要ですか？

必要ですが、GitHub Pagesの場合はmetaタグの追加やsitemap生成が容易です。MkDocs-Materialには組み込みでSEOメタフィールドがあります（site_description, theme.paletteなど）。

### Q3. WordPressと両立できますか？

できます。最初からWordPressにするのはリスクが高いので、まずは無料で始めて収益が見えてきたら段階的に移行していくのが現実的です。

---

## 参考URL

| リソース | URL |
|---------|-----|
| MkDocs公式サイト | https://www.mkdocs.org/ |
| MkDocs-Materialテーマ | https://squidfunk.github.io/mkdocs-material/ |
| GitHub Pagesドキュメント | https://pages.github.com/ |

> **広告**: この記事には広告・アフィリエイトリンクを含む場合があります。価格、仕様、利用条件は変更される場合があります。必ず公式サイトで最新情報をご確認ください。
