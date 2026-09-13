---
title: "MkDocs.yml設定ファイル例"
date: "2026-06-28T06:00:00+09:00"
status: active
tags:
  - " MkDocs"
  - " mkdocs-yml"
  - " 設定ファイル"
  - "AI活用"
  - "affiliate-free"
---

status: active

# 15. 無料版の mkdocs.yml 例

```yaml
site_name: 50代からのローカルAI実践ノート
site_description: Ubuntu、ローカルAI、Hermes Agent、Obsidian、無料アフィリエイト運用の実践記録
site_url: https://snoeru373.github.io/affiliate-free-ops/

theme:
  name: material
  language: ja

nav:
  - ホーム: index.md
  - Ubuntu:
      - UbuntuでAI環境を作る: ubuntu/local-ai-ubuntu.md
  - ローカルAI:
      - OllamaとOpen WebUI: local-ai/ollama-openwebui.md
  - Hermes Agent:
      - Obsidian外部脳化: hermes-agent/obsidian-brain.md
  - 無料サイト運営:
      - GitHub Pagesで無料サイト: tools/github-pages-affiliate.md
  - 広告について:
      - 広告・アフィリエイト表記: disclosure.md

plugins:
  - search

markdown_extensions:
  - admonition
  - tables
  - toc:
      permalink: true
```
