---
title: "WordPressを使わず無料で副業サイトを作る方法｜Ubuntu + MkDocs で始める無料メディア運営"
date: "2026-06-28T14:00:00+09:00"
categories: [アフィリエイト, マークダウン, ubuntu]
tags: [wordpress, mkdocs, ubuntu, free, affiliate]
status: draft
affiliate: true
review_required: true
description: "WordPressを使わずにUbuntu上でMkDocsを使って無料副業サイトを作る方法。サーバー・ドメイン費用ゼロで始める実践的な手順を解説します。"
---

# WordPressを使わず無料で副業ブログを作る方法｜Ubuntu + MkDocs で始める実質コストZERO運営

※この記事には広告・アフィリエイトリンクを含む場合があります。

## はじめに：なぜWordPressではないのか

ブログといえば WordPress が定番ですが、有料サーバー代（月額数百〜2000円）やドメイン代（年額1000〜2000円）がかかります。

Ubuntu で MkDocs を使えば、Markdownファイルを書き込むだけでサイトが完成します。

- サークル代 ドメインは月500円以上
- WordPressのアップデート：手動 or プラグイン依存
- MkDocs での作業は `mkdocs build → github push` の2コマンドで完結

---

## Ubuntu + MkDocs でサイトを構築するまでの手順

### ステップ1: 環境整備 (約10分)

```bash
sudo apt update && sudo apt install -y python3-pip git
python3 -m venv ~/mkdocs-env && source ~/mkdocs-env/bin/activate
pip install mkdocs mkdocs-material
```

### ステップ2: MkDocsプロジェクトの初期化 (5分以内)

```bash
cd ~/Documents && mkdir my-blog && cd /my-blog
mkdocs new docs/index.md
echo "site_name: My Free Blog" > mkdocs.yaml
mkdocs serve -a 127.0.0.1:8084
```

### ステップ3: GitHub Pagesで公開 (10分以内)

1. GitHubに新しいリポジトリを作成する
2. `git remote add origin https://...` でリモート接続
3. MkDocs build した `site/` を gh-pages ブランチへpush

---

## コスト内訳（すべて無料）

| 要素 | 費用 |
|------|------|
| サーバー | ¥0 (GitHub Pages) |
| ドメイン | ¥0 (xxx.github.io) |
| MySQLやPHP | ¥0 不要 |
| SSL証明書 | ¥0 自動付与 |
| 月額合計 | **¥0** |

---

## つまずいたポイント①〜③

| 課題 | 回避策 |
|------|-------|
| MkDocs navでファイルが見つからない警告 | `mkdocs build --strict` で早期チェック |
| Markdownの画像パスが化ける | `![](../images/foo.png)` に絶対パス表記 |
| Windows WSL上でビルドエラーが発生 | venvを再作成し pip install mkdocs でやり直す |

---

## おすすめする人 / おすすめしない人

| ✅ おすすめ | ❌ 向かない |
|-----------|-----------|
| WordPressの初期投資を抑えたい方 | 「クリックで完了」型のブログが望む方 |
| UbuntuやLinuxに触れたことがある方 | 画像コンテンツ中心の運営を目指す方 |

---

## まとめ：最初からWordPressである必要はない

無料サイトを作るならUbuntu + MkDocs は充分実用性があります。

収益が見えてきた段階で、MkDocs → WordPress の移行も可能なので最初はコストゼロで始めましょう。

---

## FAQ

### Q1. WordPressとの違いは？
CMS不要・サーバー不要・無料が最大のメリットです。

### Q2. 有料テーマはどうなりますか？
MkDocs も mkdocs-material は無料のテーマでも十分実用レベルです。

---

## 参考URL（確認済）

| リンク | 内容 |
|-------|------|
| https://www.mkdocs.org/ | MkDocs |
| https://squidfunk.github.io/mkdocs-material/ | テーマ Material |

---

> **広告**: この記事には広告・アフィリエイトリンクを含む場合があります。価格や仕様は変更されることがあります。必ず公式サイトで最新情報をご確認ください。
