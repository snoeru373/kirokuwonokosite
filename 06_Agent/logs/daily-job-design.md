---
title: "daily job 設計案"
date: "2026-06-27T09:00:00+09:00"
status: design_draft
---

# daily job 設計案（未実装）

## 目的
毎日記事をチェックし、下書きの品質向上を検索性データに基づいて自動化する。

## cron job 設定例

```yaml
schedule: "0 8 * * *"    # 毎日9:00 JST
prompt: |
  1. 02_Articles/drafts/ から未レビュー記事を読み込む
  2. 各記事のPR表記・広告表記が正しいか確認する
  3. target_keywordsに一致する検索結果をGoogle Trendsで抽出（可能なら）
  4. 問題があれば drafts に .checklist.md ファイルを作成して報告
```

## 想定される出力先
- `06_Agent/logs/daily-report.log`: dailyタスクのエグゼキューションログ

## 考慮事項
- daily jobは下書きの品質チェックのみ。公開・削除はしない。
- external API（Google Trends他）の利用が制約となるため、可能な範囲で実施。無料APIに限定する。
