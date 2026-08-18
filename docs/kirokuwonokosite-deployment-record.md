---
tags: [github-pages, mkdocs-material, kirokuwonokosite, 公開リポジトリ]
date: 2026-08-02
---

# GitHub Pages 公開記録 (kirokuwonokosite)

## サイト情報
- **URL:** https://snoeru373.github.io/kirokuwonokosite/
- **リポジトリ:** snoeru373/kirokuwonokosite
- **ブランチ:** main
- **ファイル構造:** / (root) ディレクトリ
- **テーマ:** MkDocs Material
- **公開日:** 2026-08-02

## 運用ルール
1. **MkDocs ビルド:** `mkdocs build` で `site/` に生成
2. **GitHub Pages:** main ブランチの root を source に設定
3. **.nojekyll: MkDocs のビルド成果物に `.nojekyll` が含まれていることを確認
4. **PAT 認証:** PAT は環境変数に保存し、`.netrc` か `.git-credentials` に格納
5. **公開手順:** ブラウザの Settings → Pages で Source を `main` / folder `/` に設定

## 技術メモ
- MkDocs build の結果は `docs/` 配下の md ファイルから生成される
- GitHub Pages は Jekyll ベースのため、MkDocs 成果物は `.nojekyll` ファイルで Jekyll ビルドを抑制する必要がある
- site_url を mkdocs.yml で正しく設定すること
- gh-pages-src / は一時作業用ディレクトリ

## URL ルール
- MkDocs build の結果は `site/` に出力される
- GitHub Pages で公開する場合は、これを直接 root に配置（ドキュメント直下）にマウント
- URL 構造とファイル構成の整合性に注意（例：article.md → /article/index.html）

## 将来の更新手順
1. `docs/` 配下に新しい md ファイルを追加 or 既存ファイルを更新
2. `mkdocs build` を実行して html を生成
3. `cp -r site/* gh-pages-src/.gitkeep` してコンテンツを同期
4. git add, commit, push (origin main)

## パス
- MkDocs ルート: /home/ubun/Documents/data/affiliate-free-ops/gh_pages_site/docs/
- build 成果物: /home/ubun/Documents/data/affiliate-free-ops/gh-pages-src/site.yml`で更新する必要がある点注意
