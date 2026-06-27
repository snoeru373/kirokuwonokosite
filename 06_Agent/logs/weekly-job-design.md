---
title: "weekly job 設計案"
date: "2026-06-27T09:30:00+09:00"
status: design_draft
---

# weekly-reviews job 設計案（未実装）

## 目的
週1回、Google Search Console / Cloudflare Web Analytics / ASPレポートのCSVを読み込み、記事を分析する。

## cron job 設定例

```yaml
schedule: "0 9 * * 1"    # 毎週月曜日9:00 JST
prompt: |
  1. 05_Tracking/search-console/ にあるCSVファイルを読み込む
  2. 05_Tracking/cloudflare-analytics/ にあるCSVファイルを読み込む
  3. 05_Tracking/asp-reports/ にあるASPレポートを読み込む
  
  4. CTRが低い記事（表示回数>100且つCTR<2%）を抽出してリスト化
  5. 商品導線が弱い記事を抽出（CTAが弱いもの）
  
  6. 成果が出た記事の共通点を 06_Agent/memory/affiliate_learning_memory.md に追記
  
  7. 改善案 → drafts/refresh に「改善候補」としてリスト化
  ```

## CSV読み込みルール

| ファイル | フォーマット | 必須カラム |
|----------|-------------|-----------|
| search-console | CSV（Google Search Consoleエクスポート）| query, page, impressions, clicks, ctr, position |
| cloudflare-analytics | JSON / CSV | page, visitors, pageviews |
| asp-reports | CSV（ASPダウンロード形式） | product_id, clicks, conversions, revenue |

## 改善案の出力フォーマット

```markdown
## 週間改善レポート（2026-07-06〜2026-07-12）

### CTRが低い記事（表示回数>100, CTR<2%）
| タイトル | 表示回 | CTR | 平均順位 | 改善案 |
|----------|--------|--|---------|--------|
| ... | ... | ... | ... | タイトル差し替え |

### CTAが弱い記事（CTA数<2 / pageview比） 
| タイトル | 現在のCTA数 | 推奨CTA数 | 改善案 |
|----------|------------|-----------|--------|
| ... | ... | ... | 商品リンク追記 |

### affiliate_learning_memory.md に追付する成果共通点
- 「...」系の記事はCTRが高い → タグやタイトルにこのパターンを取り入れる
- 「...」のキーワードでは検索上位に上がりやすい

---

> ⚠ このレポートは自動生成ですが、改善案の実施は人間確認後に実施してください。
```

## 成果記録用メモリファイル仕様

`06_Agent/memory/affiliate_learning_memory.md` のフォーマット：

```markdown
# Affiliate Learning Memory

## 成果が出た記事の共通点（学習ログ）

### CTR高い記事パターン
- [2026-07-10] "50代からUbuntu..." — CTR 3.2%
  → 共通点: タイトルに「50代」「無料」「手順」が含まれている
  
### CTAが効いたパターン  
- [2026-07-14] "...ASP比較" — クリック数 15
  → 共通点: 表形式の比較 + 「今すぐ確認」CTA

## 成果が出ない記事（統合・リライト候補）
- ["..."] — Impressions 5, Clicks 0
  → リライト候補 or 他記事と統合を提案
  
---
最終更新: 2026-07-14
```
