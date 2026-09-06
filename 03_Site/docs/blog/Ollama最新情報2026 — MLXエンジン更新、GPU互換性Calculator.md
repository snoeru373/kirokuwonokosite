---
created: 2026-07-06 15:00(Asia/Tokyo)
updated: 2026-07-06 15:00(Asia/Tokyo)
title: "Ollama最新情報(2026年6月〜) — MLXエンジン更新、GPU互換性Calculator"
source: https://ollama.com/blog
author: "[[Ollama Team]]"
published: 2026-06-11
type: article
status: active
tags:
  - Ollama
  - MLX
  - GPU互換性
  - ROCm
  - AMD
---

## Ollamaブログ(2026年6月11日) の主要更新

### MLXエンジン最高パフォーマンス(Apple Silicon)

- Apple Silicon向けMLXエンジンを最新アップデートで **最大の性能** を達成
- モデル出力品質が向上
- Metal GPUオフロードをマルチモーダルモデルにサポート拡大

### GPU互換性Calculatorの新機能

- 最新のApple GPUモデルをサポート
- RAMとCPU計算アルゴリズムを更新
- AMD GPUのVRAM相当するNVIDIA GPU参照可能

### NVIDIA GPUサポート要件

- compute capability **5.0+**
- ドライババージョン **550以降**

## OllamaとAMD GPUs

- ROCmプラットフォームを介してOllamaがAMD GPUsをサポート
- AMD Radeon 32GB+ GPUでローカルAIワークロード可能
- BIOSアップデートからAMDファームウェア更新で性能アップ

### Windows上でのOllama + AMD GPU(2026年6月21日)

- カスタムビルドガイドが公開(680M/780M/890M対応)
- ROCm不要でも動作するケースあり

## Brain3 Vaultへの示唆

本環境(GMKtec EVO-X2 + RTX5090 32GB):

- NVIDIA GPU(compute capability 9.0+)なので **完全サポート**
- Ollamaが最適な推論ランタイムとして確立
- CUDAバックエンドの最適化が最大限活きる
