---
title: "Open Design vs Open CoDesign — Claude Designの月額課金を回避する2つのオープンソースAIデザインツール"
date: "2026-07-05T10:00:00+09:00"
updated: 2026-07-05
tags: [ai-design, open-source, Claude Design, Open Design, Open CoDesign, design-systems]
status: draft
type: article
source: https://note.com/aiwaribiki/n/n9a11917e9622 (より引用)
---

# Open Design vs Open CoDesign — Claude Designの月額課金を回避する2つのオープンソースAIデザインツール

最近話題の「Claude Design」を使いたいけど月額が高い…という人のために、ほぼ無料で使えるオープンソース版が2つ登場しました。それぞれの特徴と違いをまとめます。

---

## 1. ガチ勢向け：Open Design

**リポジトリ**: [https://github.com/nexu-io/open-design](https://github.com/nexu-io/open-design)

アプリというより「最強の型紙セット」といったイメージです。

- **推しポイント**: Notion風やStripe風など、おしゃれなデザインの型が70種類以上使い放題
- **使い方**: コードを書きながらプロ級のサイトを一瞬で作るツール
- **おすすめ**: 「中身のコードまでこだわりたい！」というエンジニア向け

### インストール手順（Open Design）

1. リポジトリをクローン — `git clone https://github.com/nexu-io/open-design`
2. 依存関係をインストール — `npm install` または `pnpm install`
3. 環境変数の設定 — `.env`ファイルに `ANTHROPIC_API_KEY` を貼り付け
4. 起動! — `pnpm dev` でローカルサーバーが立ち上がり、ブラウザで操作可能

**用途**: デサインは信頼重視か・コードも書きたい人
**特徴**: Stripe風（信頼・安心特化）のデザインに適している。白ベース＋余白多め、直線的・整列重視、無駄な装飾なし

---

## 2. 手軽さ重視：Open CoDesign

**リポジトリ**: [https://github.com/OpenCoworkAI/open-codesign](https://github.com/OpenCoworkAI/open-codesign)

PCにインストールして使うアプリ。一番のおすすめです！

- **推しポイント**: Claudeだけじゃなく、GPT-4やDeepSeekなど好きなAIを切り替えOK
- **コスパ最強**: 月額サブスクじゃなく、「使った分だけ」のAPI代で済む
- **おすすめ**: 「難しいことは抜きで、サクッとLPや図解をAIで作ってみたい！」人

### インストール手順（Open CoDesign）

1. **インストーラーをDL** — [GitHub Releasesページ](https://github.com/OpenCoworkAI/open-codesign) から、Macなら.dmg、Windowsなら.exeをダウンロード
2. **アプリを開く** — インストールして起動。スマホやタブレットのプレビュー画面が出てきたらOK
3. **APIキーを入力** — 設定画面から自分のAnthropic（またはOpenAI）のキーを入れるだけ

**用途**: デサインは感情重視か・アプリで楽したい人
**特徴**: Notion風（親しみ・継続性）のデザインに適している。柔らかいグレー、アイコン・余白バランス重視、情報がスッと入る

---

## 結局、どっちがいい？

| | Open Design | Open CoDesign |
|--|-------------|---------------|
| タイプ | GitHubソースのエディタ型 | デスクトップアプリ型 |
| コード編集 | ✅ 対応 | ❌ 非対応 |
| AIモデル切り替え | ❌ Claudeのみ | Claude / GPT-4 / DeepSeek |
| タンプレート数 | 70種以上 | おまかせ |
| クライアント | エンジニア | ワイヤー・デザイナー |

- **コードも書きたい!** → **Open Design**
- **アプリで楽したい**! → **Open CoDesign**

---

## デザイントemplateの使い分けガイド

主要3パターンをまず覚えればOKです。

### Stripe風（信頼・安心特化）
用途：金融 / BtoB / 高単価サービス
- 白ベース＋余白多め
- 直線的・整列重視、無駄な装飾なし
- **効果**: 「ちゃんとしてる感」が一瞬で伝わり、CV率+10〜20%改善しやすい領域

### Notion風（親しみ・継続性）
用途：ブログ / SNS / 教育 / 個人サービス
- やわらかいグレー、アイコン・余白バランス重視
- **効果**: 離脱率が下がり、滞在時間+20〜40%

### Linear風（先進・トレンド）
用途：SaaS / IT / スタートアップ
- ダーク＋グラデーション、ミニマル＋動き、洗練されたタイポグラフィ
- **効果**: 「新しい」「すごい」が直感で伝わり、クリック率+15〜25%

### 選び方の公式
```
信頼が必要 → Stripe系
共感が必要 → Notion系
差別化したい → Linear系
```

さらに精度をあげるなら：
- 高単価 × 法人 → Stripe固定
- 低単価 × 個人 → Notion寄せ
- 新規プロダクト → Linear寄せ

**NGパターン**: 金融なのにLinear（信頼崩壊）、個人ブログでStripe（冷たくて離脱）、SaaSでNotion（埋もれる）

---

## AIに「売るLP」を作らせる黄金プロンプト

このテンプレートをOpen CoDesignに流し込むだけで成約率が変わります。

```
あなたはUI/UXデザイナー兼コピーライターです。
以下の条件を満たす「ヒーローセクション」を設計してください。

# 1. ターゲット（具体化必須）
年齢：27〜29歳女性  職業：会社員（都内勤務）  年収：350〜500万円
性格：慎重・効率重視・SNSリテラシー高め

# 2. 悩み（1つに絞る）
「〇〇に時間がかかりすぎて疲れている」

# 3. ベネフィット（機能ではなく結果）
NG：機能説明 → OK：変化（〇〇が10分で終わる、迷わなくなる、ストレスが減る）

# 4. ヒーロー構成
① キャッチコピー（20文字前後・感情直撃）
② サブコピー（具体的ベネフィット）
③ 信頼補強（実績・数字・安心材料）
④ CTA（1つに絞る）
⑤ ビジュアル指示（写真 or イラストの具体内容）

# 5. コピー制約
抽象語禁止、必ず数値or比較を入れる、1文は最大40文字、「自分ごと化」優先

# 6. デザイン仕様（Notion風）
バック：ホワイト（#FFFFFF）、アクセント：グレー系、余白：広め（左右32px以上）
影：なし or 極小、角丸：6〜8px、装飾：最小限

# 7. レイアウト
PC：左テキスト／右ビジュアル  SP：縦積み（テキスト→CTA→画像）
```

---

## API従量課金で月額1/10に抑える術

月額3,000円のサブスクを払い続けるのはもったいない。APIを賢く使えば「月300円」で本家以上のクオリティが出せます。

**パターン1：LP生成（Open Design）**
```python
# Role: 世界トップクラスのUI/UXデザイナー
# Task: AIライティングアシスタント「EverWrite」のLPを生成
# Design System: Notion-like (Minimalist, Inter font, Gray-scale with blue accent)
# Layout: Modern SaaS Landing Page — Sticky Nav, Hero Section, Feature Grid...
```

**パターン2：ダッシュボード（Open CoDesign）**
```python
# Skill: Dashboard_Creation
# System: Stripe-Style (Professional, Subtle Shadows, Indigo accents)
# Goal: 個人開発者向け「収益分析ダッシュボード」のプロトタイプを作成
```

---

## クライアント提案に使える一言

| スタイル | 提案コピー |
|--|--|
| Stripe系 | 「信頼性を最優先に設計します。CVに直結します」 |
| Notion系 | 「ユーザーが迷わず読み進められる設計です」 |
| Linear系 | 「第一印象で"他と違う"と感じさせます」 |

---

## 参考元

- 出典: [note.com/aiwaribiki](https://note.com/aiwaribiki/n/n9a11917e9622) — わりびきちゃん
- Open Design リポジトリ: [github.com/nexu-io/open-design](https://github.com/nexu-io/open-design)
- Open CoDesign リポジトリ: [github.com/OpenCoworkAI/open-codesign](https://github.com/OpenCoworkAI/open-codesign)
