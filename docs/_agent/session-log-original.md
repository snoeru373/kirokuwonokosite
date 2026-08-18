# AFOブログ制作 作業記録（中断時点）

## セッション完了日
2026年7月16日 午後

## ✅ 完了事項
1. MkDocs Materialテーマでのサイト構築完了（白背景・黒文字・青リンク）
2. 全10記事のmdファイル作成＆配置（docs/blog/article-01〜10.md）
3. ナビゲーション（左サイドメニュー+右目次配置）article-04デザイン準拠に統一
4. index.html にカテゴリ別一覧＆全記事リンクを配置

## 📁 重要なファイルパス/構造
| パス | 内容 |
|------|------|
| `/home/ubun/Documents/data/affiliate-free-ops/mkdocs.yml` | ナビ設定・テーマ定義（Materialテーマ・左メニュー+右目次） |
| `mkdocs.yml` | navにローカルAI関連3本、サイト構築関連4本、失敗改善記録3本の10記事配置済み |
| `/home/ubun/Documents/data/affiliate-free-ops/docs/index.md` | トップページ（50代からのローカルAI実践ノート） |
| `/home/ubun/Documents/data/affiliate-free-ops/docs/blog/index.md` | ブログTOP・カテゴリ別記事リンク10本配置済み |
| `/home/ubun/Documents/data/affiliate-free-ops/docs/blog/article-XX-xxxx.md` | 各記事mdファイル10本（article-01〜10） |

## 🔗 ブラウザ確認用URL（file://）
```text
トップページ: file:///home/ubun/Documents/data/affiliate-free-ops/site/index.html  
ブログ一覧:   file:///home/ubun/Documents/data/affiliate-free-ops/site/blog/index.html
記事1本目:   file:///home/ubun/Documents/data/affiliate-free-ops/site/blog/article-01-50-kara-ubuntu.html
記事n番目:   file:///home/ubun/Documents/data/affiliate-free-ops/site/blog/article-0N-xxxxxx.html (02〜10)
```

## ⚠️ 次回再開手順
1. MkDocs再ビルド: `cd /home/ubun/Documents/data/affiliate-free-ops && mkdocs build --clean`
2. サーバー再起動: `lsof -ti:9505` でプロセス確認 → なければ `python3 -m http.server 9505 &`
3. URL貼付てブラウザで表示確認

## 備考
- ポート8083, 8021, 9500, 9505 はいずれも閉じている状態（電源OFFで消失）
- `file://` URLアクセスが最も確実な方法である
