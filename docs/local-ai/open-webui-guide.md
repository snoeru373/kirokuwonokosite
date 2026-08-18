---
title: Open WebUI完全ガイド：ローカルAIの操作画面を無料で構築する
tags: [Open-WebUI, ローカルAI, Ollama, GUI]
date: 2026-08-02
status: published
description: Dockerで簡単に導入できるOpen WebUIの設定・操作方法を解説。Ubuntuにインストールして、Ollamaと連動させてローカルAIチャット環境を作る方法を一気通貫で紹介します。
---

# Open WebUI完全ガイド：ローカルAIの操作画面を無料で構築する {{< internal-link article-02-ollama-open-webui >}}

## 1. なぜOpen WebUIなのか？
Ollamaはコマンドラインで使うことが多いため、50代の方にはハードルが高く感じられます。Open WebUIは、まるでChatGPTのような使いやすい画面を、ローカルのUbuntu上で構築できる無料ツールです。

## 2. Open WebUIのインストール手順

### Step 1: Dockerのインストール
```bash
sudo apt update && sudo apt install -y docker.io docker-compose
```

### Step 2: Open WebUIの起動
以下のコマンドを実行すると、自動的にOpen WebUIが起動します：
```bash
docker run -d --name webui -p 3000:8080 --add-host=host.docker.internal:host-gateway ghcr.io/open-webui/open-webui:main
```

これで `http://localhost:3000` からログイン可能になります。

## 3. Ollamaとの連携設定
Open WebUIにログイン後、[Admin Settings]>[Connections]>[Ollama] の順に選択し、以下の情報を設定します：
- Base URL: `http://host.docker.internal:11434` (Mac/Windows) / `http://localhost:11434` (Linux native)
- モデルを選択（例: llama3.2）

これで、Ubuntuの画面にChatGPTのようなチャット機能が完成します！

## 4. Open WebUIとChatGPTの違い
| 項目 | Open WebUI | ChatGPT |
|------|------------|---------|
| コスト | 無料 | $20/月 |
| プライバシー | ◎ ローカル完結 | △ 外部送信 |
| カスタマイズ | 自由 | × 制限あり |

## まとめ
Open WebUIを使えば、月額コストゼロでChatGPTそっくりの操作画面が手に入ります。Ubuntuの設定さえできれば誰でも可能です。詳しくは[UbuntuでのローカルAIセットアップ記事](../ubuntu/local-ai-ubuntu.md)でも解説しています！


---
## 公開前チェックリスト：
- [ ] コマンドや手順が実際に動いて確認できているか
- [ ] Dockerのインストールが初めての人でも理解できる表現か
