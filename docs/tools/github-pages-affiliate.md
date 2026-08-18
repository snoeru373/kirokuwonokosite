---
title: GitHub Pages無料サイト作り方：アフィリエイトで使える静的ブログの構築方法
tags: [GitHub-Pages, MkDocs-Static-Site, 無料メディア]
date: 2026-08-02
status: published
description: GitHub PagesとMkDocsを使って、有料サーバーやドメインなしで技術ブログやアフィリエイトサイトを無料開設する方法をステップバイステップで解説します。
---

# GitHub Pagesで無料のウェブサイトを作る方法 {{< internal-link site-format-overview >}}

## 1. なぜGitHub Pagesなのか？
- **完全無料**：月額0円、ドメイン費も不要
- **検索流入が狙える**：Googleインデックスに載りやすい
- **自動HTTPS対応**：SSL証明書も標準で利用可能
- **MkDocsの簡単ビルド**：Markdownファイルを書くだけでサイト完成

## 2. MkDocs+Materialテーマの選び方
今回は「Material for MkDocs」を採用しました。理由：
1. デザインが洗練されている (無料でもプロ並みの見た目)
2. Pythonで書かれている → macOS/Linux/Windowsどれでもok
3. Materialテーマの日本語対応が良い

## 3. サイトの公開手順（全ステップ）

### Step 1: MkDocs インストール
```bash
pip install mkdocs-material
```

### Step 2: MkDocs プロジェクト作成
```bash
mkdocs new kirokuwonokosite --force
cd kirokuwonokosite
```

### Step 3: GitHubPages連携
- GitHubアカウントでリポジトリ「kirokuwonokosite」を作成
- MkDocsのビルド成果物をmainブランチに配置（`.nojekyll`必須）
- Pages Settingsから `Deploy from a branch → main / root` で公開

## 4. アフィリエイト記事を書く時の注意点
- 広告は本文ではなく、サイドバーか下部に集約させる (内部リンク：記事冒頭表記基準は公開設定待機中)
- 記事冒頭には必ず利用ツールの表記を入れる（AFO規格）
- GoogleサイトVerificationのmetaタグをHTMLヘッダに追加する

## まとめ
WordPressより簡単で、無料ですぐ始められます。公開手順の詳細は初期の記事10本ガイドを参照して！


---
## 公開前チェックリスト：
- [ ] MkDocs のビルド（mkdocs build）が成功しているか
- [ ] site_url、repo_urlに正しい値が入っているか
- [ ] ブラウザURLで実際にサイトが開くようになっているか
