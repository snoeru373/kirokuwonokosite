---
title: "GitHub Pagesで無料アフィリエイトサイトをつくる手順（MkDocs + Material）"
date: "2026-06-27T14:00:00+09:00"
updated: 2026-06-27
tags: [GitHub Pages, MkDocs, 無料ブログ]
status: draft
type: article
affiliate: false
review_required: true
last_verified: ---
published_url: ---
target_keywords: ["github pages 無料 ブログ", "mkdocs material theme"]
---

# 【4】タイトル（記事構成案）

**タイトル**: 「GitHub Pagesで無料アフィリエイトサイトをつくる手順（MkDocs + Material）」  
**スラッグ**: `github-pages-free-affiliate-site`  
**ステータス**: `draft` (下書き)  
**アフィリエイトリンク有無**: なし

---

## 💡 この記事を公開する理由（筆者視点）
> **狙いキーワード**: github pages 無料 ブログ, mkdocs material theme
> **検索意図**: GitHub Pagesで無料でサイトを作りたい。具体的な手順を知りたい。
> **差別化**: MkDocs + Materialテーマのセットアップ手順を50代向けに詳細解説

---

## TL;DR（結論）
> （ここに50字以内の簡潔な結論を後で入れる）

## 読者の悩み
1. ブログサイトを作りたいが、サーバー代がかかるのは敷居が高い
2. WordPressは設定が複雑そう
3. GitHub Pagesは本当に無料なのか？ドメインは？

## ターゲットユーザー
| ユーザー層 | こんな悩みを持っている |
|------------|------------------------|
| プログラミング未経験者 | コードを書くのが心配 |
| 50代の方 | 新しい技術用語がたくさんで混乱する |
| 副業始めたい方 | 初期投資ゼロで始めたい |

## 実体験

### 背景と動機
- GitHub Pagesの存在は知っていたが、敷居が高いと思っていた
- MkDocsを知り、Markdownだけで書けることに気づいた
- 最初のコミットまでにかかった時間：○時間

### やってみた結果
- 良い点: 完全無料、Git管理できる、カスタマイズ性が高い
- 不満だった点: カスタムドメインは有料、動的機能は使えない

### ハマりポイント①〜③
- ハマリ1: GitHub CLIの設定で詰まった
- ハミリ2: MkDocsテーマのインストールエラー
- ハミリ3: ビルド後のプレビュー確認方法がわからない

## やり方・手順の詳細

### ステップ1 — GitHub Pagesリポジトリの作成

```bash
gh repo create ai-practical-note --description="50代からのローカルAI実践ノート" --public
cd ai-practical-note
mkdocs new .
```

### ステップ2 — MkDocsのインストールと設定

```bash
pip install mkdocs mkdocs-material
mkdocs serve
```

### ステップ3 — 記事をコミットして公開

```bash
git add .
git commit -m "Initial commit"
gh pages upload --directory site/
```

## おすすめする人 / おすすめしない人

| | 詳細 |
|-|------|
| ✅ **おすすめ** | Markdownで書けるなら無料でサイトが作れる |
| ❌ **おすすめしない** | グラフィックを多用したい方 |

## FAQ

### Q1. GitHub Pagesは本当に無料ですか？
→ A1: はい、個人利用の場合は永久に無料です。

## まとめ（最後に一言）


> **広告**: この記事には広告・アフィリエイトリンクを含む場合があります。
