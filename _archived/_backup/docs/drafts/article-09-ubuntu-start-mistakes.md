---
title: "Ubuntu初心者がAI環境構築でつまずいたこと10選 — 50代の実体験ベース"
date: "2026-06-28T18:00:00+09:00"
categories: [ubuntu, ローカルai, tsuzuita]
tags: [ubuntu, localai, beginner, troubleshooting, ai-setup]
status: draft
affiliate: true
review_required: true
description: "50代がUbuntuでローカルAI環境を構築する中で実際に起きたつまずき10選を、トラブルシューティングと回避策つきで解説します。"
---

# Ubuntu初心者がAI環境構築でつまずいたこと10選 — 50代の実体験ベース

※この記事には広告・アフィリエイトリンクを含む場合があります。

## はじめに

UbuntuはLinuxの一種で、ローカルLLMを無料で動かすのに最適なOSです。しかし、WindowsやmacOSに慣れた初心者がUbuntuに触れると、いくつもの壁にぶつかります。

私の場合（50代、GMKtec EVO-X2 AI + Ubuntu 24.04 LTS）、次の10個のつまずきがありました。それぞれ実体験からまとめています。

---

## つまずき①：コマンドの意味を忘れてしまった

AIに言われたコマンドを実行していると、その場ではできますが、「なぜこれを打ったのか」がすぐ分からなくなります。

**回避策**: コマンド実行ごとにメモを残す（Obsidianやmarkdownファイルへ）。

```bash
# 例: 'ollama pull llama2' は何をしたか
# → AIモデル"Llama2"をローカルにダウンロードしました
ls -la /usr/local/ollama/models/
```

---

## つまずき②：パッケージの依存関係エラー

`apt install ollama` を実行しましたが、他のパッケージとの依存関係でインストールに失敗しました。

**回避策**: 公式サイトの手順（curlスクリプト）を使うのが確実でした。

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

---

## つまずき③: モデルのVRAM不足

48GB VRAM (GMKtec EVO-X2 AI) あっても、70B以上の巨大モデルを動かそうとするとメモリエラーで失敗しました。

**回避策**: まずq4_K_M（量子化済み7B〜9B）から段階的に上げていく。

---

## つまずき④ : Open WebUIがOllamaに接続できない

初回起動時にOpen WebUIが`Connection refused`のエラーを返しました。

**回避策**: `OLLAMA_HOST=http://localhost:11434` をenvironment変数に設定し直して再起動するとつながりました。

---

## つまずき⑤：ファイル名の日本語とパーミッション問題Obsidianでmarkdownを表示できなかったのは、ファイル名が全角日本語やパス表記がおかしかったからです。さらにsudoを使わなくてよいようにchownで自分のユーザーへ移動しました。

**回避策**: ファイル名は小文字英数字+ハイフンに統一。ディレクトリ権限も確認する。

```bash
chown -R $USER:$USER ~/Documents/AI-Operations/
chmod -R u+rwx ~/Documents/AI-Operations/Brain3/
```

---

## つまずき⑥：git pushで認証エラーが出たGitにpushしようとしたら「Authentication failed」になりました。

**回避策**: SSHキーを生成してGitHubに登録しました。パスワードではなくSSH鍵方式を使います。

```bash
ssh-keygen -t ed25519 && cat ~/.ssh/id_ed25519.pub
```

---

## つまずき⑦: MkDocsビルドでnavエラーが出るmkdocs.yamlファイルのナビゲーション設定をymlリスト形式に直したらwarning-freeになりました。辞書形式（`key: value` だけのネスト）が許されません。

**回避策**: `nav:` キー配下は全て `- キー：value` インデント付きリスタ形式。

---

## つまずき⑧：ポートの競合（8084や8083番がすでに使われているなど）

mkdocs serve の再起動時、既に別のプロセスが同じポートにバインドしていたため接続拒否エラーが出ました。

**回避策**: ss -tlnp でポートを確認し、空いているポートへ再指定する。

```bash
ss -tlnp | grep ':80[0-9][0-9]'  # ポート確認用
```

---

## つまずき⑨：MarkdownとGitHub同期のタイミング問題Obsidian Vaultで書いた内容がGitHubへ反映されないことがありました。なぜかsync（git push）の手順を完全に踏んでいなかったためです。

**回避策**: Brain3/Vault側の変更は必ず`cd ~/AI-Operations/Brain3 && git add . && git commit -m "update" && git push origin main`を徹底。

---

## つまずき⑩：ASP登録審査で落ちた（媒体が未熟だった）A8.net・もしもアフィリエイト等では、登録前に最低3編程度の記事、プロフィールページ、広告表記ページの用意が必要でした。

**回避策**: 上記5ページ（index, profile, disclosure + articles x3）をまず完成させてから審査申請しました。

---

## まとめ：AIに聞きながら進めよう！

UbuntuとローカルAIの構築は初心者にはハードルが高いですが、Hermes AgentやChatGPTといったAIエージェントに問いかけながら一つずつ解決していけば、思ったよりスムーズに進みます。

私のやり方の基本は「分からない→AIに聞く→コマンドを打つ→メモする→次に進む」です。これを繰り返すうちに環境が完成しました。

---

## FAQ

### Q1. UbuntuとWindowsの併用は可能ですか？
はい、WSL2を使うことで両方使えます。

### Q2. AIエージェントがすべてのトラブルを解決してくれますか？
AIに答えを教えてもらえますが、最終判断（確認・理解）は自分で行うべきです。

### Q3. 50代の初心者がUbuntuでローカルLLMはできますか？
できます！ただし、時間と根気が必要です。私は50代でも成功しました。

> **広告**: この記事には広告・アフィリエイトリンクを含む場合があります。価格や仕様は変更されることがあります。必ず公式サイトで最新情報をご確認ください。
