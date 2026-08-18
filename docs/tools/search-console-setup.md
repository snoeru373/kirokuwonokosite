---
tags: [search-console, seo, sitemap, github-pages]
date: 2026-06-28
---

# Google Search Console登録手順

## サイト情報

| 項目 | 値 |
|------|-----|
| サイトURL | https://snoeru373.github.io/affiliate-free-ops/ |
| sitemap URL | https://snoeru373.github.io/affiliate-free-ops/sitemap.xml |
| 公開方法 | GitHub Pages（静的サイト） |

## 登録手順（ブラウザで実施）

1. Google Search Console にアクセス
   - URL: https://search.google.com/search-console
   
2. 「プロパティを追加」をクリック
   - リアルタイムデータを使う場合は **URLプレフィックス** を選択
   - 入力: `https://snoeru373.github.io/affiliate-free-ops/`

3. ドメイン所有を確認
   - ファイルアップロード方式を選択
   - HTMLファイルをルートに配置（gh_pages_site/直下に設置）

4. sitemapを送信
   - URL: https://snoeru373.github.io/affiliate-free-ops/sitemap.xml

5. 完了後、インデックス登録とエラー確認を定期的に行う

## 注意点

- GitHub PagesではCNAME設定が必要（独自ドメインの場合）
- sitemap.xmlはmkdocsビルド時に自動生成される
- HTTPS必須（GitHub Pages標準）
