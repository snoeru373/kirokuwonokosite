---
title: "公開ルール v2"
date: "2026-06-25T06:00:00+09:00"
status: active
tags:
  - " automation"
  - " rules"
  - " wiki"
  - " workflow"
  - "publishing"
---


# 公開ルール

## 自動処理OK
- 記事のドラフト作成
- `drafts` → `review` への移動
- データファイル(CSV等)のInboxへの配置
- バックアップの作成

## human_review_required（禁止事項）

| 操作 | ルール |
|------|--------|
| 初回アフィリエイトリンク挿入 | ❌ 必ず人間確認 |
| article公開 | ❌ 人間の最終確認後に実行 |
| article削除 | ❌ 絶対禁止 |
| title変更 | ❌ 人間の確認後に実行 |
| URL変更 | ❌ 人間の確認後に実行 |
| CTA(誘導文)大規模追加 | ❌ 人間の確認後に実行 |

## 例外（自動化可能）
- typos修正のみで内容変更なし
- 既存リンク先URLの定期更新(301確認後)
- 広告表記の追記のみ

## 承認フロー
```
draft → (agent自動処理) → review → (human承認) → published
                                              ↕
                                          refresh → human承認 → published
```
