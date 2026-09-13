---
title: "ブログ執筆ルール"
date: "2026-06-25T06:00:00+09:00"
status: active
tags:
  - " publishing"
  - " rules"
  - " wiki"
  - " writing-guidelines"
  - "blog"
---


# ブログ執筆時の注意点とルール

**最終更新日**: 2026-06-27  
**ステータス**: brain3 ルールファイル（全エージェント共通）

---

## はじめに

このファイルは、Hermes Agent がブログ記事を執筆する際に必ず確認するルール集です。
毎回の記事作成前にこれを参照してください。

---

## 1. ブログ記事の保存場所

| 操作 | パス |
|------|------|
| **draft ファイル** | `/home/ubun/Documents/data/affiliate-free-ops/02_Articles/drafts/` |
| **review フォルダ** | `/home/ubun/Documents/data/affiliate-free-ops/02_Articles/review/` |
| **published フォルダ** | `/home/ubun/Documents/data/affiliate-free-ops/02_Articles/published/` |
| **refresh フォルダ** | `/home/ubun/Documents/data/affiliate-free-ops/02_Articles/refresh/` |
| **MkDocs ドキュメント** | `/home/ubun/Documents/data/affiliate-free-ops/03_Site/docs/drafts/` |

---

## 2. 記事作成時の基本的な構造（必須）

| 項目 | 内容 |
|------|------|
| メタ情報（yaml front matter） | title, date, categories, tags, status, affiliate, review_required |
| タイトル | 「50代からUbuntuでローカルAI環境を作ってみた｜無料で始めるAI活用の第一歩」のような形式 |
| PR 表記（冒頭） | `※この記事には広告・アフィリエイトリンクを含む場合があります。` |
| はじめに／目的 | この記事の目的・対象読者を簡潔に記載 |
| 結論 | 記事を一言でまとめると？ |
| 記事内容 | 実体験をベースに、具体例と手順を交えて記載 |
| まとめ | 「もう一度言うと」という要約を入れる |
| FAQ | 3〜5問ほど入れる |

---

## 3. 広告・アフィリエイトルール（必須チェック）

### A8.net / もしも / 楽天共通

1. **PR 表記は必ず冒頭に入れる**
   ```markdown
   ※この記事には広告・アフィリエイトリンクを含む場合があります。
   ```

2. **禁止表現リスト**（絶対に使わない）
- [ ] 「必ず稼げる」「確実に儲かる」
- [ ] 「完全自動で収益化できる」
- [ ] 「誰でも簡単に月○万円」
- [ ] これだけで安心 / 完売必至 など断定的な表現
- [ ] 価格、在庫、キャンペーン内容の確定（公式以外では断定不可）

3. **記載すべき情報**
- [ ] ASP 名、案件名（審査通ってから）
- [ ] 広告リンク挿入位置
- [ ] 「最終確認日」を必ず記録
- [ ] 金額・仕様は公式 URL を引用して記載

---

## 4. ブログ記事作成時のチェックリスト

### 執筆前
| # | チェック項目 | やるべきこと |
|---|------------|-------------|
| 1 | ターゲット読者 | 誰に読んでほしいか明確に |
| 2 | 記事テーマ | Ubuntu / ローカル AI / Obsidian / etc. |
| 3 | ASP 案件との関連性 | 広告項目が記事内容と自然につながるか？ |

### 執筆後・公開前
| # | チェック項目 | やるべきこと |
|---|------------|-------------|
| 1 | PR 表記漏れ | 冒頭に入っているか確認 |
| 2 | 誇大表現 | 禁止表現リストと照合 |
| 3 | リンク切れ | ASP 公式から URL を直接コピー |
| 4 | 最新情報 | 価格・仕様は公式サイトで再確認 |
| 5 | 人間承認 | review フォルダに移動 → 人間が最終確認 |

---

## 5. ブログと MkDocs サイトの連携

### MkDocs ビルドコマンド
```bash
cd /home/ubun/Documents/data/affiliate-free-ops/03_Site && \
source ../.venv/bin/activate && \
mkdocs build
```

### プレビュー起動コマンド
```bash
mkdocs serve -a 127.0.0.1:8083
```

### MkDocs config ファイル
- `/home/ubun/Documents/data/affiliate-free-ops/03_Site/mkdocs.yaml`
  - site_name, theme (material), nav, plugins: search

---

## 6. Brain3 Vault との同期ルール

| 操作 | パス |
|------|------|
| ブログ MD → Brain3/raw/inbox | コピー or git push（同期用） |
| rules / prompts → Brain3/_agent/| cp して sync |

**注意**: ブログと Brain3 は別ルートです。両方を更新してください。

---

## 7. ブログ記事に含めるべきセクション（テンプレート必須項目）

1. 【結論】最初に結論を入れる
2. この記事で分かること - 3つくらい箇条書き
3. 実際に試した体験を踏まえた説明
4. 失敗談や注意点
5. おすすめする人 / おすすめしない人
6. FAQ
7. 参考 URL（公式、実検証者リンク）
8. 広告表記（冒頭に必ず）

---

## 8. ブログ記事の更新記録ファイル名フォーマット

| パターン | 説明 |
|----------|------|
| blog-YYYYMMDD-01.md | 今日の日付 + シーケンス番号 |
| article-NN-nickname.md | 「article」プレフィックスありで管理 |

今日からのブログ記事は以下のパスへ配置してください：
```
/home/ubun/Documents/AI-Operations/Brain3/raw/blog/blog-20260627-{n}.md
```

---

## 9. ブログ記事作成時の注意点（エージェントへの指示）

1. **架空の価格・仕様・URL を書かない** — 「公式サイトで確認」を記載する
2. **架空の収益数字を出さない**
3. **架空の体験談は使わない** — 実際の体験に基づく内容のみ
4. **金融、医療、健康ジャンルは初期収益化対象外**
5. **記事削除は禁止** — リライトまたは統合で対応
6. **レビュー状態から published 状態へ遷移するのは人間のみ**

---

## 10. ASP 登録前の必須条件

| 条件 | 現状 | 必要アクション |
|------|------|---------------|
| トップページ | ✅ あり (index.md) | — |
| プロフィールページ | ✅ あり (profile.md) | — |
| 広告表記ページ | ✅ あり (disclosure.md) | — |
| 記事が3本以上 | ⚠️ article-01,02 充実（2本）、他はテンプレート | 少なくとも(article-05 or 6)を本格化 |
| 外部サイトからの評価 | △ まだ Google Search Console なし | 公開後に登録 |

---

## 11. ブログ記事に含めるべきメタ情報（yaml front matter）

```yaml
title: "記事タイトル"
date: "2026-06-27THH:MM:SS+09:00"
categories: [カテゴリ名]
tags: [タグ1, タグ2, タグ3]
status: draft|review|published
affiliate: true/false
review_required: true
description: "記事の概要（検索エンジン用）"
---
```

---

## エージェントへの実行指示

このファイルを読み込んでからブログ記事を執筆してください。
メタ情報・PR 表記・禁止表現チェックを必ず実行し、公開前に review フォルダへ移動すること。
