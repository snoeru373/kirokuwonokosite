---
title: ""Ubuntu環境構築セットアップ手順""
date: "2026-06-28T06:00:00+09:00"
status: archived_by_aging
tags:
  - " MkDocs導入"
  - " git"
  - " ubuntu"
  - " 初期設定"
  - "affiliate-free"
---


# 作業ディレクトリ作成

```bash
mkdir -p /home/ubun/Documents/data/affiliate-free-ops
cd /home/ubun/Documents/data/affiliate-free-ops
```

## サブフォルダ作成

```bash
mkdir -p 00_Inbox \
01_Research/{trends,products,asp,competitors} \
02_Articles/{drafts,review,published,refresh} \
03_Site/docs/{local-ai,ubuntu,hermes-agent,note-affiliate,tools} \
04_SNS/{x,note,youtube-shorts} \
05_Tracking/{search-console,cloudflare-analytics,asp-reports,weekly-reviews} \
06_Agent/{prompts,rules,logs,memory} \
99_Backup
```

## MkDocs導入

```bash
sudo apt update
sudo apt install -y python3-pip python3-venv git

python3 -m venv .venv
source .venv/bin/activate
pip install mkdocs mkdocs-material
```

## サイト初期化

```bash
cd 03_Site
mkdocs new .
```
